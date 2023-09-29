# CQRS

## CQRS Design Pattern là gì?

<img src="blog/java/img/cqrs.png" style="display: block; margin-right: auto; margin-left: auto;">

CQRS là một trong những pattern rất quan trọng khi tiến hành truy xuất/ querying giữa các microservices. CQRS có thể giúp cho chúng ta tránh được những queries phức tạp, CQRS là viết tắt của Command and Query Responsibility Segregation. Nói một cách đơn giản nó chia tách các thao tác đọc và cập nhật cho DB.

Với các ứng dụng monolithic, hầu hết ta chỉ có duy nhất 1 DB, DB này vừa đảm nhận nhiệm vụ nhận các câu truy vấn phức tạp, vừa đảm nhận thêm nhiệm vụ CRUD. Thế nhưng khi ứng dụng trở nên phức tạp hơn thì kéo theo đó các câu queries cũng như CRUD operations cũng sẽ trở nên phức tạp lên theo, lúc đó sẽ rất khó để kiểm soát hành vi của hệ thống.

Lấy ví dụ với việc đọc dữ liệu, nếu câu query của chúng ta phức tạp đến mức cần phải join 10 bảng lại với nhau, thì thời gian lock DB lại sẽ lâu hơn và từ đó độ trễ trong tính toán của câu query cũng sẽ tăng lên theo. Cũng tương tự như với thao tác ghi lên DB, khi thực thi các CRUD operations ta cần các validations phức tạp khiến cho thời gian xử lí tăng theo, dẫn đến thời gian lock DB cũng theo đó mà tăng lên.

<img src="blog/java/img/cqrs1.png" style="display: block; margin-right: auto; margin-left: auto;">

Với CQRS ta sẽ sử dụng 2 DB khác nhau cho reading và writing với các "chiến lược" khác nhau dành cho chúng.

Hơn nữa, ta cũng cần cân nhắc yêu cầu của hệ thống để đưa ra cách thiết kế phù hợp, với các ứng dụng "chuyên đọc" - read-incentive application ta nên thiết kế kiến trúc hệ thống sao cho nó tập trung cao độ vào reading DB.

- Command trong CQRS sẽ tập trung vào việc cập nhật dữ liệu (VD: thêm item vào cart, checkout order, ...), do đó commands nên được xử lí với message broker systems cung cấp cách thức xử lí commands một cách bất đồng bộ.

- Queries thì không chỉnh sửa gì dữ liệu cả, nó chỉ trả về JSON data với DTO objects.

Để có thể tách bạch hoàn toàn Commands và Queries ta sẽ tiến hành chia DB thành 2 DB phân biệt nhau. Bằng cách này, nếu ứng dụng của chúng ta là một ứng dụng "chuyên đọc", ta sẽ định nghĩa các custom schema để tối ưu hoá các câu queries.

Materialized view pattern là một ví dụ tốt cho việc triển khai reading DB, bằng cách này chúng ta có thể tránh được các phép JOINS phức tạp và có thể mapping với các schema được định nghĩa trước chuyên dùng cho thao tác query.

## Làm cách nào để đồng bộ các DBs với CQRS ?

Khi tiến hành chia tách thành 2 DBs thì có một vấn đề mà ta cần lưu tâm đó chính là việc đồng bộ hoá dữ liệu giữa chúng.

Việc này có thể được triển khai bằng cách ứng dụng Event-Driven Architecture. Với Event-Driven Architecture, khi update trong main DB, ta sẽ tiến hành publish ra một update event sử dụng message broker systems, event này sẽ được tiêu hoá bởi read DB, sau đó quá trình sync data sẽ được tiến hành.

Tuy nhiên, một vấn đề khác sẽ phát sinh ở đây, đó là consistency issue. Vì chúng ta triển khai async communication với message broker, nên dữ liệu sẽ không được phản ánh ngay lập tức.

Từ đó ta đưa ra nguyên tắc eventual consistency. Read DB sẽ "đều đặn" đồng bộ hoá dữ liệu từ write database, nó sẽ mất một khoảng thời gian nhất định để cập nhật read DB một cách "bất đồng bộ". Chúng ta sẽ cùng nhau thảo luận về eventual consistency ở các phần tiếp theo.

Khi bắt đầu thiết kế, bạn có thể sử dụng read database bằng cách replicate từ write database. Bằng cách này chúng ta có thể sử dụng các read-only replicas khác nhau bằng việc áp dụng Materialized view pattern để từ đó tăng hiệu năng của câu query.

Hơn nữa, do ta tiến hành chia tách thành 2 DBs nên chúng hoàn toàn có thể được scale một cách độc lập với nhau tuỳ theo yêu cầu của hệ thống.

Việc sử dụng Event-driven architecture để đồng bộ hoá dữ liệu đòi hỏi chúng ta cần phải biết đến Event sourcing pattern.

## Thiết kế kiến trúc - CQRS, Event sourcing, Eventual Consistency, Maerilized View
Tôi tiến hành thiết kế kiến trúc cho ứng dụng EC của mình như sau:

<img src="blog/java/img/cqrs2.png" style="display: block; margin-right: auto; margin-left: auto;">

Với Ordering service, ta sẽ chia thành 2 nhóm DB:

- Nhóm 1: DB cho writing với bản chất là RDB.
- Nhóm 2: DB cho reading dùng cho querying.
Ta sẽ tiến hành đồng bộ hoá dữ liệu giữa 2 nhóm DB này bằng message broker system thông qua publish/ subscribe pattern

Và đây chính là tech stack tôi sẽ sử dụng cho ứng dụng EC của mình:

- SQL Server cho RDB (writing DB)
- Cassandra cho NoSQL DB (reading DB)
- Kafka cho việc đồng bộ dữ liệu 2 DBs


Nguồn copy: https://viblo.asia/p/cqrs-design-pattern-trong-kien-truc-microservices-PAoJexlZ41j