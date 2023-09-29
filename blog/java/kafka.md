# Kafka

## kafka là gì? Tại sao phải dùng kafka?

Kafka là một nền tảng message publish/subscribe phân tán (distributed messaging system) mã nguồn mở được xây dựng nhằm mục đích xử lý dữ liệu streaming theo thời gian thực.

### Kafka thường được sử dụng cho 2 mục đích chính sau:

- Publish và subscribe các stream of record (luồng dữ liệu)
- Lưu trữ các stream of record theo thứ tự
- Hỗ trợ xử lý stream of record theo thời gian thực

## Ưu điểm
- Open-source
- High-throughput: Có khả năng xử lý một lượng lớn thông tin một cách liên tục, gần như không có thời gian chờ
- High-frequency: Có thể xử lý cùng lúc nhiều message và nhiều thể loại topic
- Scalability: Dễ dàng mở rộng khi có nhu cầu
- Tự động lưu trữ message, dễ dàng kiểm tra lại
- Community: Cộng đồng Kafka hỗ trợ phát triển mạnh mẽ vì là opensource (mã nguồn mở) nên hệ thống luôn được cập nhật và hoàn thiện từng ngày.
## Nhược điểm
- Chưa có bộ công cụ giám sát hoàn chỉnh: Có nhiều tool khác nhau nhưng mỗi tool chỉ đáp ứng một tính năng quản lý nhất định, chẳng hạn như: Kafka tool (offset manager) GUI tool - quản lý topic và consumer, Lense - hỗ trợ query message, Akhq - toolbox quản lý Kafka và view data bên trong Kafka
- Không chọn được topic theo wildcard: Người dùng sẽ cần phải sử dụng chính xác tên topic để xử lý message
- Giảm hiệu suất: Kích thước message tăng khiến cho consumer và producer phải compress và decompress message, từ đó làm bộ nhớ bị chậm đi, ảnh hưởng đến throughput và hiệu suất.
- Xử lý chậm: Đôi khi số lượng queues trong Kafka cluster tăng đột biến khiến Kafka xử lý chậm hơn.


=>  Một số lợi ích nổi bật của Kafka bao gồm:

- Khả năng mở rộng cao: Mô hình phân vùng nhật ký của Kafka cho phép dữ liệu có thể được phân phối trên nhiều máy chủ và mở rộng máy chủ khi có nhu cầu.
  Tốc độ nhanh chóng: Việc xử lý thông qua tách các luồng dữ liệu giúp cho tốc độ trở nên nhanh hơn.
  Khả năng chịu lỗi và độ bền: Do các gói dữ liệu được sao chép và phân phối trên nhiều máy chủ khác nhau, nên khi có sự cố thì dữ liệu sẽ ít gặp lỗi và an toàn hơn.

## Một số khái niệm cơ bản trong Kafka

1. Producer

Producer là những application produce data và gửi data tới Kafka Server. Data này sẽ là những message có định dạng, được gửi dưới dạng mảng byte tới Kafka server. Ví dụ như các bạn có một tập tin .txt chứa text bên trong, chúng ta có thể dùng Producer để đọc từng dòng trong tập tin này rồi gửi tới Kafka server.

2. Consumer

Kafka sử dụng consumer để subscribe vào topic, các consumer được định danh bằng các group name. Nhiều consumer có thể cùng đọc một topic. Sau khi nhận được data, Consumer có thể thêm code để xử lý data theo nhu cầu của mình.

3. Cluster

Kafka cluster là một set các server, mỗi một set này được gọi là 1 broker.

4. Broker

Broker là Kafka server, là cầu nối giữa Message Publisher và Message Consumer, giúp chúng có thể trao đổi message với nhau.

5. Topic

Dữ liệu truyền trong Kafka theo topic, khi cần truyền dữ liệu cho các ứng dụng khác nhau thì sẽ tạo ra các topic khác nhau.

6. Partitions

Kafka là một distributed messaging system và chúng ta có thể setup Kafka server với cluster. Trong trường hợp một topic nhận quá nhiều message tại cùng một thời điểm, chúng ta có thể chia topic này thành những partitions được share giữa các Kafka server với nhau trong một cluster được handle các message này.

Một partition sẽ small và independent với các partitions khác. Số lượng partition cho mỗi topic thì tuỳ theo nhu cầu của ứng dụng mà chúng ta có thể quyết định.

7. Consumer Group

Consumer group là một group các Consumer consume message từ Kafka server. Mỗi một Consumer Group sẽ share với nhau việc handle message.

8. ZOOKEEPER: được dùng để quản lý và bố trí các broker.


<img src="blog/java/img/kafka1.png" style="display: block; margin-right: auto; margin-left: auto;">

<img src="blog/java/img/kafka2.png" style="display: block; margin-right: auto; margin-left: auto;">


# Example about kafka

## Config: Có 2 cách config:

- config trong application.yml
- config trong file config vs @configuraion và @Bean (thực chất nếu có cả 2 thì file @Bean này sẽ ghi đè lên fiel application.yml)

```java
=============================== way 1 ===============================
spring:
  kafka:
    consumer:
      bootstrap-servers: localhost:9092
      group-id: group_id
      auto-offset-reset: earliest
      key-deserializer: org.apache.kafka.common.serialization.StringDeserializer
      value-deserializer: org.apache.kafka.common.serialization.StringDeserializer // nội dung đc gửi lên server dạng string và lắng nghe về dạng string
    producer:
      bootstrap-servers: localhost:9092
      key-serializer: org.apache.kafka.common.serialization.StringSerializer
      value-serializer: org.springframework.kafka.support.serializer.JsonSerializer // nội dung nếu sử dụng ở service này để gửi lên server sẽ ở dạng json
      properties:
        topics:
          testTopic: test_topic_application_yml

```

```java
@Data
@Configuration
@ConfigurationProperties(prefix = "spring.kafka")
public class KafkaConfig {
    ProducerConfig producer;

    @Data
    public static class ProducerConfig {
        Properties properties;
    }

    @Data
    public static class Properties {
        Topics topics;
    }

    @Data
    public static class Topics {
        String testTopic;
    }
}

// get topic file name from application.yaml
kafkaConfig.getProducer().getProperties().getTopics().getTestTopic();
```


```java
=============================== way 2 ===============================
@Configuration
class KafkaConsumerConfig {

    @Value("localhost:9092")
    private String bootstrapServers;

// consume message là string (key: string, value: string)
    @Bean
    public Map<String, String> consumerStringConfigs() {
        Map<String, Object> props = new HashMap<>();
        props.put(ConsumerConfig.BOOTSTRAP_SERVERS_CONFIG, bootstrapServers);
        props.put(ConsumerConfig.GROUP_ID_CONFIG, "group-id");
        // this is config to parse data from kafka is String
        props.put(ConsumerConfig.KEY_DESERIALIZER_CLASS_CONFIG, StringDeserializer.class);
        props.put(ConsumerConfig.VALUE_DESERIALIZER_CLASS_CONFIG, StringDeserializer.class);
        // end
        props.put(ConsumerConfig.ENABLE_AUTO_COMMIT_CONFIG, false);
        props.put(ConsumerConfig.AUTO_COMMIT_INTERVAL_MS_CONFIG, "100");
        props.put(ConsumerConfig.SESSION_TIMEOUT_MS_CONFIG, "15000");
        props.put(ConsumerConfig.AUTO_OFFSET_RESET_CONFIG, "earliest");
        props.put(JsonDeserializer.TRUSTED_PACKAGES, "*");

        return new DefaultKafkaConsumerFactory<>(props);
    }

    @Bean
    public KafkaListenerContainerFactory<ConcurrentMessageListenerContainer<String, String>> StringkafkaListenerContainerFactory() {
        ConcurrentKafkaListenerContainerFactory<String, String> factory = new ConcurrentKafkaListenerContainerFactory<>();
        factory.setConsumerFactory(consumerStringConfigs());
        return factory;
    }

// consume message là json (key: string, value: json)
    @Bean
    public ConsumerFactory<String, User> consumerJsonFactory() {
        Map<String, Object> props = new HashMap<>();
        props.put(ConsumerConfig.BOOTSTRAP_SERVERS_CONFIG, brokers);
        props.put(ConsumerConfig.GROUP_ID_CONFIG, groupId);
        // this is config to parse data from kafka is json but key is string
        props.put(ConsumerConfig.KEY_DESERIALIZER_CLASS_CONFIG, StringDeserializer.class);
        props.put(ConsumerConfig.VALUE_DESERIALIZER_CLASS_CONFIG, JsonDeserializer.class);
        // end
        props.put(ErrorHandlingDeserializer.VALUE_DESERIALIZER_CLASS, ErrorHandlingDeserializer.class);

        // set default type is object in json
        props.put(JsonDeserializer.VALUE_DEFAULT_TYPE, User.class);
        // end
        props.put(JsonDeserializer.TRUSTED_PACKAGES, "*");
        
        return new DefaultKafkaConsumerFactory<>(props);
    }

    @Bean
    KafkaListenerContainerFactory<ConcurrentMessageListenerContainer<String, User>> jsonKafkaListenerContainerFactory() {
        ConcurrentKafkaListenerContainerFactory<String, User> factory =
                new ConcurrentKafkaListenerContainerFactory<>();
        factory.setConsumerFactory(consumerJsonFactory());
        return factory;
    }

}

```
## Mặc định sẽ lắng nghe về dạng string, nhưng để config lắng nghe về dạng json

```java

config lắng nghe về dạng json

@KafkaListener(topics = Constant.KAFKA, groupId = "${groupid.consumer.test:local_if_null}", containerFactory = "jsonKafkaListenerContainerFactory")


config lắng nghe về dạng string

@KafkaListener(topics = Constant.KAFKA, groupId = "${groupid.consumer.test:local_if_null}", containerFactory = "stringKafkaListenerContainerFactory")
hoặc
@KafkaListener(topics = Constant.KAFKA, groupId = "${groupid.consumer.test:local_if_null}")

```


# Config producer

Nếu lắng nghe kafka dạng <string, string> thì dữ liệu đẩy lên cũng phải đc đồng bộ dang <string, string>

Nếu lắng nghe kafka dạng <string, json> thì dữ liệu đẩy lên cũng phải đc đồng bộ dang <string, json>

```java
@Configuration
class KafkaProducerConfig {

    @Value("localhost:9092")
    private String bootstrapServers;

// data push to kafka is string (key: string, value: string)
    @Bean
    public ProducerFactory<String, String> producerStringFactory() {
        Map<String, Object> configProps = new HashMap<>();
        configProps.put(
                ProducerConfig.BOOTSTRAP_SERVERS_CONFIG, bootstrapAddress);
        configProps.put(
                ProducerConfig.KEY_SERIALIZER_CLASS_CONFIG, StringSerializer.class);
        configProps.put(
                ProducerConfig.VALUE_SERIALIZER_CLASS_CONFIG, StringSerializer.class);
        return new DefaultKafkaProducerFactory<>(configProps);
    }

    @Bean
    public KafkaTemplate<String, String> kafkaStringTemplate() { // inject kafkaStringTemplate to send to server
        return new KafkaTemplate<>(producerStringFactory());
    }

// data in Json (key: string, value: json)
@Bean
    public ProducerFactory<String, User> producerJsonFactory() {
    // public ProducerFactory<String, String> producerJsonFactory() { // also OK if we use `KafkaTemplate<String, String> kafkaJsonTemplate()`
        Map<String, Object> configProps = new HashMap<>();
        configProps.put(
                ProducerConfig.BOOTSTRAP_SERVERS_CONFIG, bootstrapAddress);
        configProps.put(
                ProducerConfig.KEY_SERIALIZER_CLASS_CONFIG, StringSerializer.class);
        configProps.put(
                ProducerConfig.VALUE_SERIALIZER_CLASS_CONFIG, JsonSerializer.class);
        return new DefaultKafkaProducerFactory<>(configProps);
    }

    @Bean
    public KafkaTemplate<String, User> kafkaJsonTemplate() { // inject kafkaJsonTemplate to send to server
        return new KafkaTemplate<>(producerJsonFactory());
    }

```

config to read in special partition of topic
```java
application.yml
groupid.consumer: UATgroupId

/* Consumer */

    @KafkaListener(topics = Constant.KAFKA.TEST_TOPIC, groupId = "${groupid.consumer:local_group_id}")
    @KafkaListener(
            topicPartitions = {
                    @TopicPartition(
                            topic = "demo",
                            partitionOffsets = {
                                    @PartitionOffset(partition = "0", initialOffset = "1")
                            }),
                    @TopicPartition(
                            topic = "topic1",
                            partitionOffsets = {
                                    @PartitionOffset(partition = "1", initialOffset = "0")
                            })
            },
            groupId = "${groupid.consumer:default_groupId_name}
        ) // nếu có define groupid.consumer: UATgroupId thì sẽ lắng nghe. còn ko thì sẽ là default_groupId_name
        
    public void listen(String message) {
        System.out.println("Received Message in group - group-id data: " + message);
    }
    
    
/* producer */

 public void sendMessage(String topic, int partition, String message) {
//        ListenableFuture<SendResult<String, String >> future = kafkaTemplate.send(topic,  message);
        ListenableFuture<SendResult<String, String >> future = kafkaTemplate.send(topic, partition, "key",  message);
        try {
            log.info("check request with timeout 10s");
            future.get(10, TimeUnit.SECONDS);
            log.info("Send request is done: " + future.isDone());
        }
        catch (Exception e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }

        // add callback
        future.addCallback(new ListenableFutureCallback<SendResult<String, String>>() {
            @Override
            public void onSuccess(SendResult<String, String> result) {
                log.info("call send successful!");
            }

            @SneakyThrows
            @Override
            public void onFailure(Throwable ex) {
                throw new Exception("fail");
            }
        });
    }
```

## @KafkaListener

- use `@RetryableTopic` to setup retry times

@RetryableTopic(attempts = "3", backoff = @Backoff(delay = 1000))

- use `concurrency` to set thread: You can specify the number of threads to run in the listener containers by setting the `spring.kafka.listener.concurrency`.
    
    @KafkaListener(topics = "myTopic", groupId = "myGroup", concurrency = "3")


# how many maximum number consumer of kafka?

If we add more consumers to a single group with a single topic than we have partitions, some of the consumers will be idle and get no messages at all

<img src="blog/java/img/kafka3.png" style="display: block; margin-right: auto; margin-left: auto;">

https://www.oreilly.com/library/view/kafka-the-definitive/9781491936153/ch04.html



# 𝐖𝐡𝐲 𝐢𝐬 𝐊𝐚𝐟𝐤𝐚 𝐒𝐨 𝐅𝐚𝐬𝐭?

Kafka is known for its high performance and speed due to several reasons:

The two most important reasons are its low latency message delivery through Sequential I/O and Zero Copy Principle.

𝗦𝗲𝗾𝘂𝗲𝗻𝘁𝗶𝗮𝗹 𝗜𝗢:

Kafka relies heavily on the filesystem for storing and caching messages. The general perception is that "disks are slow, "meaning high seek time. Imagine if we can avoid seeking time. We can achieve low latency as low as RAM here. Kafka does this through Sequential I/O. Kafka efficiently uses log, an append-only, totally ordered data structure.

𝗧𝗿𝗮𝗱𝗶𝘁𝗶𝗼𝗻𝗮𝗹 𝗗𝗮𝘁𝗮 𝗧𝗿𝗮𝗻𝘀𝗳𝗲𝗿

To read a file from a disk, then send it over the network, a traditional data transfer would require four context switches between user and kernel modes, making the data copied four times.

The transfer process consists of four steps below:

1) The File. read() call makes the context switch from user mode to kernel mode, and the data copy is performed by the direct memory access (DMA) engine, which reads the file content from the disk and stores it into a kernel address space buffer (OS buffer);

2) Data is copied from the kernel space buffer into the user buffer(Application buffer), making the File.read() call return and the context to switch back to user mode

3) The Socket.send() call makes the context switch to kernel mode, and the third data copy is performed from the user buffer(application buffer) to the kernel buffer(socket Buffer) again

4) Finally, the DMA engine performs a fourth copy of the data, passing it from the kernel buffer (socket buffer) to the network interface controller (NIC) buffer to be sent over the network.

If the requested data is larger than the kernel buffer size, there will be even more copies between kernel and user spaces. Zero-copy optimisation reduces these redundant data copies.

𝗭𝗘𝗥𝗢 𝗖𝗢𝗣𝗬

A zero-copy optimisation consists of removing the second and third data copies, making the data transferred directly from the OS buffer to the nic buffer.

Kafka uses this zero-copy principle by requesting the kernel to move the data to nic rather than via the application.

Zero Copy is a shortcut to save multiple data copies between the application and kernel contexts. This approach brings down the time by ​​approximately 65%.

What else?

Kafka uses many other techniques apart from the ones mentioned above to make systems much faster and more efficient:

𝗠𝗲𝘀𝘀𝗮𝗴𝗲 𝗖𝗼𝗺𝗽𝗿𝗲𝘀𝘀𝗶𝗼𝗻: Kafka allows for message compression, which reduces messages' size and enables faster data transmission and processing.
Message Batching: Kafka can batch messages together, which reduces the overhead of individual message processing and improves throughput.

Overall, Kafka's speed results from its optimised architecture(sequential io, append-only log, Zero Copy), compression, batching, in-memory storage.


<img src="blog/java/img/kafka4.png" style="display: block; margin-right: auto; margin-left: auto;">

# Is Apache Kafka Providing Real Message Ordering?

Let’s imagine the situation below: a single producer sending three messages to a topic:

- Message 1, for some reason, finds a long network route to Apache Kafka
- Message 2 finds the quickest network route to Apache Kafka
- Message 3 gets lost in the network

<img src="blog/java/img/kafka5.png" style="display: block; margin-right: auto; margin-left: auto;">

Even in this basic scenario, with only one producer, we could get an unexpected series of messages on the topic. The end result on the Kafka topic will show only two events being stored, with the unexpected ordering `2`, `1`.

## Acks and Retries

But not all is lost! If we look into the producing libraries (aiokafka being an example), we have ways to ensure that messages are delivered properly.

First of all, to avoid the problem with the message `3` in the above scenario, we could define a proper acknowledgment mechanism. The acks producer parameter allows us to define what confirmation of message reception we want to have from Apache Kafka.

Setting this parameter to `1` will ensure that we receive an acknowledgment from the primary broker responsible for the topic (and partition). Setting it to all will ensure that we receive the `ack` only if both the primary and the replicas correctly store the message, thus saving us from problems when only the primary receives the message and then fails before propagating it to the replicas.

Once we set a sensible `ack`, we should set the possibility to retry sending the message if we don’t receive a proper acknowledgment. Differently from other libraries (kafka-python being one of them), aiokafka will retry sending the message automatically until the timeout (set by the `request_timeout_ms` parameter) has been exceeded.

With acknowledgment and automatic retries, we should solve the problem of the message `3`. The first time it is sent, the producer will not receive the ackTherefore, after the `retry_backoff_ms` interval, it will send the message `3` again.

<img src="blog/java/img/kafka6.png" style="display: block; margin-right: auto; margin-left: auto;">

## Max In-Flight Requests

However, if you watch the end result in the Apache Kafka topic closely, the resulting ordering is not correct: we sent `1`,`2`,`3` and got `2`,`1`,`3` in the topic… how to fix that?

The old method (available in kafka-python) was to set the maximum in-flight request per connection: the number of messages we allow to be “in the air” at the same time without acknowledgment. The more messages we allow in the air at the same time, the more risk of getting out-of-order messages.

When using kafka-python, if we absolutely needed to have a specific ordering in the topic, we were forced to limit the `max_in_flight_requests_per_connection` to `1`. Basically, supposing that we set the ack parameter to at least `1`, we were waiting for an acknowledgment of every single message (or batch of messages if the message size is less than the batch size) before sending the following one.

<img src="blog/java/img/kafka7.png" style="display: block; margin-right: auto; margin-left: auto;">

# Increase Complexity With Multiple Producers

## Different Locations, Different Latency

Again, the network is not equal, and with several producers located in possibly very remote positions, the different latency means that the Kafka ordering could differ from the one based on event time.

<img src="blog/java/img/kafka8.png" style="display: block; margin-right: auto; margin-left: auto;">

## Batching, an Additional Variable

To achieve higher throughput, we might want to batch messages. With batching, we send messages in “groups,” minimizing the overall number of calls and increasing the payload to overall message size ratio. But, in doing so, we can again alter the ordering of events. The messages in Apache Kafka will be stored per batch, depending on the batch ingestion time. Therefore, the ordering of messages will be correct per batch, but different batches could have different ordered messages within them.

<img src="blog/java/img/kafka9.png" style="display: block; margin-right: auto; margin-left: auto;">

# The Savior: Event Time


We understood that the original premise about Kafka keeping the message ordering is not 100% true. The ordering of the messages depends on the Kafka ingestion time and not on the event generation time. But what if the ordering based on event time is important?

Well, we can’t fix the problem on the production side, but we can do it on the consumer side. All the most common tools that work with Apache Kafka have the ability to define which field to use as event time, including [Kafka Streams](https://kafka.apache.org/0110/documentation/streams/core-concepts#streams_time), Kafka Connect with the dedicated Timestamp extractor single message transformation (SMT), and [Apache Flink®](https://nightlies.apache.org/flink/flink-docs-release-1.16/docs/concepts/time/).

Consumers, when properly defined, will be able to reshuffle the ordering of messages coming from a particular Apache Kafka topic. Let’s analyze the Apache Flink example below:

```java
CREATE TABLE CPU_IN (
    hostname STRING,
    cpu STRING,
    usage DOUBLE,
    occurred_at BIGINT,
    time_ltz AS TO_TIMESTAMP_LTZ(occurred_at, 3),
    WATERMARK FOR time_ltz AS time_ltz - INTERVAL '10' SECOND
    )
WITH (
   'connector' = 'kafka',
   'properties.bootstrap.servers' = '',
   'topic' = 'cpu_load_stats_real',
   'value.format' = 'json',
   'scan.startup.mode' = 'earliest-offset'
)
```

In the above Apache Flink table definition, we can notice:

- `occurred_at`: the field is defined in the source Apache Kafka topic in unix time (datatype is BIGINT).
- `time_ltz AS TO_TIMESTAMP_LTZ(occurred_at, 3)`: transforms the unix time into the Flink timestamp.
- `WATERMARK FOR time_ltz AS time_ltz - INTERVAL '10' SECOND` defines the new `time_ltz` field (calculated from `occurred_at`) as the event time and defines a threshold for late arrival of events with a maximum of 10 seconds delay.
Once the above table is defined, the `time_ltz` field can then be used to correctly order events and define aggregation windows, making sure that all events within the accepted latency are included in the calculations.

The `- INTERVAL '10' SECOND` defines the latency of the data pipeline and is the penalty we need to include to allow the correct ingestion of late-arriving events. Please note, however, that the throughput is not impacted. We can have as many messages flowing in our pipeline as we want, but we’re “waiting 10 seconds” before calculating any final KPI in order to make sure we include in the picture all the events in a specific timeframe.

An alternative approach that works only if the events contain the full state is to keep a certain key (`hostname` and `cpu` in the above example) the maximum event time reached so far, and only accept changes where the new event time is greater than the maximum.


