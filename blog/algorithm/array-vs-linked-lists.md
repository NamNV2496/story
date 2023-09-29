# Mảng (array), danh sách liên kết (linked list)

# Array truy cập `O(1)`, thêm/xóa `O(N)`, Linked list thêm/xóa `O(1)`, truy cập `O(N)`


Mảng và danh sách liên kết là 2 cấu trúc dữ liệu nền tảng cho tất cả các loại cấu trúc dữ liệu khác. Mảng và danh sách liên kết đều được dùng khi bạn muốn lưu nhiều dữ liệu (thường có cùng kiểu dữ liệu). Bảng dưới đây so sánh các thao tác về mảng và danh sách liên kết:

<img src="blog/algorithm/img/data-structures-overview.png" style="display: block; margin-right: auto; margin-left: auto;">

# **Mảng (Arrays)**

Mảng là một cấu trúc dữ liệu cực kỳ đơn giản và có thể xem như một danh sách với chiều dài cố định. Mảng là một cấu trúc dữ liệu “đẹp” bởi tính đơn giản của nó. Mảng đặc biệt thích hợp cho các tình huống mà ta biết trước được số lượng phần tử hoặc có thể xác định được khi chạy chương trình.

Giả sử bạn cần một đoạn mã để tính giá trị trung bình của một vài con số. Mảng là sự lựa chọn tuyệt vời để giữ các giá trị bởi yêu cầu bài toán không đòi hỏi một thứ tự lưu trữ cụ thể và các phép tính toán cũng không đòi hỏi gì khác ngoài việc duyệt qua toàn bộ các phần tử.

Một trong những sức mạnh khác của mảng chính là ta có thể truy cập các phần tử của mảng một cách ngẫu nhiên bằng chỉ số. Ví dụ như, bạn có một danh sách gồm tên của các học sinh trong lớp học. Mỗi học sinh đánh số từ 1 đến n. Khi đó để đọc hoặc lưu tên học sinh thứ i bạn chỉ cần gọi tới studentName[i]. Do các phần tử của mảng ở vị trí liên tiếp nhau trong bộ nhớ máy tính, nên thao tác này chỉ mất độ phức tạp O(1). Tuy nhiên cũng vì lý do này, nên việc tăng kích thước mảng hay thêm / xóa 1 phần tử vào vị trí bất kỳ của mảng có độ phức tạp O(N).

Mảng có số lượng phần tử cố định, mỗi phần tử giữ của mảng một thông tin và ở một vị trí không đổi đã được định nghĩa trước đó.

## Tổng kết

- Bộ nhớ cố định, cần biết trước số phần tử
- Truy cập một vị trí bất kỳ: O(1)
- Thêm / Xóa 1 phần tử vào cuối mảng: độ phức tạp trung bình: O(1)
- Thêm / xóa một phần tử: O(N)

# **Danh sách liên kết (Linked Lists)**

Danh sách liên kết là một cấu trúc dữ liệu có thể giữ một số lượng phần tử tùy ý và dễ dàng thay đổi kích thước, cũng như dễ dàng bỏ đi các phần tử bên trong nó.

Danh sách liên kết, hiểu theo cách đơn giản nhất là một con trỏ trỏ tới một nút dữ liệu. Mỗi nút dữ liệu bao gồm dữ liệu cần chứa và một con trỏ trỏ tới nút tiếp theo. Tại điểm cuối cùng, con trỏ trỏ tới giá trị NULL.

Với thiết kế như ban đầu, một danh sách liên kết thích hợp để lưu trữ dữ liệu khi chưa biết trước được số lượng các phần tử hoặc các phần tử thường xuyên thay đổi. Tuy vậy, chúng ta không thể truy cập một cách ngẫu nhiên các phần tử của danh sách liên kết. Để tìm kiếm một giá trị, ta phải bắt đầu tại phần tử đầu tiên và duyệt tuần tự qua các phần tử cho tới khi bắt gặp được giá trị mà mình cần tìm kiếm. Để chèn một nút vào danh sách liên kết, bạn cũng phải thực hiện tương tự. Độ phức tạp của cả 2 thao tác này là O(N). Tuy nhiên, nếu ta biết được con trỏ trỏ đến phần tử cần xóa, thì độ phức tạp chỉ là O(1). Dễ dàng nhận thấy, thao tác tìm kiếm và chèn trong danh sách liên kết không thật sự hiệu quả.

<img src="blog/algorithm/img/array-vs-linked-lists2.png" style="display: block; margin-right: auto; margin-left: auto;">

```C++
struct ListNode {
    int data; // dữ liệu được lưu ở nút của linked list
    ListNode* nextNode; // con trỏ trỏ tới phần tử tiếp theo của linked list.
};
ListNode* firstNode;
```
Bạn có thể chèn một nút mới vào bằng cách chèn chúng vào đầu danh sách. Thao tác này có độ phức tạp là O(1).

```C++
ListNode* newNode = new ListNode();
newNode->nextNode = firstNode;
firstNode = newNode;
```

Duyệt qua toàn bộ danh sách liên kết rất đơn giản như sau:

```C++
ListNode* curNode = firstNode;
while (curNode != NULL) {
   cout << curNode->data << endl;
   curNode = curNode->nextNode;
}
```

## Tổng kết

- Thêm / xóa 1 phần tử mới vào đầu / cuối: O(1)
- Truy cập 1 phần tử ở vị trí bất kỳ: O(N).