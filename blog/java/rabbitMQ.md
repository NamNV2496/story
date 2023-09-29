# RabbitMQ
## 1. RabbitMQ

## Giới thiệu
RabbitMQ là một message broker ( message-oriented middleware) sử dụng giao thức AMQP - Advanced Message Queue Protocol (Đây là giao thức phổ biến, thực tế rabbitmq hỗ trợ nhiều giao thức). RabbitMQ được lập trình bằng ngôn ngữ Erlang. RabbitMQ cung cấp cho lập trình viên một phương tiện trung gian để giao tiếp giữa nhiều thành phần trong một hệ thống lớn ( Ví dụ openstack - Một công nghệ rất thú vị hi vọng một ngày nào đó tôi đủ sức để viết vài bài về chủ đề này ). RabbitMQ sẽ nhận message đến từ các thành phần khác nhau trong hệ thống, lưu trữ chúng an toàn trước khi đẩy đến đích.

Thực sự, với lập trình viên thì rabbitmq rất đáng giá. Nếu không có các hệ thống message broker như rabbitmq thì bất cứ lúc nào cần đẩy data giữa các thành phần trong hệ thống, lập trình viên cần một kết nối trực tiếp. Một hệ thống càng lớn. Số thành phần càng nhiều, mức độ trao đổi message giữa càng thành phần cũng vì thế tăng lên khiến việc lập trình trở nên phức tạp. Tôi từng đọc vài bài báo về lập trình thì thấy họ khuyến cáo các lập trình viên chỉ nên tập trung vào business logic của ứng dụng còn các công tác hậu trường thì nên được tái sử dụng các giải pháp đã có. Rabbitmq cũng là một giải pháp rất tốt trong các kiến trúc hệ thống lớn.

## Tại sao lại sử dụng RabbitMQ
Chúng ta thử xem các message broker như rabbitmq đem lại lợi ích gì trong việc thiết kế ứng dụng. Trong một hệ thống phân tán (distributed system), có rất nhiều thành phần. Nếu muốn các thành phần này giao tiếp được với nhau thì chúng phải biết nhau. Nhưng điều này gây rắc rối cho việc viết code. Một thành phần phải biết quá nhiều đâm ra rất khó maintain, debug. Giải pháp ở đây là thay vì các liên kết trực tiếp, khiến các thành phần phải biết nhau thì sử dụng một liên kết trung gian qua một message broker. Với sự tham gia của message broker thì producer sẽ không hề biết consumer. Nó chỉ việc gửi message đến các queue trong message broker. Consumer chỉ việc đăng ký nhận message từ các queue này.

Tất nhiên, có thể có một giải pháp là sử dụng database để lưu các message trong các temporary table. Tuy nhiên xét về hiệu năng thì không thể bằng message broker vì một số lý do: Tần xuất trao đổi message cao sẽ làm tăng load của database, giảm performance đáng kể. Trong môi trường multithread, database cần có cơ chế lock. Lock cũng làm giảm performance. Sử dụng message broker sẽ không có vấn đề này.

Vì producer nói chuyện với consumer trung gian qua message broker nên dù producer và consumer có khác biệt nhau về ngôn ngữ thì giao tiếp vẫn thành công. Dù viết bằng java, python, php hay ruby... thì chỉ cần thỏa mãn giao thức với message broker thì thông suốt hết. HIện nay, rabbitmq cũng đã cung cấp client library cho khá nhiều các ngôn ngữ rồi. Tính năng này cho phép tích hợp hệ thống linh hoạt.

Một đặc tính của rabbitmq là asynchronous. Producer không thể biết khi nào message đến được consumer hay khi nào message được consumer xử lý xong. Đối với producer, đẩy message đến message broker là xong việc. Consumer sẽ lấy message về khi nó muốn. Đặc tính này có thể được tận dụng để xây dựng các hệ thống lưu trữ và xử lý log. ELK stack - Elasticsearch Logstash Kibana là một ví dụ. Đây là hệ thống được sử dụng trong môi trường DevOps khá hiệu quả. Giải quyết bài toán chia sẻ log của ứng dụng cho dev. Log được đẩy vào message broker rồi qua logstash lưu trữ vào elastic để đánh index. Sau đó index được kibana (một web interface) sử dụng để thực hiện truy vấn và hiển thị kết quả

<img src="blog/java/img/rabbitMQ1.png" style="display: block; margin-right: auto; margin-left: auto;">

Bản thân redis cũng có thể đảm nhận vai trò của message broker ( Dù cho mục đích ban đầu của nó không phải vậy. Đây là tính năng mở rộng sau này của redis ) Mô hình trên bạn có thể hoàn toàn thay redis queue bằng rabbitmq.

Bên cạnh các lợi ích kể trên, rabbitmq còn có nhiều tính năng thú vị khác như:

cluster: các bạn có thể gom nhiều rabbitmq instance vào một cluster. Một queue được đinh nghĩa trên một instance khi đó đều có thể truy xuất từ các instance còn lại. Có thể tận dụng để làm load balancing (tuy rằng có hạn chế, tôi sẽ nói sau)
high availibilty: cho phép failover khi sử dụng mirror queue.
reliability: có cơ chế ack để đảm bảo message được nhận bởi consumer đã được xử lý.

## Những khái niệm cơ bản trong RabbitMQ

- **Producer**: Ứng dụng gửi message.
- **Consumer**: Ứng dụng nhận message.
- **Queue**: Lưu trữ messages.
- **Message**: Thông tin truyền từ Producer đến Consumer qua RabbitMQ.
- **Connection**: Một kết nối TCP giữa ứng dụng và RabbitMQ broker.
- **Channel**: Một kết nối ảo trong một Connection. Việc publishing hoặc consuming từ một queue đều được thực hiện trên channel.
- **Exchange**: Là nơi nhận message được publish từ Producer và đẩy chúng vào queue dựa vào quy tắc của từng loại Exchange. Để nhận được message, queue phải được nằm trong ít nhất 1 Exchange.
- **Binding**: Đảm nhận nhiệm vụ liên kết giữa Exchange và Queue.
- **Routing** key: Một key mà Exchange dựa vào đó để quyết định cách để định tuyến message đến queue. Có thể hiểu nôm na, Routing key là địa chỉ dành cho message.
- **AMQP**: Giao thức Advance Message Queuing Protocol, là giao thức truyền message trong RabbitMQ.
- **User**: Để có thể truy cập vào RabbitMQ, chúng ta phải có username và password. Trong RabbitMQ, mỗi user được chỉ định với một quyền hạn nào đó. User có thể được phân quyền đặc biệt cho một Vhost nào đó.
- **Virtual host/Vhost**: Cung cấp những cách riêng biệt để các ứng dụng dùng chung một RabbitMQ instance. Những user khác nhau có thể có các quyền khác nhau đối với vhost khác nhau. Queue và Exchange có thể được tạo, vì vậy chúng chỉ tồn tại trong một vhost.

<img src="blog/java/img/rabbitMQ4.png" style="display: block; margin-right: auto; margin-left: auto;">

# Các loại Exchange

## Direct Exchange

Direct Exchange vận chuyển message đến queue dựa vào routing key. Thường được sử dụng cho việc định tuyến tin nhắn unicast-đơn hướng (mặc dù nó có thể sử dụng cho định tuyến multicast-đa hướng). Các bước định tuyến message:

- Một queue được ràng buộc với một direct exchange bởi một routing key K.
- Khi có một message mới với routing key R đến direct exchange. Message sẽ được chuyển tới queue đó nếu R=K.

<img src="blog/java/img/rabbitMQ5.png" style="display: block; margin-right: auto; margin-left: auto;">

## Default Exchange
Mỗi một exchange đều được đặt một tên không trùng nhau, default exchange bản chất là một direct exchange nhưng không có tên (string rỗng). Nó có một thuộc tính đặc biệt làm cho nó rất hữu ích cho các ứng dụng đơn giản: mọi queue được tạo sẽ tự động được liên kết với nó bằng một routing key giống như tên queue.

Ví dụ, nếu bạn tạo ra 1 queue với tên "hello-world", RabbitMQ broker sẽ tự động binding default exchange đến queue "hello-word" với routing key "hello-world".

## Fanout Exchange
Fanout exchange định tuyến message tới tất cả queue mà nó bao quanh, routing key bị bỏ qua. Giả sử, nếu nó N queue được bao quanh bởi một Fanout exchange, khi một message mới published, exchange sẽ vận chuyển message đó tới tất cả N queues. Fanout exchange được sử dụng cho định tuyến thông điệp broadcast - quảng bá.

## Topic Exchange

Topic exchange định tuyến message tới một hoặc nhiều queue dựa trên sự trùng khớp giữa routing key và pattern. Topic exchange thường sử dụng để thực hiện định tuyến thông điệp multicast. Ví dụ một vài trường hợp sử dụng:

- Phân phối dữ liệu liên quan đến vị trí địa lý cụ thể.
- Xử lý tác vụ nền được thực hiện bởi nhiều workers, mỗi công việc có khả năng xử lý các nhóm tác vụ cụ thể.
- Cập nhật tin tức liên quan đến phân loại hoặc gắn thẻ (ví dụ: chỉ dành cho một môn thể thao hoặc đội cụ thể).
- Điều phối các dịch vụ của các loại khác nhau trong cloud

## Headers Exchange
Header exchange được thiết kế để định tuyến với nhiều thuộc tính, đễ dàng thực hiện dưới dạng tiêu đề của message hơn là routing key. Header exchange bỏ đi routing key mà thay vào đó định tuyến dựa trên header của message. Trường hợp này, broker cần một hoặc nhiều thông tin từ application developer, cụ thể là, nên quan tâm đến những tin nhắn với tiêu đề nào phù hợp hoặc tất cả chúng.


# 2.1 Mô hình

<img src="blog/java/img/rabbitMQ2.png" style="display: block; margin-right: auto; margin-left: auto;">

Có thể hiểu message broker gần như bưu điện. Site A theo cách gọi của rabbitmq là producer (người gửi thông điệp). Site B và Site C theo cách gọi của rabbitmq là consumer (người nhận thông điệp). Producer connect đến message broker để đẩy message. Message sẽ đi qua message broker để đến được consumer. Cấu trúc của message broker chỉ gồm hai phần exchange và queue.

Exchange có nhiều loại. Trong hình vẽ trên exchange type là fanout. Lựa chọn các exchange type khác nhau sẽ dẫn đến khác đối xử khác nhau của message broker với thông điệp nhận được từ producer. Exchange được bind (liên kêt) đến một số queue nhất định. Với exchange type là fanout, message sẽ được broadcast đến các queue được bind với exchange. Consumer sẽ connect đến message broker để lấy message từ các queue.

Cài đặt và cấu hình cơ bản
Các bạn có thể tham khảo hướng dẫn trực tiếp từ trang chủ của rabbitmq:
[Download link](https://www.rabbitmq.com/install-rpm.html)

Vì rabbitmq được lập trình bằng erlang nên bạn cần cài đặt erlang trước. Tôi cài đặt erlang từ epel repo:

    wget http://download.fedoraproject.org/pub/epel/6/i386/epel-release-6-8.noarch.rpm
    rpm -ivh epel-release-6-8.noarch.rpm
    yum install --enablerepo=epel erlang

Sau đó, tôi cài tiếp rabbitmq:

    wget https://www.rabbitmq.com/releases/rabbitmq-server/v3.5.2/rabbitmq-server-3.5.2-1.noarch.rpm
    rpm --import https://www.rabbitmq.com/rabbitmq-signing-key-public.asc
    rpm -ivh rabbitmq-server-3.5.2-1.noarch.rpm

VIệc cài đặt đến đây là xong. Bạn có thể start thử service của rabbitmq bằng cách:
    
    service rabbitmq-server start

Cấu hình mẫu của rabbitmq đi kèm gói cài đặt nằm ở:
`/usr/share/doc/rabbitmq-server-3.5.2/`
Tôi copy về khu vực đặt file cấu hình của rabbitmq rồi đổi tên:

    cp /usr/share/doc/rabbitmq-server-3.5.2/rabbitmq.config.example /etc/rabbitmq/
    mv /etc/rabbitmq/rabbitmq.config.example  /etc/rabbitmq/rabbitmq.config

Cấu hình của rabbitmq mặc định chạy khá ổn. Các bạn có thể sử dụng luôn mà không cần bận tâm tùy chỉnh cấu hình.

## 2.2 Cài đặt management plugin

Rabbitmq có đi kèm một plugin cho phép quản trị hoạt động qua một web interface trông rất trực quan và thân thiện. Nhưng mặc định, plugin này không được enable. Để enable, bạn thực hiện lệnh sau:

    rabbitmq-plugins enable rabbitmq_management

Tất cả các hoạt động quản trị qua web cũng có thể thực hiện qua một command line tool có tên là rabbitmqadmin. Tool này đi kèm trong management plugin. Bạn cũng chỉ có thể sử dụng nó sau khi đã enable management plugin.

Cách download rabbitmqadmin tool:

Tôi cài đặt rabbitmq trên một server có ip `192.168.3.252` nên địa chỉ download sẽ là:

    http://192.168.3.252:15672/cli/rabbitmqadmin

Cách sử dụng tool này trong các tình huống cụ thể sẽ nằm trong các phần tới


# 3. Setup

## run on docker

    docker run -d -p 5672:5672 -p 15672:15672 --name my-rabbit rabbitmq:3.7-management

link: `http://localhost:15672`
login: username: guest password:guest

<img src="blog/java/img/rabbitMQ3.png" style="display: block; margin-right: auto; margin-left: auto;">

pom.xml

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-amqp</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

application.properties

```yml
    spring.rabbitmq.host=${RABBITMQ_HOST}
    spring.rabbitmq.port=${RABBITMQ_PORT}
    spring.rabbitmq.username=${RABBITMQ_USERNAME}
    spring.rabbitmq.password=${RABBITMQ_PASSWORD}
    spring.rabbitmq.virtual-host=${RABBITMQ_VHOST}

    rabbitmq.queue=MessageQueue
    rabbitmq.exchange=exchange
    rabbitmq.routingkey=routekey
```

Tạo file `RabbitMQConfig.java` để cấu hình routing và listener:

## **Consumer config**

```java
@Configuration
public class ConfigReceive {

    @Bean
    Queue queue() {
        return new Queue(QUEUE_NAME, false);
    }

    @Bean
    TopicExchange exchange() {
        return new TopicExchange(EXCHANGE_NAME);
    }

    @Bean
    Binding binding(Queue queue, TopicExchange exchange) {
        return BindingBuilder.bind(queue).to(exchange).with(ROUTING_KEY);
    }
}
```

Đoạn code này có ý nghĩa là chúng ta sẽ tạo ra một queue có tên là queue_name , một topic exchange tên là exchange_name và sẽ queue này sẽ được đăng kí nhận message từ exhange này qua ROUTING có partern rabbitmq.*

with multi-queue
```java
@Bean
public Declarables directBingdings() {
    Queue directQueue1 = new Queue(TOPIC_QUEUE_1_NAME, false);
    Queue directQueue2 = new Queue(TOPIC_QUEUE_2_NAME, false);
    DirectExchange directExchange = new DirectExchange(DIRECT_EXCHANGE_NAME);
    return new Declarables(
        directExchange,
        bind(directQueue1).to(directExchange).with("direct.exchange-1"),
        bind(directQueue2).to(directExchange).with("direct.exchange-2")
    );
}
    
@RabbitListener(queues = {TOPIC_QUEUE_1_NAME})
public void receiveMessageTopic1(String message) {
    System.out.println(String.format("[%s] [%s] Received message: %s", TOPIC_EXCHANGE_NAME, TOPIC_QUEUE_1_NAME, message));
}

@RabbitListener(queues = {TOPIC_QUEUE_2_NAME})
public void receiveMessageTopic2(String message) {
    System.out.println(String.format("[%s] [%s] Received message: %s", TOPIC_EXCHANGE_NAME, TOPIC_QUEUE_2_NAME, message));
}
```

## **Producer config**

Use `RabbitTemplate` to send

```java
@RestController
@RequestMapping("/api/v1")
public class MessageController {

    @Autowired
    private RabbitTemplate rabbitTemplate;

    @PostMapping("/message")
    public void sendMessage(@RequestBody final Message message) {
        rabbitTemplate.convertAndSend(RabbitMQConfig.TOPIC_EXCHANGE_NAME, "msg.important.warn",
                "topic important warn: " + message.getMessage());

        rabbitTemplate.convertAndSend(RabbitMQConfig.TOPIC_EXCHANGE_NAME, "msg.error",
                "topic important error: " + message.getMessage());

        rabbitTemplate.convertAndSend(exchange, routingKey, message);
    }
}
```

Or you can use Spring AMQP:

```java
AmqpTemplate.convertAndSend(
                    exchange,
                    routingKey,
                    eventJson,
                    message -> {
                        message.getMessageProperties().setPriority(priority);
                        return message;
                    }
```