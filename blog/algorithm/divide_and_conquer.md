# Tìm kiếm nhị phân và chia để trị

## 1. Bài toán tìm kiếm

Tìm kiếm là một đòi hỏi rất thường xuyên trong các ứng dụng. Bài toán có thể phát biểu như sau:
Cho 1 dãy gồm n bản ghi r1, r2, ... r(n). Mỗi bản ghi r(i) (1 <= i <= n) tương ứng với một khó k(i). Hãy tìm bản ghi có giá trị khóa bằng x cho trước.

X: được gọi là khóa tìm kiếm hay đối trị tìm kiếm (argument)

Công việc tìm kiếm sẽ haofn thành nếu như có 1 trong 2 tình huống sau xảy ra
- Tìm đc bản ghi có kháo tương ứng bằng x, lúc đó phép tìm kiếm là thành công
- Không tìm đc bản ghi nào có khóa bằng x, lúc đó phép tìm kiếm là thất bại

### 1.1 Tìm kiếm tuần tự

Tìm kiếm tuần tự là 1 kỹ thuật tìm kiếm đơn giản. Nội dung của nó là tìm từ bản ghi đầu tiên, lần lượt so sánh khóa tìm kiếm với khóa tương ứng của các bản ghi trong danh sách cho tới khi tìm thấy bản ghi mong muốn hoặc đã duyệt hết danh sách

```java
int sequentialSearch(type k[], type x) {
    k[n+1] = x;
    int i = 1;
    while (k[i] != x) ++i;
    if (i ==n +1) 
        return 0;
    else
        return i;
}
```
Độ phức tạp của thuật toán: O(1) là trường hợp tốt nhất, xấu nhất O(n) và trung bình là O(n).

### 1.2 Tìm kiếm nhị phân

Reference: [Binary search](blog-post-1.html?algorithm=binary-search)

### 1.3 Tìm kiếm cây nhị phân

Reference: [Binary search tree](blog-post-1.html?algorithm=binary_search_tree)

## 2. Thuật toán chia để trị

Chia để trị là một mô hình thiết kế thuật toán dựa trên đệ quy với nhiều phân nhánh.
Thuật toán chia để trị là chia 1 cách đệ quy một vấn đề. Ví dụ: sắp xếp (quick sort, merge sort) nhaanh với các số lớn (thuật toán kararsuka). tìm cặp điểm gần nhất  (find the closest pair of points), phân tích cú pháp và các tính toán biến đổi furier rời rạc.

Việc thiết kế thuật toán chia để trị hiệu quả có thể khó khăn. Tương tự như quy nạp toán học, thông thường cần phải tổng quát hóa vấn đề để biến thành giải pháp đệ quy. Tính đúng đắn của thuật toán chia để trị thường đc chứng minh bằng quy nạp toán học và chi phí tính toán của nó thường đc xác định bằng cách giải các quan hệ lặp đi lặp lại

Thuật toán chia để trị có thể chia thành 3 phần:

1. Didive (chia): chia bài toán thành các bài toán nhỏ hơn. Bước này thường thực hiện 1 cách đệ quy để bài toán nhỏ cho đến khi không chia nhỏ đc nữa.
2. Conquer(trị): Giải quyết bài toán con để được chia nhỏ đến mức đơn giản có thể giải quyết trực tiếp.
3. Combine (kết hợp):  Khi các bài toán nhỏ hơn đc giải quyết, kết hợp chúng 1 cách đệ quy cho đến khi chúng là lời giải của bài toán ban đầu.

<img src="blog/algorithm/img/divide_and_conquer.png" style="display: block; margin-right: auto; margin-left: auto;">

### 2.1 Tìm tập con liên tục có tổng lớn nhất

Cho một mảng N số nguyên. Tìm mảng con liền nhau có tổng lớn nhất. Bài toán này dùng brute-force sẽ có độ phức tạp `O(n^3)` bằng cách xét tổng của tất cả các mảng con liên nhau.

```java
int maxSubarraySum(int arr[], int n) {
    int maxSum = arr[0];
    for (int i=0; i<n; i++) {
        for (int j= i; j<n; j++) {
            int sum =0;
            for (int k=i; k<=j; k++) {
                sum += arr[k];
            }
            if (sum > maxSum) {
                maxSum = sum;
            }
        }
    }
}
```
Áp dụng chia để trị ta có thể giải quyết như sau:

1. chia mảng đầu vào thành 2 mảng
2. Trả ra tổng lớn nhất của:
- mảng bên trái (bằng cách đệ quy).
- mảng bên phải (bằng cách đệ quy).
- mảng gồm của phần giữa.

``` java
int maxCrossingSum(int arr[], int l, int m, int h) {
    int sum  =0;
    int leftSum = 0;
    for (int i=m; i>=l; i--) {
        sum += arr[i];
        if (sum > leftSum) {
            leftSum = sum;
        }
    }
    // reset sum =0
    sum  =0;
    int rightSum = 0;
    for (int i=m+1; i<=h>; i++) {
        sum += arr[i];
        if (sum > rightSum) {
            rightSum = sum;
        }
    }
    return max(leftSum + rightSum, leftSum, rightSum);
}
int maxSubarraySum(int arr[], int l, int h) {
    if (l==h) {
        return arr[l];
    }
    int m = (l+h)/2;

    return max(maxSubarraySum(arr, l, m),       // call maxCrossingSum(l, (m+l)/2, m)
                maxSubarraySum(arr, m+1, h),    // call maxCrossingSum(m+1, (m+1+h)/2, h)
                maxCrossingSum(arr, l, m, h)    // call maxCrossingSum(arr, l, m, h)
    );
}

```

### 2.2 Tìm cặp điểm gần nhất

Cho N điểm trên một mặt phẳng và nhiệm vụ của bạn là tìm 1 cặp điểm có khoảng cách giữa chúng nhỏ nhất. Tất cả các điểm sẽ là duy nhất và chỉ có 1 cặp có khoảng cách nhỏ nhất.
Áp dụng brute-force: độ phức tạp là `O(N^2)`
```java
struct Point {
    int x, y;
}
float bruteForce(Point[], int n) {
    float min = 0;
    for (int i=0; i<n; i++) {
        for (int j=i+1; j<n; j++) {
            if (dist(P[i], P[j]) < min) {
                min = dist(P[i], P[j]) ;
            }
        }
    }
    return min;
}
double dist(Point p1, Point p2) {
    return sqrt((p1.x -p2.x)*(p1.x -p2.x) + (p1.y -p2.y)*(p1.y -p2.y));
}
```

Sử dụng hàm sinh để tìm min distance

```java
class Point {
    int x;
    int y;
}
float min;

float backTracking(Point[] point, int n) {
    retrun findMin(point[0], n);
}

float findMin(Point p, int n) {
    for (int i=1, i<n; i++) {
        if (dist(p, p[i]) < min) {
            min = dist(p, p[i]);
        }
        findMin(p[i], n);
    }
    return min;
}

double dist(Point p1, Point p2) {
    return sqrt((p1.x -p2.x)*(p1.x -p2.x) + (p1.y -p2.y)*(p1.y -p2.y));
}
```
