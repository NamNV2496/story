# Generic


Đặt tên kiểu tham số là rất quan trọng để học Genericics. Nó không bắt buộc, tuy nhiên chúng ta nên đặt theo quy ước chung để dễ đọc, dễ bảo trì. Các kiểu tham số thông thường như sau:

E- Element (phần tử – được sử dụng phổ biến trong Collection Framework)
K – Key (khóa)
V – Value (giá trị)
N – Number (kiểu số: Integer, Double, Float, …)
T – Type (Kiểu dữ liệu bất kỳ thuộc Wrapper class: String, Integer, Long, Float, …)
S, U, V … – được sử dụng để đại diện cho các kiểu dữ liệu (Type) thứ 2, 3, 4, …



```java
@Getter
@Setter
@AllArgsConstructor
@NoArgsConstructor
@Builder
public class FilterDTO {
    private String field;
    private String value;
}

private static <T, T1 extends FilterDTO> Specification<T> customQuery(List<T1> filter, int type) {
    List<Predicate> list = new ArrayList<>();
    return (root, cq, cb) -> {
        if (filter != null && !filter.isEmpty()) {
            filter.forEach(ft -> {
                if (!ft.getField().isEmpty() && !ft.getValue().isEmpty()) {
                    switch (ft.getField()) {
                        case "name":
                            list.add(cb.like(cb.lower(root.get(ft.getField())), "%" + ft.getValue().toLowerCase() + "%"));
                            break;
                        case "age":
                            list.add(cb.equals(root.get(ft.getField()), ft.getValue()));
                            break;
                        case "money":
                            list.add(cb.greaterThanOrEqualTo(root.get(ft.getField()), ft.getValue()));
                        case "type":
                            list.add(cb.in(root.get(ft.getField())).value(List.of(1, 2, 3, 4)));
                    }
                }
            }
        }
    }
}
```


```java
    public <T, R> T postSync(String baseUrl, String uri, R requestBody, Class<T> clazz, Integer attempt) {
    // baseUrl = "http://localhost:8080"
        Interger firstBackoff = 5; // 5 seconds
        return webClient.create(baseUrl)
                .post()
                .retrieve()
                .onStatus(HttpStatus::is4xxClientError, clientResponse -> {
                    log.error("Error from Client side system");
                    return Mono.error(new Exception("INTERNAL_ERROR"));
                })
                .body(BodyInserters.fromValue(requestBody))
                .bodyToFlux(clazz)
                // retry attempt times if request error
                .retryWhen(Retry.backoff(attempt, Duration.ofSeconds(firstBackoff))
                        .filter(this::is5xxServerError)
                        .onRetryExhaustedThrow((retryBackoffSpec, retrySignal) -> new Exception("MAX_RETRY_ATTEMPTS"))
                )
                .collectList()
                .block();
    }
```


```java
class Test<T> {
    // An object of type T is declared
    T obj;
    Test(T obj) { this.obj = obj; } // constructor
    public T getObject() { return this.obj; }
}
```




