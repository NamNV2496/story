# Keycloak

## 1. Keycloak là gì?
Cơ bản Keycloak là một Open Source Identity và Access Management cho Modern Applications và Services. Dùng để quản lý nhận dạng và truy cập vào các ứng dụng/ dịch vụ.
Về hướng dẫn cài đặt, cách thêm realm, client, role, user trên giao diện web của Keycloak thì các bạn có thể check google hoặc tại đây: Link1 hoặc tại đây: Link2 ( Các bạn ấy viết rất đầy đủ và chi tiết về các thông số hiển thị trên giao diện của Keycloak)



## **Tính năng của Keycloak**
Sau phần giới thiệu ngắn gọn ở đoạn trước, t
ôi nghĩ đã đến lúc nói với bạn nhiều hơn về những gì Keycloak có thể làm được.

- **Hỗ trợ nhiều giao thức**
Hiện tại, Keycloak hỗ trợ ba giao thức khác nhau, đó là - OpenID Connect, OAuth 2.0 và SAML 2.0.
- **SSO**
Keycloak có hỗ trợ đầy đủ cho Đăng nhập một lần và Đăng xuất một lần.
- **Bảng điều khiển dành cho quản trị viên**
Keycloak cung cấp GUI dựa trên web, nơi bạn có thể “nhấp ra” tất cả các cấu hình mà phiên bản của bạn yêu cầu để hoạt động như bạn mong muốn.
- **Danh tính Người dùng và Quyền truy cập**
Keycloak có thể được sử dụng làm trình quản lý truy cập và nhận dạng người dùng độc lập bằng cách cho phép chúng tôi tạo cơ sở dữ liệu người dùng với các vai trò và nhóm tùy chỉnh. Thông tin này có thể được sử dụng thêm để xác thực người dùng trong ứng dụng của chúng tôi và bảo mật các phần của nó dựa trên các vai trò được xác định trước.
- **Đồng bộ hóa nguồn nhận dạng bên ngoài**
Trong trường hợp khách hàng của bạn hiện có một số loại cơ sở dữ liệu người dùng, Keycloak cho phép chúng tôi đồng bộ hóa với cơ sở dữ liệu đó. Theo mặc định, nó hỗ trợ LDAPvà Active Directorynhưng bạn có thể tạo tiện ích mở rộng tùy chỉnh cho bất kỳ cơ sở dữ liệu người dùng nào bằng cách sử dụng API lưu trữ người dùng Keycloak. Hãy nhớ rằng một giải pháp như vậy có thể không có tất cả dữ liệu cần thiết để Keycloak hoạt động đầy đủ, vì vậy hãy nhớ kiểm tra xem chức năng mong muốn của bạn có hoạt động hay không.
- **Môi giới nhận dạng**
Keycloak cũng có thể hoạt động như một proxy giữa người dùng của bạn và một số nhà cung cấp hoặc nhà cung cấp danh tính bên ngoài. Danh sách của họ có thể được chỉnh sửa từ Keycloak Admin Panel.
- **Nhà cung cấp danh tính xã hội**
Ngoài ra, Keycloak cho phép chúng tôi sử dụng Nhà cung cấp nhận dạng xã hội. Nó có hỗ trợ tích hợp Google, Twitter, Facebook, Stack Overflow nhưng cuối cùng, bạn phải cấu hình tất cả chúng theo cách thủ công từ bảng quản trị. Danh sách đầy đủ các nhà cung cấp nhận dạng xã hội được hỗ trợ và hướng dẫn cấu hình của họ có thể được tìm thấy trong tài liệu Keycloak.
- **Tùy chỉnh trang**
Keycloak cho phép bạn tùy chỉnh tất cả các trang được nó hiển thị cho người dùng của bạn. Các trang đó có .ftlđịnh dạng để bạn có thể sử dụng các HTML  đánh dấu và CSSkiểu cổ điển để làm cho trang phù hợp với kiểu ứng dụng và thương hiệu công ty của bạn. Bạn thậm chí có thể đặt các JStập lệnh tùy chỉnh như một phần của tùy chỉnh trang để khả năng xảy ra là vô hạn.
  


# Setup keycloak tool

Access to download: https://www.keycloak.org/downloads.html

<img src="blog/java/img/keycloak1.png" style="display: block; margin-right: auto; margin-left: auto;">

Để start Keycloak server sử dụng cho development, các bạn hãy mở Console trên Window hoặc Terminal trên Linux và macOS, đi đến thư mục bin của Keycloak rồi chạy câu lệnh sau:


    linux: bin/kc.sh start-dev
    Windows: bin/kc.bat start-dev

Cho môi trường production thì các bạn hãy sử dụng command sau các bạn nhé!:

    linux: bin/kc.sh start
    Windows: bin/kc.bat start

<img src="blog/java/img/keycloak2.png" style="display: block; margin-right: auto; margin-left: auto;">

Run done:

<img src="blog/java/img/keycloak3.png" style="display: block; margin-right: auto; margin-left: auto;">

Access: http://localhost:8080/

<img src="blog/java/img/keycloak4.png" style="display: block; margin-right: auto; margin-left: auto;">

Create an username/pasword

<img src="blog/java/img/keycloak5.png" style="display: block; margin-right: auto; margin-left: auto;">

Now, access Administrator console

<img src="blog/java/img/keycloak6.png" style="display: block; margin-right: auto; margin-left: auto;">

keycloak console screen

<img src="blog/java/img/keycloak7.png" style="display: block; margin-right: auto; margin-left: auto;">

Create new Realm: example `ldap`

<img src="blog/java/img/keycloak8.png" style="display: block; margin-right: auto; margin-left: auto;">

<img src="blog/java/img/keycloak9.png" style="display: block; margin-right: auto; margin-left: auto;">


Create user with group

<img src="blog/java/img/keycloak10.png" style="display: block; margin-right: auto; margin-left: auto;">


<img src="blog/java/img/keycloak11.png" style="display: block; margin-right: auto; margin-left: auto;">

Setting token

<img src="blog/java/img/keycloak12.png" style="display: block; margin-right: auto; margin-left: auto;">

How to get token of an account

<img src="blog/java/img/keycloak13.png" style="display: block; margin-right: auto; margin-left: auto;">


    curl --location 'http://localhost:8080/realms/ldap/protocol/openid-connect/token' \
    --header 'Content-Type: application/x-www-form-urlencoded' \
    --data-urlencode 'client_id=test' \
    --data-urlencode 'username=broker1' \
    --data-urlencode 'password=1' \
    --data-urlencode 'grant_type=password'


**to refresh token**

    curl --location 'http://localhost:8080/realms/ldap/protocol/openid-connect/token' \
    --header 'Content-Type: application/x-www-form-urlencoded' \
    --data-urlencode 'client_id=test' \
    --data-urlencode 'username=broker1' \
    --data-urlencode 'password=1' \
    --data-urlencode 'grant_type=refresh_token' \
    --data-urlencode 'refresh_token=eyJhbGciOiJIUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICI4ZmZlYjNmNi0yOGRlLTQ3YTYtODcxOS0xMjhmMzkxNTI0ZTQifQ.eyJleHAiOjE2ODM5NTk5MzEsImlhdCI6MTY4Mzk1ODEzMSwianRpIjoiZjZkMzk4M2EtOThlZC00OTEyLWI3M2MtODhhMWNhYmZhOTJjIiwiaXNzIjoiaHR0cDovL2xvY2FsaG9zdDo4MDgwL3JlYWxtcy9sZGFwIiwiYXVkIjoiaHR0cDovL2xvY2FsaG9zdDo4MDgwL3JlYWxtcy9sZGFwIiwic3ViIjoiZTQ2Nzk3NDMtYjJjMS00YWY0LThlZTEtYzQ5ZTk5YmUzOTgxIiwidHlwIjoiUmVmcmVzaCIsImF6cCI6InRlc3QiLCJzZXNzaW9uX3N0YXRlIjoiMjQ2OTEzZjEtYzdhOS00MGYwLWFkMDEtM2NiZmY3OTY5ZTgxIiwic2NvcGUiOiJwcm9maWxlIGVtYWlsIiwic2lkIjoiMjQ2OTEzZjEtYzdhOS00MGYwLWFkMDEtM2NiZmY3OTY5ZTgxIn0.euWQgrmq7sAQaZXiX8ORpd9IEZgj_B-oMpqu4E_p5Rw'


# setup java

add dependencys

```xml
<dependency>
    <groupId>org.keycloak</groupId>
    <artifactId>keycloak-spring-boot-starter</artifactId>
    <version>16.1.0</version>
</dependency>
<dependency>
    <groupId>org.keycloak</groupId>
    <artifactId>keycloak-admin-client</artifactId>
    <version>16.1.0</version>
</dependency>

# to use webclient to call
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-webflux</artifactId>
</dependency>
```
application.yml

```xml
# Keycloak settings
keycloak:
  realm: ldap
  auth-server-url: http://localhost:8080/auth
  ssl-required: none
  resource: demo-app
  use-resource-role-mappings: true
  bearer-only: true
  cors: true
  principal-attribute: preferred_username
```

To get token

    curl --location 'http://localhost:8081/test' \
    --header 'Cookie: JSESSIONID=44F3B32B43B260D933E10BEE9B04699C'

**to refresh token (need to change body) **

    curl --location 'http://localhost:8081/test2' \
    --header 'Content-Type: text/plain' \
    --header 'Cookie: JSESSIONID=44F3B32B43B260D933E10BEE9B04699C' \
    --data 'eyJhbGciOiJIUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICI4ZmZlYjNmNi0yOGRlLTQ3YTYtODcxOS0xMjhmMzkxNTI0ZTQifQ.eyJleHAiOjE2ODM5NjQ5NTcsImlhdCI6MTY4Mzk2MzE1NywianRpIjoiYmZlM2NhZTEtZjIyYS00OWMxLWFjYWEtNjQxNDczZGUwZTJkIiwiaXNzIjoiaHR0cDovL2xvY2FsaG9zdDo4MDgwL3JlYWxtcy9sZGFwIiwiYXVkIjoiaHR0cDovL2xvY2FsaG9zdDo4MDgwL3JlYWxtcy9sZGFwIiwic3ViIjoiZTQ2Nzk3NDMtYjJjMS00YWY0LThlZTEtYzQ5ZTk5YmUzOTgxIiwidHlwIjoiUmVmcmVzaCIsImF6cCI6InRlc3QiLCJzZXNzaW9uX3N0YXRlIjoiZTRjNTE5YmQtYWE1NC00MmYzLWFiMTYtMjI1Y2UzZjU3ZWE2Iiwic2NvcGUiOiJwcm9maWxlIGVtYWlsIiwic2lkIjoiZTRjNTE5YmQtYWE1NC00MmYzLWFiMTYtMjI1Y2UzZjU3ZWE2In0.ZpQAPjZguf-TP-8WrkPvi2RBGFlsEl16Iu7ZnPLuNEo'




Ref: https://huongdanjava.com/vi/keycloak

https://www.youtube.com/watch?v=vZdz2UoYef0&ab_channel=code215



