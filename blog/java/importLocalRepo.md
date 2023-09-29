# **How to import local jar to your project or jar file which build from another project?**

# Có 2 cách để import local jar vào trong project
- 1 là import trong tiếp trong local folder
- 2 là import thông qua server online


## Ưu nhược điểm của từng phương thức
### +) **Phương thức 1:**
- **Ưu điểm**: ta có thể import từ 1 file `jar` bất kỳ từ đâu đó. Có thể ở trên mạng, hoặc từ 1 bên cung cấp nào đó mà họ chỉ cho ta file jar chứ không cho ta source.
- **Nhược điểm**: Ta phải có 1 file `jar` dạng chuẩn (tức là file dùng để làm library để import vào service khác, chứ không phải thực thi bình thường ta hay build bằng deploy hoặc package)

### +) **Phương thức 2:**

- **Ưu điểm**: load động mà không cần copy file `jar` vào thư mục trong project. Có thể dễ dàng cập nhật version bất cứ lúc nào

- **Nhược điểm**: ta phải có source của project import và cần config server (ở đây dùng Nexus)

=> tùy vào từng nhu cầu để chúng ta tự lựa chọn cách thức phù hợp


# **import local repository**
Example in:
- importLocalRepo
- demo

## create standard java library

<img src="blog/java/img/importLocalRepo1.png" style="display: block; margin-right: auto; margin-left: auto;">

run package to create library

<img src="blog/java/img/importLocalRepo2.png" style="display: block; margin-right: auto; margin-left: auto;">

the output in target folder

<img src="blog/java/img/importLocalRepo3.png" style="display: block; margin-right: auto; margin-left: auto;">

Lưu ý là file cần dùng là file không có `-exec`. về bản chất thì đây chỉ là file build ra các file có đuôi `.class` và package dạng chuẩn để chúng ta có thể import


Khác biệt giữa 2 file thường và file chạy (đuôi -exec)

<img src="blog/java/img/importLocalRepo22.png" style="display: block; margin-right: auto; margin-left: auto;">

## import standard java library to target project

copy the lib jar file to /lib folder

<img src="blog/java/img/importLocalRepo4.png" style="display: block; margin-right: auto; margin-left: auto;">

config in pom to import

## **Note**: ở đây cần lưu ý bắt buộc phải có `<scope>system</scope>` để load file dạng tĩnh mà ko cần compile (mặc định nếu ko khai báo là complie)

<img src="blog/java/img/importLocalRepo5.png" style="display: block; margin-right: auto; margin-left: auto;">

get groupId, artifactId, version from pom of importLocalRepo

<img src="blog/java/img/importLocalRepo7.png" style="display: block; margin-right: auto; margin-left: auto;">

now we can import and use package of importLocalRepo

<img src="blog/java/img/importLocalRepo6.png" style="display: block; margin-right: auto; margin-left: auto;">

# **import repository by maven from NEXUS**

Example in:
- importNexusServer
- demo


install nexus on docker

    docker pull sonatype/nexus3

<img src="blog/java/img/importLocalRepo8.png" style="display: block; margin-right: auto; margin-left: auto;">

**run on cmd**

    docker run -d -p 8081:8081 --name nexus sonatype/nexus3

<img src="blog/java/img/importLocalRepo10.png" style="display: block; margin-right: auto; margin-left: auto;">

Access `http://localhost:8081/`

<img src="blog/java/img/importLocalRepo11.png" style="display: block; margin-right: auto; margin-left: auto;">

access to get password

<img src="blog/java/img/importLocalRepo12.png" style="display: block; margin-right: auto; margin-left: auto;">

<img src="blog/java/img/importLocalRepo13.png" style="display: block; margin-right: auto; margin-left: auto;">

sign-in to web
<img src="blog/java/img/importLocalRepo14.png" style="display: block; margin-right: auto; margin-left: auto;">


## Config to deploy jar to Nexus
Copy `settings.xml` to .m2 folder

    C:\Users\{user}\.m2

trong file này ta sẽ config về link đến server nexus và authentication để upload và download file từ server về

<img src="blog/java/img/importLocalRepo17.png" style="display: block; margin-right: auto; margin-left: auto;">

Edit file with authorization

<img src="blog/java/img/importLocalRepo18.png" style="display: block; margin-right: auto; margin-left: auto;">

<img src="blog/java/img/importLocalRepo15.png" style="display: block; margin-right: auto; margin-left: auto;">

<img src="blog/java/img/importLocalRepo16.png" style="display: block; margin-right: auto; margin-left: auto;">

check maven on Nexus

<img src="blog/java/img/importLocalRepo19.png" style="display: block; margin-right: auto; margin-left: auto;">

## Import to demo app

<img src="blog/java/img/importLocalRepo20.png" style="display: block; margin-right: auto; margin-left: auto;">

# **Result**

<img src="blog/java/img/importLocalRepo21.png" style="display: block; margin-right: auto; margin-left: auto;">

