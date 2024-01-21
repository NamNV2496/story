# REDIS

## Redis là gì? Tại sao phải dùng Redis?

Redis là tên viết tắt của Remote Dictionary Server (Máy chủ từ điển từ xa), đây là một kho lưu trữ dữ liệu dưới dạng key-value, trên bộ nhớ, mã nguồn mở, nhanh chóng.

<img src="blog/java/img/redis.png" style="display: block; margin-right: auto; margin-left: auto;">

## Ưu điểm

- Hoạt động lưu trữ key-value trên RAM cao.
- Cho phép phục hồi dữ liệu khi gặp sự cố nhờ việc lưu trữ dữ liệu trên đĩa cứng .
- Có khả năng phản hồi rất nhanh chỉ với vài mili giây để xử lý hàng triệu request mỗi giây.
- Tính năng sao chép đồng bộ giữa 2 cơ sở dữ liệu với nhau (replication) và tính năng cluster.


## Redis có các kiểu dữ liệu nào?
- String: là một trong những kiểu dữ liệu linh hoạt nhất redis. String là cấu trúc dữ liệu nhị phân và có thể lưu trữ đa dạng loại dữ liệu như số thập phân, ảnh JPEG, chuỗi,… Redis có thể làm việc với string hoặc từng phần của nó, đồng thời thực hiện tăng hay giảm các giá trị của float, integer.
- List: là một danh sách của strings, chứa tập hợp các phần tử chuỗi và được sắp xếp theo thứ tự insert. Redis có thể dễ dàng thêm một phần tử vào cuối hoặc đầu list. Vì việc truy xuất cực nhanh nên list rất phù hợp với các bài toán cần thao tác với nhiều phần tử gần đầu và cuối. Tuy nhiên việc thực hiện truy xuất các phần tử ở giữa list lại diễn ra rất chậm.
- Set: là tập hợp các string (đều không được sắp xếp). Redis có khả năng hỗ trợ các thao tác như đọc, thêm, xóa từng phần tử hay truy xuất, kiểm tra một phần tử xuất hiện trong tập hợp. Bên cạnh đó, redis còn hỗ trợ các phép tập hợp như lấy phần hợp, phần giao hay lấy phần khác nhau,…
- Hash: Việc lưu trữ hash table của các cặp key-value và trong đó các key được sắp xếp ngẫu nhiên, không theo bất kỳ thứ tự nào cả. Redis hỗ trợ các thao tác người dùng như đọc, thêm , xóa từng phần tử hoặc toàn bộ hash.
- Sorted set: là một danh sách được sắp xếp theo score, trong đó mỗi phần tử như là map của 1 string (member) và 1 floating-point number (score). Tương tự với set, redis cũng có thể thêm, xóa, đọc từng phần tử. Các phần tử của sorted set đều được sắp xếp theo thứ tự từ score nhỏ đến lớn.


## Where is redis saving?
Redis saves snapshots of the dataset on disk, in a binary file called dump.rdb by default. You can configure Redis to have it save the dataset every N seconds if there are at least M changes in the dataset, or you can manually call the SAVE or BGSAVE commands¹.

Nguồn: Cuộc hội thoại với Bing, 30/5/2023
(1) Redis persistence | Redis. https://redis.io/docs/management/persistence/.
(2) How To Back Up and Restore Your Redis Data on Ubuntu 14.04. https://www.digitalocean.com/community/tutorials/how-to-back-up-and-restore-your-redis-data-on-ubuntu-14-04.
(3) What is Redis? Get started with data types, commands, and more - Educative. https://www.educative.io/blog/what-is-redis.
(4) Where is the data directory in Redis? - Stack Overflow. https://stackoverflow.com/questions/32284494/where-is-the-data-directory-in-redis.
(5) Where does Redis store data? - Quora. https://www.quora.com/Where-does-Redis-store-data.



<img src="blog/java/img/redis1.jpg" style="display: block; margin-right: auto; margin-left: auto;">

<img src="blog/java/img/redis2.jpg" style="display: block; margin-right: auto; margin-left: auto;">

## what is maximum size of redis
The maximum size of a Redis string is 512MB¹. Redis can handle up to 2^32 keys, and was tested in practice to handle at least 250 million keys per instance. Every hash, list, set, and sorted set can hold 2^32 elements³.

## How to read data in redis without locking

You can read data from Redis without locking it by using the Jedis library¹. You can use the JedisPool class to create a pool of connections to Redis. Then you can use the getResource() method to get a connection from the pool. Once you have a connection, you can use it to read data from Redis¹. You can also use Redisson's Lock which implements java.util.concurrent.locks.Lock interface².

Nguồn: Cuộc hội thoại với Bing, 30/5/2023
(1) java - Redis data retrieval - Stack Overflow. https://stackoverflow.com/questions/39517844/redis-data-retrieval.
(2) A Guide to Redis with Redisson | Baeldung. https://www.baeldung.com/redis-redisson.
(3) Introduction to Lettuce - the Java Redis Client | Baeldung. https://www.baeldung.com/java-redis-lettuce.
Write to

You can read data from Redis without locking it using Jedis in two threads at the same time by using the JedisPool class¹. The JedisPool class is thread-safe and can be used to create multiple instances of Jedis objects. Here is an example of how to read data from Redis using two threads:
```java
JedisPool pool = new JedisPool("localhost", 6379);
ExecutorService executor = Executors.newFixedThreadPool(2);

for (int i = 0; i < 2; i++) {
    executor.submit(() -> {
        try (Jedis jedis = pool.getResource()) {
            String value = jedis.get("mykey");
            System.out.println(value);
        }
    });
}

executor.shutdown();
```
This code creates a JedisPool object that connects to Redis on the local machine. It then creates an ExecutorService with two threads and submits two tasks to the executor. Each task gets a connection from the pool using the getResource() method and reads the value of the key "mykey" using the get() method.

I hope this helps!

Nguồn: Cuộc hội thoại với Bing, 30/5/2023
(1) Why A single Jedis instance is not threadsafe? - Stack Overflow. https://stackoverflow.com/questions/30107383/why-a-single-jedis-instance-is-not-threadsafe.
(2) Intro to Jedis - the Java Redis Client Library | Baeldung. https://www.baeldung.com/jedis-java-redis-client-library.
(3) transactions - ERR EXEC without MULTI - Jedis - Stack Overflow. https://stackoverflow.com/questions/16084723/err-exec-without-multi-jedis.


## Jedis pool vs redisTemplate

Jedis and RedisTemplate are both Java clients for Redis. Jedis is the official Java client recommended by Redis. It is a simple and lightweight client that provides basic Redis operations such as get, set, del, etc. JedisPool can be used to obtain connections for these operations. On the other hand, RedisTemplate is a highly encapsulated JedisApi in SpringDataRedis. It provides more advanced features than Jedis such as object serialization and deserialization, transaction management, and connection pool management¹³⁴. 

In terms of performance, Jedis is faster than RedisTemplate because it is a simple client that does not have the overhead of Spring's abstraction layer⁵. However, RedisTemplate provides more advanced features than Jedis and is easier to use in Spring applications².

I hope this helps! Let me know if you have any other questions.

Nguồn: Cuộc hội thoại với Bing, 30/5/2023
(1) Jedis与RedisTemplate的区别 - 何苦-> - 博客园. https://www.cnblogs.com/fuqian/p/17326507.html.
(2) redisTemplate、jedis、redission的对比 - 宥晔 - 博客园. https://www.cnblogs.com/seven717/p/15394122.html.
(3) Introduction to Spring Data Redis | Baeldung. https://www.baeldung.com/spring-data-redis-tutorial.
(4) Why Jedis faster than spring redisTemplate? - Stack Overflow. https://stackoverflow.com/questions/51611207/why-jedis-faster-than-spring-redistemplate.
(5) redisTemplate和JedisPool的效率对比 - CSDN博客. https://blog.csdn.net/qq_40310480/article/details/103991771.
(6) Redis深入学习：Jedis和Spring的RedisTemplate - CSDN博客. https://blog.csdn.net/csdn2497242041/article/details/102675435.



## Cài đặt

```xml
pom.xml
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>s
    </dependency>
```
### Config application.yml
```yml
application.yml

spring:
  cache:
    type: redis
  redis:
    host: localhost
    port: 6379
    // set database 
    // database from 0-15 change to save in another storage
    database: 4
#// lecture by application
#    Password:
#    Timeout: 1000ms
#    lettuce:
#      pool:
#        max-active: 16
#        max-idle: 16
#        min-idle: 8
#        max-wait: 1ms
#        time-between-eviction-runs: 9000
```
### redis config để set dạng data sẽ được lưu trên redis:

### - STRING: new StringRedisSerializer() Ex: ``[{"roles":"admin"},{"roles":"guest"}]``
### - JSON: new GenericJackson2JsonRedisSerializer() Ex: ``"{\n  \"AccountID\": \"1123013FFD\",\n  \"Transaction_id\": \"43248937850\",\n  \"Transaction_time\": \"30-01-2022 16:21:54\",\n}"`` - các thông tin sẽ có thêm ký tự \

```java
@Configuration
@EnableRedisRepositories
public class RedisConfig {

// way 1: use jedis rất thông dụng và sử dụng cho single thread
    @Bean
    JedisConnectionFactory jedisConnectionFactory() {
        return new JedisConnectionFactory();
    }

// key: string, value: json
    @Bean
    public RedisTemplate<String, Object> redisTemplateJson() {
        RedisTemplate<String, Object> template = new RedisTemplate<>();
        template.setConnectionFactory(jedisConnectionFactory());
        template.setDefaultSerializer(new GenericJackson2JsonRedisSerializer());
        template.setKeySerializer(new StringRedisSerializer());
        template.setHashKeySerializer(new GenericJackson2JsonRedisSerializer());
        template.setValueSerializer(new GenericJackson2JsonRedisSerializer());
        return template;
    }

// key: string, value: string
    @Bean
    public RedisTemplate<String, String> redisTemplateString(RedisConnectionFactory connectionFactory) {
        RedisTemplate<String, String> template = new RedisTemplate<>();
        template.setConnectionFactory(connectionFactory);
        template.setDefaultSerializer(new StringRedisSerializer());
        template.setKeySerializer(new StringRedisSerializer());
        template.setHashKeySerializer(new StringRedisSerializer());
        template.setValueSerializer(new StringRedisSerializer());
        return template;
    }

// way 2: use lettuce. Letture hỗ trợ multithread nên sẽ rất an toàn nhưng phải dựa trên thực tế để đưa ra lựa chọn
//    @Bean
//    public RedisConnectionFactory redisConnectionFactory() {
//    return new LettuceConnectionFactory();
//}
//    @Bean
//    @Primary
//    public RedisTemplate<String, Shopping> redisTemplate(){
//        RedisTemplate<String, Shopping> empTemplate = new RedisTemplate<>();
//        empTemplate.setConnectionFactory(redisConnectionFactory());
//        return empTemplate;
//    }
}
```


### Redis hỗ trợ các định dạng
```java
    redisTemplate.opsForValue().set(key)
    redisTemplate.opsForValue().get(key)

    redisTemplate.opsForList().put(key, data)
    redisTemplate.opsForList().get(key)

    redisTemplate.opsForHash().put(key, hashKey)
    redisTemplate.opsForHash().get(key, hashKey)

    redisTemplate.opsForSet().set(key, data)
    redisTemplate.opsForSet().get(key)
```



Redis (Remote Dictionary Server) is known for its exceptional speed and performance. There are several reasons why Redis is considered fast:

- **𝙄𝒏-𝒎𝙚𝒎𝙤𝒓𝙮 𝙙𝒂𝙩𝒂 𝒔𝙩𝒐𝙧𝒂𝙜𝒆**: Redis is primarily an in-memory data store, which is stored and accessed directly from the server's RAM, eliminating disk I/O operations significantly slower than memory access, resulting in faster data retrieval and manipulation.

- **𝙎𝒊𝙢𝒑𝙡𝒆 𝒅𝙖𝒕𝙖 𝙨𝒕𝙧𝒖𝙘𝒕𝙪𝒓𝙚𝒔**: Redis supports various data structures like strings, lists, sets, hashes, and sorted sets. These data structures are optimized for performance and allow for efficient operations, making Redis a highly performant key-value store.

- **𝙎𝒊𝙣𝒈𝙡𝒆-𝒕𝙝𝒓𝙚𝒂𝙙𝒆𝙙 𝙖𝒓𝙘𝒉𝙞𝒕𝙚𝒄𝙩𝒖𝙧𝒆**: Redis follows a single-threaded, event-driven architecture eliminating the need for context switching and locking mechanisms, reducing overhead and improving overall performance. It also allows Redis to handle many concurrent requests without the overhead of managing multiple threads.

- **𝘼𝒔𝙮𝒏𝙘𝒉𝙧𝒐𝙣𝒐𝙪𝒔 𝒐𝙥𝒆𝙧𝒂𝙩𝒊𝙤𝒏𝙨**: Redis supports asynchronous operations, allowing clients to send multiple commands without waiting for a response, and improves throughput by reducing the time spent waiting for network round trips.

- **𝙈𝒊𝙣𝒊𝙢𝒂𝙡 𝙙𝒊𝙨𝒌 𝒑𝙚𝒓𝙨𝒊𝙨𝒕𝙚𝒏𝙘𝒆**: Although Redis is primarily an in-memory database, it provides options for disk persistence to ensure data durability. However, Redis uses an append-only file (AOF) and point-in-time snapshots rather than continuously writing data to disk, minimising disk I/O and maintaining high performance.

- **𝙊𝒑𝙩𝒊𝙢𝒊𝙯𝒆𝙙 𝙙𝒂𝙩𝒂 𝒔𝙩𝒓𝙪𝒄𝙩𝒖𝙧𝒆𝙨 𝙖𝒏𝙙 𝙖𝒍𝙜𝒐𝙧𝒊𝙩𝒉𝙢𝒔**: Redis is designed with performance in mind. It utilise efficient data structures and algorithms to achieve high-speed operations. For example, Redis cleverly uses data structures like skip lists and hash tables, which provide fast access and search capabilities.

- **𝙉𝒐𝙣-𝙗𝒍𝙤𝒄𝙠𝒊𝙣𝒈 𝑰/𝑶**: Redis uses non-blocking I/O operations, leveraging the asynchronous nature of modern operating systems, allowing Redis to efficiently handle many concurrent connections without being limited by traditional blocking I/O mechanisms.

Overall, Redis combines several performance-oriented design choices, including in-memory storage, a single-threaded architecture, asynchronous operations, optimised data structures, and non-blocking I/O, to deliver exceptional speed and responsiveness.
