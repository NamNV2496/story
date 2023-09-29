# Auditing

Auditing lưu lại thông tin của người tạo, thời gian tạo, người sửa cuối cùng, thời gian sửa cuối cùng. Nó vô cùng hữu ích cho việc truy xuất thông tin trong dự án 

<img src="blog/java/img/auditing.png" style="display: block; margin-right: auto; margin-left: auto;">

in domain please extends `extends AuditTrail<String>`

```java
@Configuration
@EnableJpaAuditing(auditorAwareRef = "aware")
class JpaConfig {

    @Bean
    public AuditorAware<String> aware() {
//        Read username from token
        return new AuditorAwareImpl();
    // hard code return value of user id is Administrator
//        return () -> Optional.of("Administrator");
    }
}

@Service
public class AuditorAwareImpl implements AuditorAware<String> {

    @Autowired
    private WebUtil webUtil;
    @SneakyThrows
    @Override
    public Optional<String> getCurrentAuditor() {
        return Optional.of(webUtil.getToken());
    }
}

@MappedSuperclass
@EntityListeners(AuditingEntityListener.class)
public class AuditTrail<U> {
    @CreatedBy
    @Column(name = "created_by", updatable = false)
    private U createdBy;

    @CreatedDate
    @Column(name = "created_date", updatable = false)
    @Temporal(TemporalType.TIMESTAMP)
    private Date creationDate;

    @LastModifiedBy
    @Column(name = "last_modified_by")
    private U lastModifiedBy;

    @LastModifiedDate
    @Column(name = "last_modified_date")
    @Temporal(TemporalType.TIMESTAMP)
    private Date lastModifiedDate;
}
```

then when you work with data the history will save



# how to import a service to static class

## Step 1: define service normally

<img src="blog/java/img/auditing1.png" style="display: block; margin-right: auto; margin-left: auto;">


## Step 2: define import function `setMyConfig`

<img src="blog/java/img/auditing2.png" style="display: block; margin-right: auto; margin-left: auto;">

## Step 3: create a static class to import `StaticContextInitializer`


must use `@Component` and `@PostConstruct` to init

<img src="blog/java/img/auditing3.png" style="display: block; margin-right: auto; margin-left: auto;">

```java
    @PostConstruct
    public void init() {
        CacheUtils.setMyConfig(cacheService);
    }
```


Now we call use CacheUtils in everywhere

<img src="blog/java/img/auditing4.png" style="display: block; margin-right: auto; margin-left: auto;">



# Fake created_date

use `@PrePersist`, `@PreUpdate`, `@PreRemove` to handler action insert, update, delete

<img src="blog/java/img/auditing6.png" style="display: block; margin-right: auto; margin-left: auto;">

    curl --location 'http://localhost:8080/testFakeCreatedDate' \
    --header 'Content-Type: application/json' \
    --data '{
    "name": "testChange created_date"
    }'

<img src="blog/java/img/auditing5.png" style="display: block; margin-right: auto; margin-left: auto;">

The result:

    {
        "createdBy": "system",
        "creationDate": "1011-11-11",
        "lastModifiedBy": "system",
        "lastModifiedDate": "2023-05-19",
        "id": 3,
        "name": "testChange created_date"
    }