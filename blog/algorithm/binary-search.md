# Thuật toán tìm kiếm nhị phân

# `O(log(N))`

## Tổng quát hóa bài toán

Từ ví dụ trên, ta có thể dễ dàng hiểu được ý tưởng của thuật toán tìm kiếm nhị phân. Đúng như tên gọi, thuật toán sẽ liên tục chia không gian tìm kiếm thành hai nửa và loại một nửa đi. Thuật toán có thể trình bày như sau:

1. Ta duy trì một không gian tìm kiếm S là một dãy con các giá trị có thể là kết quả (ở đây là chỉ số các phần tử trong A). Ban đầu, không gian tìm kiếm là toàn bộ các chỉ số của mảng S=1,…,n với n là chỉ số phần tử cuối cùng của A.
2. Ở mỗi bước, thuật toán so sánh giá trị cần tìm với phần tử có chỉ số là trung vị trong không gian tìm kiếm. Dựa trên sự so sánh đó, cộng thêm việc ta biết dãy A có thứ tự, ta có thể loại một nửa số phần tử của S. Lặp đi lặp lại quá trình này, cuối cùng ta sẽ được một không gian tìm kiếm bao gồm một phần tử duy nhất.
3. Khi đó, nếu phần tử duy nhất đó bằng với giá trị cần tìm x thì đó là nghiệm của bài toán, nếu không thì bài toán vô nghiệm.

Ở đây có hai lưu ý khi cài đặt thuật toán:

- Do không gian tìm kiếm luôn là một đoạn liên tục các giá trị nguyên, ta không cần lưu tất cả phần tử của không gian khi tìm kiếm mà chỉ cần duy trì hai biến low và high tượng trưng cho phần tử đầu và cuối của đoạn.
- Ta có thể tối ưu thuật toán hơn bằng việc dừng sớm nếu trong quá trình so sánh gặp một phần tử trung vị thỏa yêu cầu đề bài chứ không cần đợi đến khi không gian tìm kiếm chỉ còn một phần tử.

```C++
int binary_search(int A[], int sizeA, int target) {
    int lo = 1, hi = sizeA;
    while (lo <= hi) {
        int mid = lo + (hi - lo)/2;
        if (A[mid] == target)
            return mid;       	
        else if (A[mid] < target)
            lo = mid+1;
        else
            hi = mid-1;
    }
    // không tìm thấy giá trị target trong mảng A
    return -1;
}    	
```

<img src="blog/algorithm/img/binary-search1.png" style="display: block; margin-right: auto; margin-left: auto;">

## Độ phức tạp thuật toán

Ở mỗi bước, kích thước không gian tìm kiếm bị giảm đi một nửa. Ta dễ thấy rằng độ phức tạp của thuật toán là **O(log(N))** với N là số phần tử ban đầu của không gian tìm kiếm.

### Tìm kiếm nhị phân trong thư viện chuẩn STL
C++ Standard Template Library đã cài đặt sẵn tìm kiếm nhị phân bằng các hàm `lower_bound`, `upper_bound`, `binary_search`, `equal_range`, bạn đọc có thể tham khảo tùy thuộc vào mục đích sử dụng.

## Cơ sở lý thuyết: Định lý chính của tìm kiếm nhị phân

Khi gặp một bài toán mà ta đoán được có thể dùng tìm kiếm nhị phân để giải, thì ta phải chứng minh tính đúng đắn suy luận của chúng ta. Do đó, xây dựng một cơ sở lý thuyết vững chắc là vô cùng cần thiết. Sau đây, tôi sẽ trình bày một lớp tổng quát hóa nữa cho các bài toán có thể áp dụng tìm kiếm nhị phân, song song đó là ví dụ thực tế với bài toán mở đầu.

Cho không gian tìm kiếm S bao gồm các ứng cử viên cho kết quả của bài toán. Ta định nghĩa môt hàm kiểm tra P:S→true,false là hàm nhận một ứng cử viên x∈S và trả về giá trị true/false cho biết x có hợp lệ hay không (tùy vào bài toán mà định nghĩa hợp lệ sẽ khác nhau). Hiểu đơn giản, hàm P là hàm "kiểm tra" một tính chất nào đó, xem một ứng cử viên cho kết quả của bài toán có thỏa tính chất đó không.


    ∀x,y∈S,y>x∧P(x)=true⇒P(y)=true      (*)

    ∀x,y∈S,y<x∧P(x)=false⇒P(y)=false    (**)


- Tính chất (*) có thể giải thích như sau: nếu x hợp lệ thì mọi phần tử y>x đều hợp lệ. Tính chất này giúp chúng ta loại đi nửa sau của không gian tìm kiếm do đã biết chắc x là phần tử nhỏ nhất trong nửa sau hợp lệ, ta ghi nhận x là kết quả tạm thời và tiếp tục tìm xem có phần tử nào ở nửa đầu (nhỏ hơn x) hợp lệ hay không.
- Tương tự, tính chất (**) có thể giải thích như sau: nếu x không hợp lệ thì mọi phần tử y < x đều không hợp lệ. Tính chất này giúp chúng ta loại đi nửa trước của không gian tìm kiếm do đã biết chắc chúng không hợp lệ, ta chỉ quan tâm những phần tử ở nửa sau (lớn hơn x) mà ta chưa biết thông tin chúng có hợp lệ hay không.

## Cài đặt thuật toán tổng quát

1. Dãy P(S) của bạn có dạng false−true (false liên tiếp rồi true liên tiếp) hay true−false (ngược lại)? Ở cài đặt phía dưới sẽ mặc định dãy P(S) có dạng false−true, vì vậy nếu dãy P(S) của bạn có dạng true−false, hãy đảo hàm P(x) của bạn ngược lại.
2. Bài của bạn có luôn có nghiệm không? Nếu không hãy kiểm tra trước khi tìm kiếm nhị phân để tiết kiệm chi phí tính toán.
3. Mục tiêu của bạn là tìm phần tử false lớn nhất hay tìm phần tử true nhỏ nhất? Ở đây sẽ trình bày cả hai cách.
4. Nếu bài toán có nghiệm, hãy đảm bảo giá trị chặn dưới và chặn trên của khoảng tìm kiếm (biến lo và hi) là bắt đầu và kết thúc của một khoảng đóng mà chắc chắn chứa kết quả cần tìm (phần tử x đầu tiên mà P(x)=true). Hãy đảm bảo điều kiện này trong lúc thu hẹp không gian tìm kiếm để tránh xảy ra lỗi.
5. Phạm vi tìm kiếm đã đủ rộng chưa? Sẽ có nhiều lúc chấm xong bạn nhận ra là mình thiếu vài trường hợp biên. Vì thời gian chạy chỉ tăng theo hàm logarit O(log(N)), bạn hoàn toàn có thể nâng rộng khoảng tìm kiếm ra mà ít khi lo bị quá thời gian. Tuy nhiên, phải để ý các lỗi như tràn mảng, tràn số,…
6. Luôn kiểm tra trường hợp P(S)=[false,true]. Để hiểu lý do tại sao hãy đọc Trường hợp 2 của cài đặt.

### **TH1: tìm x nhỏ nhất mà P(x)=true.**

```java
bool P(int x) {
    // Logic của hàm P ở đây
    return true;  // thay giá trị này bằng giá trị đúng logic.
}

int binary_search(int lo, int hi) {
    while (lo < hi) {
        int mid = lo + (hi-lo)/2;
        if (P(mid) == true)
            hi = mid;
        else
            lo = mid+1;
    }
  	
    if (P(lo) == false)
        return -1; // P(x) = false với mọi x thuộc S, bài toán vô nghiệm.
  	
   return lo; // lo là giá trị x nhỏ nhất mà P(x) = true
}
```

### **TH2: tìm x lớn nhất mà P(x)=false.**

```java
bool P(int x) {
    // Logic của hàm P ở đây
    return true;  // thay giá trị này bằng giá trị đúng logic.
}

int binary_search(int lo, int hi) {
    while (lo < hi) {
        int mid = lo + (hi-lo+1)/2;
        if (P(mid) == true)
            hi = mid - 1;
        else
            lo = mid;
    }
  	
    if (P(lo) == true)
        return -1; // P(x) = true với mọi x thuộc S, bài toán vô nghiệm.
  	
   return lo; // lo là giá trị x lớn nhất mà P(x) = false
}
```


## Ví dụ

https://leetcode.com/problems/capacity-to-ship-packages-within-d-days/


```java
class Solution {
    private static boolean CanShipCargo(int [] weights, int max, int days){
        int count_weight=0;
        int count_days=1;
        // max = 32
        for(int i=0;i< weights.length;i++){
            if(count_weight+ weights[i]> max)
            {
                count_days++;
                count_weight= weights[i];

            }else count_weight+= weights[i];

        }
    System.out.println(count_weight+" "+max+" "+ count_days);
        return count_days<=days;
    }
    public int shipWithinDays(int[] weights, int days) {
        
        // find max of array (it means that is min of result) and find sum (min of result)
        int max=0;
        int sum=0;
        for(int i=0;i< weights.length;i++){
            max= Math.max(weights[i], max); // 10
            sum+= weights[i]; // 55
        }

        int lo= max; // 10
        int hi= sum; // 55
        if(days== weights.length) return max;
        int ans = -1;
        while(lo<= hi){
            int mid= lo+(hi-lo)/2;
           if(CanShipCargo(weights, mid, days)){
             ans= mid;
             hi= mid-1;
           }else lo=mid+1;
          
        }
        return ans;
    }
}
```


