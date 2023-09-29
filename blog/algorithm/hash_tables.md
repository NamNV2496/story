# Bảng băm (Hash Tables)

# tìm khóa `O(1)`, thêm/xóa `O(1)`


Bảng băm là một CTDL thường được sử dụng như một từ điển: mỗi phần tử trong bảng băm là một cặp (khóa, giá trị). Nếu so sánh với mảng, khóa được xem như chỉ số của mảng, còn giá trị giống như giá trị mà ta lưu tại chỉ số tương ứng. Bảng băm không như các loại từ điển thông thường - ta có thể tìm được giá trị thông qua khóa của nó.

Bảng băm hoạt động dựa trên hàm Hash: Hash là quá trình khởi tạo một giá trị khóa (thường là 32 bit hoặc 64 bit) từ một phần dữ liệu. Nó có thể là n
 bit đầu tiên của dữ liệu, n
 bit cuối cùng, giá trị mod cho một số nguyên tố nào đó. Dựa theo giá trị hash, dữ liệu được chia vào các bucket:


<img src="blog/algorithm/img/data-structures-overview5.png" style="display: block; margin-right: auto; margin-left: auto;">

- Tìm 1 khóa: O(1).
- Thêm / xóa 1 khóa: O(1).