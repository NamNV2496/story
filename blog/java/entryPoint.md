# Entry point

Là 1 request thông qua HTTP hoặc HTTPS để tạo request cho 1 service khác
Thông thường ta sẽ biết đến 2 kiểu là `RestTemplate` và `WebClient`

Hãy cùng so sánh chúng nhé!


## 1. WebClient

WebClient là gì ?
WebClient có thể hiểu đơn giản là một interface đại diện cho một entry point chính thực hiện các request.
- Nó được tạo ra như là một phần của module Spring Web Reactive và thay thế cho class RestTemplate cũ.
- Nó là interface chỉ có duy nhất một triển khai đó là class DefaultWebClient và đây cũng là class mà chúng ta sẽ sử dụng.


```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-webflux</artifactId>
</dependency>
```

### 1.1 Làm sao để tạo ra 1 request vs webClient

**Khởi tạo**

    WebClient client1 = WebClient.create();
    WebClient client2 = WebClient.create("http://localhost:8080");
    WebClient client3 = WebClient
        .builder()
            .baseUrl("http://localhost:8080")
            .defaultCookie("cookieKey", "cookieValue")
            .defaultHeader(HttpHeaders.CONTENT_TYPE, MediaType.APPLICATION_JSON_VALUE) 
            .defaultUriVariables(Collections.singletonMap("url", "http://localhost:8080"))
        .build();

**Customer request**

setup đầu tiên là setup method của request là gì

    client1.method(HttpMethod.POST)
    client1.post()

Tiếp theo là setup xem uri của request này:
    
    client1.post()
        .uri("/account-service") 
    
    client1.post()
        .uri(uriBuilder -> uriBuilder
                            .path("/account-service")
                            .queryParam("param1", paramValue)
                            .build())
    
    => output: POST http://localhost:8080/account-service?param1=paramValue

    client1.post()
        .uri(uriBuilder -> uriBuilder
                            .path("/account-service/{path}")
                            .queryParam("param1", paramValue)
                            .build(`patchValue`))
    
    => output: POST http://localhost:8080/account-service/patchValue
    
    client1.post()
        .uri(uriBuilder -> uriBuilder
                            .path("/account-service/{path1}/{path2}")
                            .queryParam("param1", paramValue)
                            .build(patchValue1, patchValue2))

     => output: POST http://localhost:8080/account-service/patchValue1/patchValue2?param1=paramValue


        MultiValueMap<String, String> params = new LinkedMultiValueMap<>();
        params.set("param1", paramValue1);
        params.set("param2", paramValue2);
        params.set("param3", paramValue3);

    client1.get()
        .uri(uriBuilder -> uriBuilder
                        .path("/account-service")
                        .replaceQueryParams(params)
                        .build())

     => output: POST http://localhost:8080/account-service?param1=paramValue1&param2=paramValue2&param3=paramValue3

**Setup requestBody, header, context-type**


với method post, path, delete ta có thể gọi method .body() để truyền data vào requestBody

    User user = new User();
    user.setName("Test");
    user.setRole("admin");
    client1.post()
        .uri(uriBuilder -> uriBuilder
                            .path("/account-service")
                            .queryParam("param1", paramValue)
                            .build())
        .body(BodyInserters.fromObject(user))
        .header(HttpHeaders.AUTHORIZATION, "Bearer " + token)
        .accept(MediaType.APPLICATION_JSON, MediaType.APPLICATION_XML)
        .acceptCharset(Charset.forName("UTF-8"))

    client2.post()
        .uri(uriBuilder -> uriBuilder
                            .path("/account-service")
                            .queryParam("param1", paramValue)
                            .build())
        .body(Mono.just(user), new ParameterizedTypeReference<>() {})
        .header(HttpHeaders.AUTHORIZATION, "Bearer " + token)
        .accept(MediaType.APPLICATION_JSON, MediaType.APPLICATION_XML)
        .acceptCharset(Charset.forName("UTF-8"))

**Handle response**

    String response2 = client1
                        .exchangeToMono(String.class)
                        .block();


    User user = client1
                .retrieve()
                .bodyToMono(new ParameterizedTypeReference<User>() {})
                .timeout(60000, Mono.error(new Exception("timeout")))
                .block();

**Add header**

```java
    webClient
        .filter((request, next) -> next.exchange(
                withBearerAuth(request, tokenEx)))
        .filter(logRequest())
        .filter(logResponse())
        .build();

    private static ClientRequest withBearerAuth(ClientRequest request, String token) {
        return ClientRequest.from(request)
                .header(HttpHeaders.AUTHORIZATION, "Bearer " + token)
                .build();
    }

    // This method returns filter function which will log request data
    private static ExchangeFilterFunction logRequest() {
        return ExchangeFilterFunction.ofRequestProcessor(clientRequest -> {
            log.info("Request: {} {}", clientRequest.method(), clientRequest.url());
            clientRequest.headers().forEach((name, values) -> values.forEach(value -> log.info("{}={}", name, value)));
            return Mono.just(clientRequest);
        });
    }

    // This method returns filter function which will log response data
    private static ExchangeFilterFunction logResponse() {
        return ExchangeFilterFunction.ofResponseProcessor(clientResponse -> {
            clientResponse.statusCode();
            if(clientResponse.statusCode().is5xxServerError() || clientResponse.statusCode().is4xxClientError()) {
                return clientResponse.bodyToMono(String.class)
                        .flatMap(errorBody -> Mono.error(new Exception("Error when send request")));
            }else {
                return Mono.just(clientResponse);
            }
        });
    }
```

## 2. RestTemplate

    RestTemplate restTemplate = new RestTemplate();
    String url = "https://api.example.com/users";
    User[] users = restTemplate.getForObject(url, User[].class);

    User[] users = restTemplate.getForEntity(url, User[].class);

    restTemplate.postForObject(url, request, Void.class); 

pom

```xml
    spring:
    application:
        name: my-application
    restTemplate:
        connect-timeout: 5000
        read-timeout: 5000
```

GET method

    getForEntity
    getForObject

POST method

    postForObject
    postForLocation
    postForEntity

PUT

    put

DELETE

    delete


**Call method**

    restTemplate.exchange( "/" + id,
            HttpMethod.DELETE,
            null,
            Void.class,
            id);



















