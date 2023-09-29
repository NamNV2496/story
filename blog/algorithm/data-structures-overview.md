# Tổng quan về Cấu Trúc Dữ Liệu

# BST Binary Search Tree trung bình là `O(logN)`, worst `O(N)`

- CTDL lưu trữ: thường có các thao tác như thêm 1 phần tử, xóa 1 phần tử. Có thể có thêm các thao tác như tìm kiếm 1 phần tử.
- CTDL truy vấn: thường dùng cho các bài toán mà bạn phải duy trì một tập hợp các số và thực hiện 1 số truy vấn trên đó.
- CTDL xâu: dùng cho các bài tập Xử lý xâu.\

# 1. CTDL Lưu trữ
## 1.1 Mảng (array), danh sách liên kết (linked list)

Mảng và danh sách liên kết là 2 cấu trúc dữ liệu nền tảng cho tất cả các loại cấu trúc dữ liệu khác. Mảng và danh sách liên kết đều được dùng khi bạn muốn lưu nhiều dữ liệu (thường có cùng kiểu dữ liệu). Bảng dưới đây so sánh các thao tác về mảng và danh sách liên kết:

<img src="blog/algorithm/img/data-structures-overview.png" style="display: block; margin-right: auto; margin-left: auto;">


## 1.2. Stack, Queue, Deque

### 1.2.1. Stack

Stack là CTDL cho phép thực hiện các thao tác:

- Thêm 1 phần tử vào cuối CTDL
- Xóa 1 phần tử khỏi cuối CTDL

Cả 2 thao tác trên đều có độ phức tạp O(1). Chú ý ta chỉ có thể xóa phần tử ở cuối CTDL, nói cách khác là phần tử mà mới được thêm vào gần nhất. Vì vậy, Stack còn được gọi là FIFO (First In First Out).
Stack có cài đặt đơn giản và được sử dụng trong nhiều thuật toán như DFS, tìm chu trình Euler, tìm khớp của đồ thị.
Trong C++ STL, có sẵn kiểu dữ liệu stack.

### 1.2.2. Queue

Queue là CTDL cho phép thực hiện các thao tác:

- Thêm 1 phần tử vào cuối CTDL
- Xóa 1 phần tử khỏi đầu CTDL.
Cả 2 thao tác đều có độ phức tạp O(1). Chú ý ta chỉ có thể xóa phần tử ở đầu CTDL, nói cách khác là phần tử mà đã được thêm vào lâu nhất. Vì vậy, Stack còn được gọi là LIFO (Last In First Out).
Queue có cài đặt đơn giản và được sử dụng trong BFS.
Trong C++ STL, có sẵn kiểu dữ liệu queue.

### 1.2.3. Deque

Deque (Double Ended Queue), là CTDL tổng quát hơn của Stack và Queue. Nó cho phép:

- Thêm 1 phần tử vào đầu hoặc cuối CTDL.
- Xóa 1 phần tử khỏi đầu hoặc cuối CTDL.
Cả 2 thao tác đều có độ phức tạp O(1).

Deque được sử dụng trong một số thuật toán như:

- BFS 01
- Tìm Min/Max trên đoạn tịnh tiến.
Trong C++ STL, có sẵn kiểu dữ liệu deque.

## 1.3. Priority Queue - Heap

Heap là một cấu trúc dữ liệu cho phép thực hiện các thao tác:

- Thêm một phần tử, với độ phức tạp O(logN).
- Xóa một phần tử, với độ phức tạp O(logN).
- Tìm max của các phần tử, với độ phức tạp O(1).

## 1.4. Cây Tìm Kiếm Nhị Phân

Cây Tìm Kiếm Nhị Phân (BST Binary Search Tree) là một cây nhị phân có tính chất: Với mỗi giá trị trên đỉnh đang xét, giá trị của mọi đỉnh trên cây con trái luôn nhỏ hơn đỉnh đang xét và giá trị của mọi đỉnh trên cây con phải luôn lớn hơn đỉnh đang xét.

<img src="blog/algorithm/img/data-structures-overview3.png" style="display: block; margin-right: auto; margin-left: auto;">

Cây tìm kiếm nhị phân cho phép thực hiện các thao tác:

- Thêm 1 phần tử.
- Xóa 1 phần tử.
- Kiểm tra 1 phần tử có tồn tại hay không.
- Tìm phần tử đầu tiên lớn hơn hoặc bằng 1 giá trị x cho trước.
Trong trường hợp dữ liệu ngẫu nhiên, các thao tác trên có độ phức tạp trung bình là `O(logN)`. Tuy nhiên trong trường hợp xấu nhất, cây tìm kiếm nhị phân bị suy biến (thành 1 "đường thẳng"), thì độ phức tạp mỗi thao tác là `O(N)`.


## Bảng băm (Hash Tables)

Bảng băm là một CTDL thường được sử dụng như một từ điển: mỗi phần tử trong bảng băm là một cặp (khóa, giá trị). Nếu so sánh với mảng, khóa được xem như chỉ số của mảng, còn giá trị giống như giá trị mà ta lưu tại chỉ số tương ứng. Bảng băm không như các loại từ điển thông thường - ta có thể tìm được giá trị thông qua khóa của nó.

Bảng băm hoạt động dựa trên hàm Hash: Hash là quá trình khởi tạo một giá trị khóa (thường là 32 bit hoặc 64 bit) từ một phần dữ liệu. Nó có thể là n
 bit đầu tiên của dữ liệu, n
 bit cuối cùng, giá trị mod cho một số nguyên tố nào đó. Dựa theo giá trị hash, dữ liệu được chia vào các bucket:


<img src="blog/algorithm/img/data-structures-overview5.png" style="display: block; margin-right: auto; margin-left: auto;">

- Tìm 1 khóa: O(1).
- Thêm / xóa 1 khóa: O(1).

