# Logging

<img src="blog/java/img/logging.png" style="display: block; margin-right: auto; margin-left: auto;">

log là 1 phần rất quan trọng trong dự án, dựa trên log giúp chúng ta debug và kiểm thử. Vậy nên cần 1 quy chuẩn cho việc logging này

## Phân loại

Có 2 loại logging phổ biến:
 - logback
 - sleuth

### Logback cho chúng custom thông tin của log in ra, ví dụ thêm thông tin requestId, hoăc 1 threadId nào đó ( kết hợp MDC để đẩy thông tin ra)

### sleuth config nhanh chóng chỉ cần thêm trong pom mà không cần config nhiều với requestId tự sleuth quản lý



## *WAY 1: logback* 

When a file in the classpath has one of the following names, Spring Boot will automatically load it over the default configuration:

- logback-spring.xml
- logback.xml
- logback-spring.groovy
- logback.groovy

<!-- ### we wrap `RequestBodyAdviceAdapter` and `ResponseBodyAdviceAdapter` to print log of request and reponse


```java
@ControllerAdvice
public class LogRequestBodyAdviceAdapter extends RequestBodyAdviceAdapter {
    @Autowired
    ILoggingService loggingService;

    @Autowired
    HttpServletRequest httpServletRequest;

    @Override
    public boolean supports(MethodParameter methodParameter, Type type, Class<? extends HttpMessageConverter<?>> aClass) {
        return true;
    }

    @Override
    public Object afterBodyRead(Object body, HttpInputMessage inputMessage, MethodParameter parameter, Type targetType,
                                Class<? extends HttpMessageConverter<?>> converterType) {
        loggingService.logRequest(httpServletRequest, body);

        return super.afterBodyRead(body, inputMessage, parameter, targetType, converterType);
    }
}

@ControllerAdvice
public class LogResponseBodyAdviceAdaper implements ResponseBodyAdvice<Object> {

    @Autowired
    ILoggingService loggingService;

    @Override
    public boolean supports(MethodParameter returnType, Class<? extends HttpMessageConverter<?>> converterType) {
        return true;
    }

    @Override
    public Object beforeBodyWrite(Object body, MethodParameter methodParameter, MediaType mediaType,
                                  Class<? extends HttpMessageConverter<?>> aClass, ServerHttpRequest request,
                                  ServerHttpResponse response) {
        if (request instanceof ServletServerHttpRequest && response instanceof ServletServerHttpResponse) {
            loggingService.logResponse(((ServletServerHttpRequest) request).getServletRequest(),
                    ((ServletServerHttpResponse) response).getServletResponse(), body);
        }
        return body;
    }
}


MDC.put("transaction_id", tx.getTransactionId());

add to logback.xml

%X{transaction_id} to catch it
```

log type in `LoggingServiceImpl` class

```java

REQUEST method=[GET] path=[/person] headers=[application/jsonPostmanRuntime/7.29.2*/*58899ae1-4727-43c7-8ff7-01ebdc264586localhost:8080gzip, deflate, brkeep-alive53]

RESPONSE method=[GET] path=[/person] headers=[application/jsonPostmanRuntime/7.29.2*/*58899ae1-4727-43c7-8ff7-01ebdc264586localhost:8080gzip, deflate, brkeep-alive53] body=[Person(id=1, name=nam, age=13)]

``` -->
API:

    GET http://localhost:8080/person

    GET http://localhost:8080/hello

# `logback-spring.xml`

```java
<?xml version="1.0" encoding="UTF-8"?>
<configuration>

    <property name="LOGS" value="./logs" />

    <appender name="Console"
        class="ch.qos.logback.core.ConsoleAppender">
        <layout class="ch.qos.logback.classic.PatternLayout">
            <Pattern>
                %black(%d{ISO8601}) %highlight(%-5level) [%blue(%t)] %yellow(%C{1.}): %msg%n%throwable %X{transaction_id}
            </Pattern>
        </layout>
    </appender>

    <appender name="RollingFile"
        class="ch.qos.logback.core.rolling.RollingFileAppender">
        <file>${LOGS}/spring-boot-logger.log</file>
        <encoder
            class="ch.qos.logback.classic.encoder.PatternLayoutEncoder">
            <Pattern>%d %p %C{1.} [%t] %m%n</Pattern>
        </encoder>

        <rollingPolicy
            class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <!-- rollover daily and when the file reaches 10 MegaBytes -->
            <fileNamePattern>${LOGS}/archived/spring-boot-logger-%d{yyyy-MM-dd}.%i.log
            </fileNamePattern>
            <timeBasedFileNamingAndTriggeringPolicy
                class="ch.qos.logback.core.rolling.SizeAndTimeBasedFNATP">
                <maxFileSize>10MB</maxFileSize>
            </timeBasedFileNamingAndTriggeringPolicy>
        </rollingPolicy>
    </appender>
    
    <!-- LOG everything at INFO level -->
    <root level="info">
        <appender-ref ref="RollingFile" />
        <appender-ref ref="Console" />
    </root>

    <!-- LOG "com.baeldung*" at TRACE level -->
    <logger name="com.baeldung" level="trace" additivity="false">
        <appender-ref ref="RollingFile" />
        <appender-ref ref="Console" />
    </logger>

</configuration>

```

```java

%d – outputs the time that the log message occurred in formats that SimpleDateFormat allows.
%thread – outputs the name of the thread that the log message occurred in.
$-5level – outputs the logging level of the log message.
%logger{36} – outputs the package + class name the log message occurred in. The number inside the brackets represents the maximum length of the package + class name. If the output is longer than the specified length, it will take a substring of the first character of each individual package starting from the root package until the output is below the maximum length. The class name will never be reduced. A nice diagram of this can be found in the Conversion Word docs.
%M – outputs the name of the method that the log message occurred in (apparently this is quite slow to use and not recommended unless you're not worried about performance, or if the method name is particularly important to you).
%msg – outputs the actual log message.
%n – line break.
%magenta() – sets the color of the output contained in the brackets to magenta (other colors are available).
highlight() – sets the color of the output contained in the brackets depending on the logging level (for example ERROR = red).
```

# *WAY 2: SLEUTH* 

```java
    <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-sleuth</artifactId>
            <version>3.1.4</version>
        </dependency>
    </dependencies>
    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-dependencies</artifactId>
                <version>${spring-cloud.version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>
```
config on POM and remove logback-spring.xml

### Note: ### We can't migrate logbak with Sleuth 

[Reference](https://dzone.com/articles/configuring-logback-with-spring-boot)


Enable log in application:

```java
logging:
  level:
    // enable hibernate log
    org.hibernate.SQL: DEBUG
    org.hibernate.type.descriptor.sql.BasicBinder: DEBUG
    // enable hibernate mongoDB
    org.springframework.data.mongodb.core.MongoTemplate: DEBUG
    org.springframework.data.mongodb.repository.query: DEBUG
```

