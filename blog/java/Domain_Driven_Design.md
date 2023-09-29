# Domain Driven Design

DDD (Domain Driven Design) là một cách thiết kế phần mềm "hướng nghiệp vụ" - tức sẽ lấy nghiệp vụ (domain) làm trung tâm của hệ thống.

Có thể ví dụ với hệ thống "nghiệp vụ ngân hàng" hoặc "nghiệp vụ kho vận". Nếu chỉ nói về mặt khái niệm thì sẽ rất khó hình dung DDD là gì nên do đó trong bài viết này tôi sẽ cố gắng minh hoạ từng khái niệm của DDD tương ứng với code API của mình.

# Một vài khái niệm cơ bản trong DDD

Vì là "domain" driven design nên domain là khái niệm chính ở đây. Xoay quanh khái niệm về domain ta có 3 key words chính đó là Domain, Model và Domain Model

Domain là một lĩnh vực nghiệp vụ nào đó ví dụ như "nghiệp vụ ngân hàng" hoặc "nghiệp vụ kho vận", ...

Model là quá trình trừu tượng hoá các đối tượng cũng như các thao tác của nghiệp vụ. Quá trình mô hình hoá nghiệp vụ thành các đối tượng mang tính trừu tượng này được gọi là quá trình Modeling hoặc Mô hình hoá.

Domain Model là các models ta thu được sau quá trình Modeling ở trên.

Trở lại với API ở trên, domain của tôi chính là việc người dùng có khả năng đăng bài và theo dõi các người dùng khác. Tôi sẽ mô hình hoá thành 2 models chính là:

User
Post
đây cũng chính là hai "đối tượng" chính trong API của tôi. Minh hoạ rõ ràng hơn cho 2 models trên tôi có sơ đồ Domain Model như sau:

<img src="blog/java/img/ddd1.png" style="display: block; margin-right: auto; margin-left: auto;">


Nhìn qua sơ đồ này bạn đọc có thể thấy được:

- Các thuộc tính cơ bản của post: content, imageUrls, ...
- Các thuộc tính cơ bản của user: email, userName, password
- Mối quan hệ 1-n giữa User-Post

Nó đúng nhưng vẫn còn thiếu rất nhiều, ví dụ như:

- Comment
- Like
- Follow
User detail (lưu thông tin chi tiết của user như: firstName, lastName, age, ...)
Không những thế trong DDD chúng ta còn có một khái niệm rất quan trọng khác đó là Aggregate - Kết tập. Giải thích một cách đơn giản thì aggregate (kết tập) là: một tập hợp các objects có liên quan chặt chẽ với nhau về mặt dữ liệu mà ta luôn phải đảm bảo điều đó.

<img src="blog/java/img/ddd2.png" style="display: block; margin-right: auto; margin-left: auto;">

<img src="blog/java/img/ddd3.png" style="display: block; margin-right: auto; margin-left: auto;">

Example

```java
class UserEntity extends BaseEntity {
  detail: UserDetailEntity;
}

class UserDetailEntity extends BaseEntity {
  id?: number;
  active: boolean;
  nickName: string;
  avatarURL: string;
  gender: UserDetailGender;

  constructor() {
    super();
  }
}
```
Bản thân từng method được định nghĩa trong các Aggregate class sẽ thể hiện cho nội dung của nghiệp vụ mà hệ thống đang tiến hành mô hình hoá, ở ví dụ trên đó là updateDetail.

Phân tích tương tự với Post Aggregate tại source code. Post Aggregate với Post object là root aggregate, các objects Comment & Like sẽ là các objects con được "kết tập" bên trong Post Aggregate thông qua các thuộc tính của class như sau:

```java 
class PostEntity extends BaseEntity {
  likes: LikeEntity[];
  comments: CommentEntity[];
}

class CommentEntity extends BaseEntity {
  id?: number;
  content: string;
  userId: number;
  postId: number;
}

class LikeEntity extends BaseEntity {
  id?: number;
  userId: number;
  postId: number;
}
```

Nguồn copy: https://viblo.asia/p/toi-da-xay-dung-mot-api-don-gian-bang-ddd-nhu-the-nao-phan-1-018J263aJYK?fbclid=IwAR0-tgmZpAvzjHUZpav0ojg2G79J5XMxB4MQYgyoMfwuM_m7zgUqjVEoPWU





