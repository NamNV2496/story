# Thuật toán vét cạn

## 1. Định nghĩa
Trong khoa học máy tính, vét cạn (brust-force search hoặc exhaustive search), còn được gọi là  tạo và kiểm tra, là một kỹ thuật giải quyết vấn đề rât chung à mô hình thuật toán bao gồm liệt kê một cách có hệ thống tất cả các ứng viên có thể có cho giải pháp và kiểm tra xem mỗi ứng viên có đáp ứng yêu cầu bài toán không?

Mặc dù vét cạn rất dễ thực hiện và luôn tìm ra giải pháp nếu nó tồn tại, nhưng chi phí thực hiện tỷ lệ thuận với số lượng giải pháp, các vấn đề trong thực tế xó xu hướng tăng rất nhanh số lượng giải pháp khi quy mô của vấn đề tăng lên. Do đó, vét cạn thường được sử dụng khi kích thước vét cạn hạn chế hoặc các kinh nghiệm vấn dề có thể là giảm số lượng giải pháp xuống mức chấp nhận đc.

## **Thuật toán**
```C++
c <- first(P)
while c != null do
    if valid(P, c) then
        output(P, c)
    c <- next(P, c)
end while
```
1. Xác định candidate solution c đầu tiên cho P
2. Kiểm tra xem c có phải `null candidate` không?
3. Nếu c là giải pháp hợp lệ của P hay không
    - in ra giải pháp
    - else sinh ra candidate solution c tiếp theo

### Ví dụ

Tìm số lớn nhất trong dãy số nguyên cho trước (-5, 2, -1, 0, 3, 1)

|   Step       |    explanation |
|----------- |-----------|
|   c <- first(P)  | c = -5  |
|   while c != null do  |    c != null  |
|   if valid(P, c) then |   valid(P, c) == true |
|   output(P, c)    |   max = -5    |
|   c <- next(P,c)  |   c = 2   |
|   while c != null do  |    c != null  |
|   if valid(P, c) then |   valid(P, c) == true |
|   output(P, c)    |   max = 2    |
|   c <- next(P,c)  |   c = -1   |
|   while c != null do  |    c != null  |
|   if valid(P, c) then |   valid(P, c) == false |
|   c <- next(P,c)  |   c = 0   |
|   while c != null do  |    c != null  |
|   if valid(P, c) then |   valid(P, c) == false |
|   c <- next(P,c)  |   c = 3   |
|   while c != null do  |    c != null  |
|   if valid(P, c) then |   valid(P, c) == true |
|   output(P, c)    |   max = 3    |
|   c <- next(P,c)  |   c = 1   |
|   while c != null do  |    c != null  |
|   if valid(P, c) then |   valid(P, c) == false |
|   c <- next(P,c)  |   c = null   |
|   while c != null do  |   c == null => end  |

```java 
public Integer test() {
    List<Integer> arr = List.of(-5, 2, -1, 0, 3, 1);
    Integer c = arr.get(0);
    for (int i=1; i<arr.size(); i++) {
        if (c < arr.get(i)) {
            c = arr.get(i);
        }
    }
    return c;
}
```
## 2. Tăng tốc vét cạn

### 2.1 Giảm không gian tìm kiếm

Một cách để tăng tốc thuật toán vét cạn là giảm không gian tìm kiếm, tức là tập hợp các giải pháp ứng viên, bằng cách sử dụng phương pháp kinh nghiệm. việc một chút phân tích thường sẽ dẫn đến việc giảm đáng kể số lượng các khả năng và có thể biến một vấn đề tầm thường.

Trong một số trường hợp, phân tích có thể giảm các khả năng xuống thành tập hợp tất cả các khả năng hợp lệ, nghĩa là có thể có một thuật toán liệ kê trực tiếp tất cả các giải pháp hợp lệ, mà không lãng phí thời gian với phần kiểm tra và tạo ra các khả năng không hợp lệ.

```text
    Ví dụ: tìm tất cả số nguyên từ 1 đến 1.000.000 chia hết cho 417,  Một giải pháp vét cạn sẽ ra tất cả các số nguyên trong phạm vi, kiểm tra tính chia hết của số đó cho 417 không.
    Tuy nhiên, vấn đề đó có thể giải quyết hiệu quả hơn bằng cách bắt đầu với 417 và liên tục thêm 417 cho đến khi số đó vượt quá 1.000.000 => chỉ mất 2398 (1.000.000 / 417) bước.
```

### 2.2 Sắp xếp lại không gian tìm kiếm

Trong các bài toán chỉ yêu cầu một giải pháp, thay vì tất cả các giải pháp, thời gian dự kiến chạy của vét cạn thường sẽ phụ thuộc vào thứ tự mà các khả năng được kiểm tra. Theo nguyên tắc chung, người ta nên kiểm tra những khả năng có triển vọng nhất trước. Và thường thì xác suất cảu một khả năng hợp lệ thường bị ảnh hưởng bởi các khả năng thất bại trước đó.

```text
    Ví dụ: Khi tìm kiếm ước riêng của một số ngẫu nhiên n, tốt hơn nên liệt kê các ước ứng viên theo thứ tự tăng dần, và từ `2` đến `n-1`, hoặc ngược lại - bởi vì xác xuất n chia hết cho c là 1/c
```

## 3. Sự bùng nổ của các bài toán tổ hợp

Nhược điểm chính của phương pháp vét cạn là đối vưới nhiều bài toán trong thế giới thực, số lượng khả năng là rất lớn. Hãy xem xét:

**Bài toán 1:** Cho n thành phố đánh số từ 1 đến n và các tuyến đường giao thông hai chiều giữa chúng, mạng lưới giao thông này được cho bởi mảng C[1…n, 1…n] ở đây C[i][j] = C[j][i] là chi phí đi đoạn đường trực tiếp từ thành phố I đến thành phố j.Một người du lịch xuất phát từ thành phố 1, muốn đi thăm tất cả các thành phố còn lại mỗi thành phố đúng 1 lần và cuối cùng quay lại thành phố 1. Hãy chỉ ra chi phí ít nhất mà người đó phải bỏ ra.

    https://www.tutorialspoint.com/design_and_analysis_of_algorithms/design_and_analysis_of_algorithms_travelling_salesman_problem.htm

## 4. Phương pháp sinh (Generation)

Để thực hiện phương pháp vét cận cần phương pháp sinh ra các khả năng. Phương pháp sinh có thể mô tả như sau:

``` txt
<Xây dựng cấu hình đầu tiên>
repeate
    <Đưa ra cấu hình đang có>
    <Từ cấu hình đang có, đưa ra cấu hình kế tiếp>
util;
```

Với cơ sở lý thuyết tất cả các cấu hình (candidate) là độc nhất. Do đó tồn tại 1 thứ tự cho việc sắp xếp các cấu hình này và được gọi là **thứ tự từ điển**

**Thứ tự từ điển đối với các tập hợp có cùng độ dài**
```text
    Ví dụ: các tập con của 3 phần từ (1, 2, 3)
    1. (1,2,3)
    2. (1,3,2)
    3. (2,1,3)
    4. (2,3,1)
    5. (3,1,2)
    6. (3,2,1)
```
**Thứ tự từ điển đối với các tập hợp không cùng độ dài**
```text
    Ví dụ: 
    1. ()
    2. (1)
    3. (1,2)
    4. (1,2,3)
    5. (2)
    6. (2,3)
    7. (3)
```
### 4.1 Liệt kê các hoán vị (bài toán 1)

Ta sẽ lập chương trình liệt kê các hoán vị của {1,2,3,...} theo thứ tự từ điển:
Ví dụ: n=3
```text
    1, 123  2, 132  
    3, 213  4, 231
    5, 312  6, 321
```
Như vậy ta có hoán vị đầu tiên là {1,2,3,...,n} và hoán vị cuối cùng là {n,...,3,2,1}
Hoán vị sẽ sinh phải lớn hơn hoán vị hiện tại, hơn nữa phải là hoán vị **vừa đủ lớn hơn** hoán vị hiện tại nghĩa là không thể có 1 hoán vị nào chen vào giữa chúng khi sắp xếp thứ tự.
```text
    Ví dụ: x=(3,2,6,5,4,1) xét 4 phần tử cuối là các số giảm dần => cho dù ta hoán vị 4 phần tử này thế nào cũng sẽ chỉ đc 1 số nhỏ hơn số hiện tại
    Như vậy ta phải xét đến phần tử thứ 2 từ trái qua x2 = `2`
    - case 1: x2 được thay thế bằng 1 số < `2` vì nó sẽ tạo ra số nhỏ hơn (hiện tại 3`2`6541, số thay thế 3`1`6541) => fail
    - case 2: x2 được thay thế bằng 1 số > `2`  nhưng không được == `3` (vì trùng với giá trị của x1=3 từ trái sang) => các giá trị được thay thế là `4`, `5`, `6`
    Vì cần 1 giá trị **vừa đủ lớn hơn** giá trị hiện tại nên x2 được thay thế bằng `4`. các giá trị x3, x4, x5, x6 sẽ là hoán vị của {`2`, `6`, `5`, `1`}
    Vì tính vừa đủ thì ta sẽ cần sinh ra hoán vị của x3, x4, x5, x6 là (`1`,`2`,`5`,`6`)
    => Hoán vị mới: (`3`,`4`,`1`,`2`,`5`,`6`)
```
=> Ta có nhận xét:
- Ban đầu khi đề bài đưa ra thì đoạn cuối là hoán vị `được sắp xếp giảm dần`
- Sau khi hoán vị x2=2 vs x5=4 thì output là: 3,4,6,5,2,1 vẫn `được sắp xếp giảm dần`
- Sau khi đổi chỗ x2 vs x5 thì đoạn cuối cần đảo ngược lại để lấy được số **vừa đủ lớn hơn** 
```text
    Ví dụ: (2,1,3,4)
    Ta thấy x3=3, x4=4 là sắp xếp tăng dần => số vừa đủ lớn hơn là (2,1,4,3)
```
**Kỹ thuật sinh hoán vị kế tiếp từ hoán vị hiện tại:**

1. xác định đoạn cuối giảm dần dài nhất, tìm chỉ số i của phần từ x(i) đứng liền trước đoạn cuối đó.
```java
    i = n-1
    while ((i>0) && (x[i] > x[i+1])) --i;
```
2. Trong đoạn cuối giảm dần, tìm phần từ x(k) sao cho thỏa mãn điều kiện x(k) > x(i). do đoạn cuối giảm dần, ta cũng có thể tìm chỉ số k bằng cách tìm cuối dãy lên đầu và dừng ở chỉ số k đầu tiên thỏa mãn x(k) > x(i).
```java
    k = n;
    while(x[k] < x[i]) --k;
```
3. đổi chỗ x[k] với x[i] và lật ngược đoạn cuối căng dần
```java
    swap(x[i, x[k]]);
    a = i + 1;
    b = n;
    while(a<b>) {
        swap(x[a], x[b]);
        ++a;
        --b;
    }
```

### 4.2 Liệt kê các tập con k phần tử (bài toán 2)
```text
    Ví dụ: Cho tập hợp n={1,2,3,4,5} liệt kê các tập con với k=3

    1, {1,2,3}  2, {1,2,4}  3, {1,2,5}
    4, {1,3,4}  5, {1,3,5}
    6, {1,4,5}
    7, {2,3,4}  8, {2,3,5}  9,{2,4,5}
    10, {3,4,5}
```
Ta nhận thấy cấu hình kết thúc dạng {n-k+1, n-k+2, ..., n}
- Các tập con có phân tử được sắp xếp tăng dần -> giới hạn trên của các phần tử thứ i sẽ là **x(i)= n-k+i**
- Các tập con có phân tử được sắp xếp tăng dần -> giới hạn dưới của các phần tử thứ i sẽ là **x(i)= x(i-1)+1**
- giới hạn trên của các phần tử **x(i) = n-k+1** => kết thúc
- dãy mới sinh ra phải **vừa đủ lớn hơn** giá trị hiện tại

```text
**Ví dụ:** n =9, k=6, cấu hình đang có {1,2,6,7,8,9}
- ta thấy x3=6, x4=7, x5=8, x6=9 đạt giá trị tối đa nên ta không thể sử dụng cách tăng giá trị của 1 phần tử x3, x4, x5, x6 lên 1
- ta chỉ còn cách tăng giá trị x2=2 lên 1 => giá trị mới của x2=3 => cấu hình mới {1,3,6,7,8,9}
- nhưng giá trị này chưa thỏa mãn giá trị vừa đủ lớn:
    x3= x2+1 = 4
    x4= x3+1 = 5
    x5= x4+1 = 6
    x6= x5+1 = 7
=> cấu hình mới {1,3,4,5,6,7} đây là cấu hình kế tiếp
- để tìm câu hình kế tiếp, ta thấy x6=7 chưa phải giá trị max nên ta tăng x6 lên 1
    x6 = x6 + 1 = 7 + 1 = 8
=> cấu hình mới {1,3,4,5,6,8} 
```

**Kỹ thuật sinh tập con**

1. Tìm từ cuối lên đầu dãy cho đến khi phần tử x(i) chưa đạt giới hạn trên `n-k+1`
```java
    i=n;
    while((i>0) && (x[i] = n-k+1)) --i;
    if (i==0) countinue; // lùi đến 0 thì kết thúc
```

2. Tăng x(i) lên một đơn vị
```java
    ++x[i];
```

3. đặt lại tất cả các phần tử phía sau bằng giới hạn dưới
```java
    for(j=i+1; j<= k; ++j)
        x[j] = x[j-1] + 1;
```

### 4.3 Liệt kê các tập con k phần tử khác nhau (bài toán 3)

Bài toàn này bao gồm bài toán 1 và bài toán 2
```text
    1, {1,2,3}  2, {1,2,4}  3, {1,2,5}
    4, {1,3,4}  5, {1,3,5}
    6, {1,4,5}
    7, {2,3,4}  8, {2,3,5}  9,{2,4,5}
    10, {3,4,5}
```

### 4.3 Liệt kê các dãy nhị phân (bài toán 4)

x={x1, x2, x3, ..., xn}
```text
    000, 001, 010, 011, 100, 101, 110, 111
```
Nhận xét thấy rằng số tiếp theo được sinh ra bằng cách cộng thêm 1 theo cơ số 2 có nhớ
```text
    ví dụ: 
    số hiện tại:            10010000
    số tiếp theo:           10010000 + 1 = 10010001
    số tiếp theo sau đó:    10010001 + 1 = 10010010
    số tiếp theo sau đó:    10010010 + 1 = 10010011
    số tiếp theo sau đó:    10010011 + 1 = 10010100
```
Như vậy kỹ thuật sinh được tóm tắt:
```java
    i=n;
    while((i>0) && (x[i] == 1)) --i;
    if (i>0) {
        x[i] = 1;
        for (j=i+1; j<=n; ++j) x[j] =0;
    }
```
