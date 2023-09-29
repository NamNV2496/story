# Authentication Oauth2
## 1. Oauth2


OAuth2 hay Open Authentication thực ra nó là một phương thức chứng thực, xây dựng ra để các ứng dụng có thể chia sẻ tài nguyên cho nhau mà không cần chia sẻ thông tin tài khoản (username và password). OAuth2 đảm nhận 2 việc Authentication (Xác thực người dùng) và Authorization (Ủy quyền truy cập vào tài nguyên)

<img src="blog/java/img/oauth1.png" style="display: block; margin-right: auto; margin-left: auto;">

Nhìn vào hình ảnh trên, ta có thể thấy giao thức OAuth2 được thực hiện qua 2 bên là Service API - cung cấp dịch vụ xác thực và ủy quyền sử dụng tài nguyên và Application - ứng dụng bên thứ 3 muốn lấy tài nguyên của Resource Owner. Ngoài ra, còn có một số khái niệm quan trọng như:

- Resource Owner: Là những người muốn ủy quyền cho phép Application truy cập vào tài khoản của mình. Application có thể lấy thông tin nhưng sẽ bị giới hạn bởi scope được cấp phép.
- Authorization Server: Có nhiệm vụ kiểm tra và xác thực thông tin đăng nhập của user, sau đó cung cấp một chuỗi gọi là Access Token để Client có thể xác thực về sau.
- Resource Server: Là nơi lưu trữ tài nguyên của User và bảo mật tài nguyên cho User.

## **Luồng hoạt động**
1. Application gửi yêu cầu ủy quyền truy cập vào Resource Server.
2. Nếu User ủy quyền cho phép Application truy cập vào tài nguyên, thì sẽ nhận được một "giẩy ủy quyền" từ User.
3. Application gửi thông tin đăng nhập kèm theo "giẩy ủy quyền" tới Authorization Server.
4. Nếu thông tin gửi đi từ Application là hợp lệ, Authorization Server sẽ trả về một đoạn chuỗi gọi là access_token.
5. Application muốn truy cập tài nguyên của Resource Server thì phải gửi yêu cầu kèm theo đó là chuỗi access_token vừa nhận được.
6. Nếu access_token hợp lệ, Resource Server sẽ trả về tài nguyên mà Application đang yêu cầu.

## Authorization Grant

Để lấy được access_token thì phải qua bước ủy quyền và xác thực. Loại ủy quyền (Authorization Grant) phụ thuộc vào phương thức mà Application sử dụng ủy quyền. Đối với OAuth2, định nghĩa có 4 loại ủy quyền sau:

- Authorization Code: Thường sử dụng trong các server-side application.
- Resource Owner Password Credentials: Sử dụng với các ứng dụng được tin cậy
- Client Credentials: Sử dụng truy cập với các ứng dụng thông qua API
- Implicit: Sử dụng trong các Mobile App hoặc Web App



## IMPORTANT: must change etc/hosts

access: `C:\Windows\System32\drivers\etc`

edit `hosts`: add `127.0.0.1 auth-server`


## Step 1: run 3 services


<img src="blog/java/img/oauth2.png" style="display: block; margin-right: auto; margin-left: auto;">

## Step 2: call url

    http://127.0.0.1:8080/articles

the Resource will redirect to `http://auth-server:9000/login`

<img src="blog/java/img/oauth3.png" style="display: block; margin-right: auto; margin-left: auto;">


login with: `admin/password`

after that, the page will auto redirect to destination url


<img src="blog/java/img/oauth4.png" style="display: block; margin-right: auto; margin-left: auto;">

Further requests to the articles endpoint won't require logging in, as the access token will be stored in a cookie.


Ref: https://www.baeldung.com/spring-security-oauth-auth-server


Ref: https://www.tutorialspoint.com/spring_boot/spring_boot_google_oauth2_sign_in.htm

