# Cây Tìm Kiếm Nhị Phân

Cây Tìm Kiếm Nhị Phân (BST Binary Search Tree) là một cây nhị phân có tính chất: Với mỗi giá trị trên đỉnh đang xét, giá trị của mọi đỉnh trên cây con trái luôn nhỏ hơn đỉnh đang xét và giá trị của mọi đỉnh trên cây con phải luôn lớn hơn đỉnh đang xét.

<img src="blog/algorithm/img/data-structures-overview3.png" style="display: block; margin-right: auto; margin-left: auto;">

Cây tìm kiếm nhị phân cho phép thực hiện các thao tác:

- Thêm 1 phần tử.
- Xóa 1 phần tử.
- Kiểm tra 1 phần tử có tồn tại hay không.
- Tìm phần tử đầu tiên lớn hơn hoặc bằng 1 giá trị x cho trước.
Trong trường hợp dữ liệu ngẫu nhiên, các thao tác trên có độ phức tạp trung bình là `O(logN)`. Tuy nhiên trong trường hợp xấu nhất, cây tìm kiếm nhị phân bị suy biến (thành 1 "đường thẳng"), thì độ phức tạp mỗi thao tác là `O(N)`.

Để khắc phục điều này, có rất nhiều CTDL cải tiến từ cây tìm kiếm nhị phân, thường được gọi là các cây nhị phân cân bằng. Khi đó, các thao tác trên có thể được thực hiện với độ phức tạp `O(logN)`. Ví dụ:

- Cây Đỏ Đen (Red-Black Tree) là một dạng cây tìm kiếm nhị phân (BST) mà sau mỗi truy vấn được thực hiện, cây tự cân bằng theo đúng tính chất của nó với độ phức tạp O(log(N)). CTDL set trong C++ được cài đặt bằng cây đỏ đen.

<img src="blog/algorithm/img/data-structures-overview4.png" style="display: block; margin-right: auto; margin-left: auto;">

