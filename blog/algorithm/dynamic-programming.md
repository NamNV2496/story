# Quy hoạch động

Các thuật toán  đệ quy có ưu điểm dễ cài đặt, tuy nhiên  do bản chất của đệ quy, các chương trình  thường kéo tho những đòi hỏi lớn về không gian bộ nhớ và 1 khối lượng tính toán khổng lồ

Quy hoạch động sinh ra là 1 kỹ thuật nhưng đơn giản hóa việc tính toán các công thức truy hồi bằng cách lưu trữ toàn bộ hay 1 phần kết quả tính toán tại mỗi bước với mục đích sử dụng lại. Bản chất của quy hoạch động là thay thế mô hình tính toán từ trên xuống bằng mô hình từ dưới lên.

Từ programing ở đây không liên quan đến lập trình cho máy tính, mà đó là thuật ngữ các nhà toán học dùng để chi ra các ước chung trong việc giải quyết một dạng bài toán hoặc một lớp các vấn đề. Không có 1 thuật toán tổng quát để giải tất cả các bài toán quy hoạch động. Mục đích của phần này là cung cấp một cách tiếp cận trong việc giải quyết các bài toán tối ưu mang bản chất **đệ quy**

## 1. Công thức truy hồi

### 1,1 Ví dụ

Cho số tự nhiên n <= 100. Hãy cho biết bao nhiêu cách phân tích số n thành tổng của các số nguyên dương, các hoán vị của nhau tính là 1 cách

Ví dụ: n=5 thì có 7 cách:

    1. 5=1+1+1+1+1
    2. 5=1+1+1+2
    3. 5=1+1+3
    4. 5=1+4
    5. 5=1+2+2
    6. 5=2+3
    7. 5=5

Với n=0 thì ra coi có 1 cách phân tích thành tổng các số nguyên dương.

Để giải bài toán này ta có thể dùng phương pháp liệt kê và đếm số cấu hình.
*Nhận xét:*
    Nếu gọi F[m,v] là số cách phân tích số v thành tổng các số nguyên dương <=m, khi đó cách phân tích số v thành các số nguyên dương <=m có 2 loại:

- loại 1: không chứa số m trong phép phân tích, khi đó số các phân tích loại này chính là số cách phân tích số v thành tổng các số nguyên dương < m. tức là phân tích số v thành tổng các số nguyên dương <=m-1 và nó chính là F[m,v-1].
- Loại 2: có chứa ít nhất 1 số m trong phép phân tích. khi đó nếu tỏng cách phân tích chúng ta bỏ số m đó đi, thì ta sẽ có được cách phân tích v-m thành tổng các số nguyên dương <=m. có nghía là về mặt số lượng , số cách phân tích loại này là F[m,v-m].

Trong trường hợp m>v thì rõ ràng làm loại 1, còn nếu m <=v thì có cả loại 1 và loại 2. Vì thế:
F[m,v] = F[m-1,v] nếu m>v
F[m,v] = F[m-1,v] + F[m,v-m] nếu m<=v
Ta có công thức xây dựng F[m,v] từ F[m-1,v] và F[m,v-m]. Công thức này được gọi là công thức truy hồi đưa việc tính F[m,v] về việc tính các F[m',v'] với dữ liệu nhỏ hơn. Tất nhiên cuối cùng ta quan tâm đến F[n,n] số cách phân tích n thành tổng các số nguyên dương <=n.


Với n=5 ta có bảng F sẽ là:



```java
int max=100;
int f[max+1][max+1];
int n,m,v;

int solve() {
    memset(f, 0, (max+1)*(max+1));
    f[0][0]=1;
    for (m=1; i<n; i++) {
        for (v=0; v<m; v++) {
            if (v<m) {
                f[m][v] = f[m-1][v];
            } else {
                f[m][v] = f[m-1][v] + f[m][v-m];
            }
        }
    }
    return f[n][n];
}
```
### 1.2 Cải tiến
Ta thấy rằng không gian bộ nhớ cần là `n^2`. Điều này sẽ gây khó khăn nếu n lớn.
Tuy nhiên dựa theo công thức trên khi tính hàng thứ n thì ta chỉ cần giá trị ở hàng n-1. Do đó ta cso thể cải tiến bộ nhớ bằng cách dùng mảng 1 chiều.

Việc dùng 2 mảng 1 chiều có thể làm tộc độ chậm đi (trong việc gán lại giá trị cho hàng thứ n-1 thành hàng n khi chuẩn bị tính hàng n+1) hoặc phức tạp trong cài đặt bằng cách đổi tham chiếu của 2 mảng. 

=> ta có thể cải tiến bằng cách dùng 1 mảng 1 chiều với loại dụng tham sốc tham chiếu của máy tính để tính giá trị kế tiếp. Khi đó công thức sẽ là `f[v]=f[v]+f[v-m]`;

```java
int max;
int f[max+1];
int n,m,v;
int solve() {
    memset(f, 0, (max+1)*(max+1));
    f[0]=1;
    for (m=1; i<n; i++) {
        for (v=m; v<n; v++) {
            f[v] = f[v] + f[-m];
        }
    }
    return f[n];
}

```

















