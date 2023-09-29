# Thuật toán quy lui

## 1. Thuật toán quy lui

**Thuật toán quy lui dùng để giải bài toán liệt kê các cấu hình. Mỗi cấu hình được xây dựng bằng cách xây dựng từng phần tử, mỗi phần tử được chọn bằng cách thử tất cả các khả năng**

Giả sử cấu hình cần liệt kê có dạng  (x1, x2, x3, ...., xn) khi đó thuật toán quy lui thực hiện qua các bước:

1. xét tất cả các giá trị x1  có thể nhận thử cho x1 lần lượt nhận các giá trị đó. Với mỗi giá trị thử gán cho x1 ta sẽ tiến hành bước 2
2. xét tất cả các giá trị x2  có thể nhận thử cho x1 lần lượt nhận các giá trị đó. Với mỗi giá trị thử gán cho x2 ta sẽ tiến hành các khả năng của x3 ... cứ tiếp tục đến xn

Mô hình thuật toán tổng quát như sau

```java
void try(int i) {
    for (v : {tập các giá trị V mà có thể gắn cho x[i]}) {
        x[i] = v;
        if (i==n) {
            return cấu hình tìm đc;
        } else {
            mark[v] = true; //  ghi nhận việc x[i] nhận giá trị v
            try(i+1);
            mark[v] = false; // bỏ việc ghi nhận x[i] nhận giá trị v
        }
    }
}
```

## 2. Liệt kê các dãy nhị phân độ dài n (bài toán 4 của [Exhaustive search](blog-post-1.html?algorithm=exhaustive_search))

**Ý tưởng**
```C++
C++

int N;
int x[100];

void backtrack(int i) {
    for (int v=0;  v<=1; ++v) {
        x[i] = v;
        if (i==N) {
            print(x);
        } else {
            backtrack(v);
        }
    }
}
```

<img src="blog/algorithm/img/backtracking.png" style="display: block; margin-right: auto; margin-left: auto;">

**Example:**

```C++
C++
#include <iostream>
 
using namespace std;
 
int N;
 
int x[100];
 
void in(int x[]){
    for (int i = 1; i <= N; i++)
        cout << x[i];
    cout << endl;
}
 
void deQuy(int i){
    for (int j = 0; j <= 1; j++){
        x[i] = j;
        if (i == N)
            in(x);
        else
            deQuy(i + 1);
        
    }
}
 
int main(){
    cin >> N;
    deQuy(1);
}
```

```java
java

private Integer N;
int[] x;

@GetMapping("/test")
public void test(@RequestParam int n) {
    this.N = n;
    x = new int[n]; // x =[0,0,1]
    this.tryFunc(0);
}

// i là số phấn tử của 1 đối tượng cần in ra
private void tryFunc( int i) {
    for (int v=0;  v<=1; v++) {
        x[i] = v;
        if (i==N-1) {
            System.out.println(Arrays.toString(x));
        } else {
            tryFunc(i+1);
        }
    }
}

GET http://localhost:8080/test?k=3

```

## 2.1 Liệt kê các tập con k phần tử có trùng nhau (mở rộng)

**Example:**

```C++
C++
#include <iostream>
 
using namespace std;
 
int k;
int n;
int c[100];
int x[100];
 
void in(int x[]){
    for (int i = 1; i <= k; i++)
        cout << x[i];
    cout << endl;
}
 
void deQuy(int i){
    for (int j = 0; j <= n; j++){
    // for (v : values){
    //   x[i] = v;
        x[i] = j;
        if (i == k)
            in(x);
        else
            deQuy(i + 1);
    }
}
 
int main(){
    cin >> N;
    deQuy(1);
}
```

```java
java

private Integer n;
private Integer k;
int[] x;

@GetMapping("/test")
public void test(@RequestParam int n, @RequestParam int k) {
    this.n = n; // số lượng phần tử trong 1 element kết quả
    this.k = k; // số lượng phần tử cho từ {1, 2, 3, ... n}
    x = new int[k];
    this.tryFunc(0);
}

private void tryFunc(int i) {
    for (int v=1;  v<=n; v++) {
        x[i] = v;
        if (i == k-1) {
            System.out.println(Arrays.toString(x));
        } else {
            tryFunc(i + 1);
        }
    }
}

    GET http://localhost:8080/test?k=3&n=5

result:
[1, 1, 1]
[1, 1, 2]
[1, 1, 3]
[1, 1, 4]
...
[5, 5, 4]
[5, 5, 5]
```


## 3. Liệt kê các tập con k phần tử tăng dần (bài toán 2 của [Exhaustive search](blog-post-1.html?algorithm=exhaustive_search))

Để liệt kê k phần tử của tập {1, 2, 3, ..., n} ta có thể đưa về liệt kê  các cấu hình (x1, x2, x3, ... x(k)) với x1 < x2 < x3 < ... < x(k)

ta có nhận xét:
- x(k) <= n
- x(k-1) < x(k)-1 < n-1
- x(i) < n-k+1
=> x(i-1) +1 <= x(i) < n-k+i (1<=i<=k)

như vậy, x1 từ 1 (`x0+1`) đến `n-k+1`, với mỗi giá trị đó xét tất cả các lựa chọn của x2 từ `x1+1` đế `n-k+2`

**Ý tưởng**

```java
int k;
int[] x;

void backtrack(int i) {
    for (int v=x[i-1]+1; v<=n-k+i; v++) {
        x[i]=v;
        if (i ==k) {
            print(x);
        } else {
            backtrack(i+1);
        }
    }
}
```

**Example:**

```C++
C++
#include <iostream>
 
using namespace std;
 
int k;
int n;
int c[100];
int x[100];
 
void in(int x[]){
    for (int i = 1; i <= n; i++)
        cout << x[i];
    cout << endl;
}
 
void deQuy(int i){
    for (int v=x[i-1]+1; v<=n-k+i; v++) {
        x[i] = j;
        if (i == k)
            in(x);
        else
            deQuy(i + 1);
    }
}
 
int main(){
    cin >> k;
    deQuy(1);
}
```

```java
java
private Integer k;
private Integer n;
int[] x;

@GetMapping("/test")
public void test(@RequestParam int k, @RequestParam int n) {
    this.k = k; // số lượng phần tử trong 1 element kết quả
    this.n = n; // số lượng phần tử cho từ {1, 2, 3, ... n}
    x = new int[k];
    this.tryFunc(0);
}

private void tryFunc(int i) {
    for (int v=x[i-1 < 0 ? 1 : i-1]+1;  v<=n-(k-1)+i; v++) {
        x[i] = v;
        if (i == k-1) {
            System.out.println(Arrays.toString(x));
        } else {
            tryFunc(i + 1);
        }
    }
}

    GET http://localhost:8080/test?k=3&n=5

result:
[1, 2, 3]
[1, 2, 4]
[1, 2, 5]
...
[2, 4, 5]
[3, 4, 5]
```

## 4. Liệt kê các tập con k phần tử không lặp (bài toán 3 của [Exhaustive search](blog-post-1.html?algorithm=exhaustive_search))

Để có thể liệt kê các tập con k phần tử không lặp thì hàm tryFunc(i) có quyền xét tất cả các khả năng chọn từ 1 đến n mà các giá trị này chưa bị các phần tử đứng trước chọn
- ta khai báo 1 mảng `c` kiểu boolean để đánh dấu giá trị i đã bị chọn hay chưa.

**Ý tưởng**

```java
private void tryFunc(int i) {
    for (int v=1;  v<=n; v++) {
        if (Objects.isNull(c[v]) || c[v]) {
            x[i] = v;
            if (i == k-1) {
                System.out.println(Arrays.toString(x));
            } else {
                c[v]= false; // đánh dấu v đã được dùng không sử dụng nữa
                tryFunc(i + 1);
                c[v]= true; // bỏ đánh dấu v đã được dùng để sử dụng lại
            }
        }
    }
}
```


**Example:**

```C++
C++
#include <iostream>
 
using namespace std;
 
int k;
int n;
int c[100];
int x[100];
 
void in(int x[]){
    for (int i = 1; i <= k; i++)
        cout << x[i];
    cout << endl;
}
 
void deQuy(int i){
    for (int j = 0; j <= n; j++){
        x[i] = j;
        if (i == k)
            in(x);
        else
            deQuy(i + 1);
    }
}
 
int main(){
    cin >> N;
    deQuy(1);
}
```
```java

private Integer k;
private Integer n;
int[] x;
Boolean[] c;

@GetMapping("/test")
public void test(@RequestParam int k, @RequestParam int n) {
    this.k = k; // số lượng phần tử trong 1 element kết quả
    this.n = n; // số lượng phần tử cho từ {1, 2, 3, ... n}
    x = new int[k]; // init
    c = new Boolean[1]; // init
    this.tryFunc(0);
}

private void tryFunc(int i) {
    for (int v=1;  v<=n; v++) {
        if (Objects.isNull(c[v]) || c[v]) {
            x[i] = v;
            if (i == k-1) {
                System.out.println(Arrays.toString(x));
            } else {
                c[v]= false;
                tryFunc(i + 1);
                c[v]= true;
            }
        }
    }
}

    GET http://localhost:8080/test?k=3&n=5

result :
[1, 2, 3]
[1, 2, 4]
[1, 2, 5]
...
[5, 4, 2]
[5, 4, 3]

```

## 4. Liệt kê các hoán vị (bài toán 1 của [Exhaustive search](blog-post-1.html?algorithm=exhaustive_search))

Bài toán hoán vị chính là bài toán  liệt kê các chỉnh hợp vs n=k. 

**Ý tưởng**

```java
private void tryFunc(int i) {
    for (int v=1;  v<=n; v++) {
        if (Objects.isNull(c[v]) || c[v]) {
            x[i] = v;
            if (i == N-1) {
                System.out.println(Arrays.toString(x));
            } else {
                c[v]= false; // đánh dấu v đã được dùng không sử dụng nữa
                tryFunc(i + 1);
                c[v]= true; // bỏ đánh dấu v đã được dùng để sử dụng lại
            }
        }
    }
}
```


**Example:**


```java

private Integer k;
int[] x;
int[] n = {1,4,5,8,9};
Boolean[] c;

@GetMapping("/test")
public void test(@RequestParam int k) {
    this.k = k; // số lượng phần tử trong 1 element kết quả
    x = new int[k]; // init
    c = new Boolean[6]; // init
    this.tryFunc(0);
}

private void tryFunc(int i) {
    for (int v=0;  v<n.length; v++) {
        if (Objects.isNull(c[v]) || c[v]) {
            x[i] = n[v];
            if (i == k-1) {
                System.out.println(Arrays.toString(x));
            } else {
                c[v]= false;
                tryFunc(i + 1);
                c[v]= true;
            }
        }
    }
}

    GET http://localhost:8080/test?k=3

int[] n = {1,4,5,8,9};

result:

[1, 4, 5]
[1, 4, 8]
[1, 4, 9]
[1, 5, 4]
....
[9, 5, 8]
[9, 8, 1]
[9, 8, 4]
[9, 8, 5]
```


**Ưu điểm**: Việc quay lui là thử tất cả các tổ hợp để tìm được một lời giải. Thế mạnh của phương pháp này là nhiều cài đặt tránh được việc phải thử nhiều trường hợp chưa hoàn chỉnh, nhờ đó giảm thời gian chạy.

**Nhược điểm**: Trong trường hợp xấu nhất độ phức tạp của quay lui vẫn là cấp số mũ. Vì nó mắc phải các nhược điểm sau:

- Rơi vào tình trạng "thrashing": qúa trình tìm kiếm cứ gặp phải bế tắc với cùng một nguyên nhân.
- Thực hiện các công việc dư thừa: Mỗi lần chúng ta quay lui, chúng ta cần phải đánh giá lại lời giải trong khi đôi lúc điều đó không cần thiết.
- Không sớm phát hiện được các khả năng bị bế tắc trong tương lai. Quay lui chuẩn, không có cơ chế nhìn về tương lai để nhận biết đc nhánh tìm kiếm sẽ đi vào bế tắc.
