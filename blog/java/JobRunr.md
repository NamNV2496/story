# JobRunr

Trong dự án chắc chắn sẽ có các job cần chạy hàng ngày hoặc theo 1 khoảng thời gian cố định

Vậy ta có thể dùng [`@Scheduled`](blog-post-1.html?java=scheduler) như 1 bài viết trước mà tại sao lại phải dùng **jobrunr**???

Nhưng bạn đã gặp trường hợp định sẵn job sẽ chạy nhưng cần 1 điều kiện phải được thực hiện trước không?

**Ví dụ** : 9h hàng ngày job sẽ chạy nhưng có điều kiện là người vận hành phải đóng hết các giao dịch. Vậy là nếu người vận hành đi làm muộn chưa đóng thì 9h job được chạy sẽ check chưa thấy chạy và bỏ qua => Vậy là 1 job không được chạy ngày hôm đó. Mà job 9h này nhất định phải được thực hiện thì sao????

**Solution** : nếu 9h job check và thấy điều kiện chưa thỏa mãn thì sẽ delay job 10 phút sau sẽ chạy => đó là lý do cần dùng jobrunr

# Setup

pom.xml

```xml
    <dependency>
        <groupId>org.jobrunr</groupId>
        <artifactId>jobrunr-spring-boot-starter</artifactId>
        <version>5.2.0-RC1</version>
    </dependency>
```

application.yml

```yml
org:
  jobrunr:
    background-job-server:
      enabled: true
    job-scheduler:
      enabled: true
```


to config dashboard UI

```yml
org.jobrunr.dashboard.enabled=true 
org.jobrunr.dashboard.port=8000
```

## create a job

```java
    @Job(name = "The sample job without variable")
    public void executeJob() {
        execute("Hello world!");
    }

    @Job(name = "The sample job with variable")
    public void executeJob(String param) {
        execute("Hello world!" + param);
    }
```

import `import org.jobrunr.scheduling.JobScheduler;`


trigger a job and remove immediatly (a fire-and-forget job - one time job)

```java
jobScheduler.enqueue(() -> sampleJobService.executeJob());
jobScheduler.enqueue(() -> sampleJobService.executeJob("some string"));
```

add a job to list and will not remove after run

```java
// use cron express
    jobScheduler.scheduleRecurrently(id,
            cronJob.getCron(),
            () -> startJobService.executeJob(param));
// or
// use time
    jobScheduler.schedule(
			LocalDateTime.now().plusHours(5),
			() -> sampleJobService.executeJob());

```

### **Delete job**

```java
    jobScheduler.delete(id);
```
