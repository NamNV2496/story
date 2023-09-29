# Thuật toán tham lam (greedy)


## 1. Thuật toán tham lam

Thuật toán tham làm làm bất kỳ thuật toán nào tuân theo kinh nghiệm giải quyết vấn đề để đưa ra tối ưu cục bộ ở mỗi giai đoạn.

Trong hầu hết các vấn đề, việc áp dụng tham lam không tạo ra một giải pháp tối ưu nhưng đem lại các giải pháp tối ưu cục bộ gần đúng với một giải pháp tối ưu trong thời gian hợp lý

Nói chung thuật toán tham lam có 5 phần:

1. Một tập hợp các cấu hình để dựa vào đó đưa ra một giải pháp - A candidate set
2. Một hàm lựa chọn để chọn cấu hình tốt nhất để thêm vào giải pháp - A selection function
3. Một hàm khả thi để xác định xem 1 cấu hình có thể được đưa vào giải pháp không? - A feasibility function
4. Một hàm mục tiêu để chỉ định 1 giá trị cho 1 giải pháp hoặc 1 phần giải pháp - An objective function
5. 1 hàm giả pháp để cho biết khi nào ta xác định ra 1 giải pháp hoàn chỉnh - A solution function


## 2. Một số bài toán áp dụng tham lam

### 2.1 Thuật toán minimum spanning tree
Tìm cành ngắn nhất

Áp dụng cho các bài toán tìm đường đi dây, ...

### 2.2 Thuật toán kruskal

Thuật toán có 3 bước:

1. Sắp xếp tất cả các cạnh theo thứ tự tăng dần theo trọng số
2. Chọn cạnh nhỏ nhất. Kiểm tra xem nó cso tạo thành 1 chu trình cây khung đã xây dựng hay không. Nếu không tạo chu trình thì thêm cạnh vào cây khung đang xây dựng. Ngược lại, loại bỏ cây khung
3. Lặp lại bước 2 cho đến khi có (v-1) cạnh cho cây

```java
bool kruskalMST() {
    int parent[];
    int count[];

    for (int i=0; i<V; i++) {
        parent[i] = -1; // tât cả cây có 1 nút
        count[i] = 1
    }

    qsort(E); // sắp xếp lại các cạnh của đồ thị theo thứ tự tăng dần

    while(mst.size() < V-1 && E.size() > 0) {
        e = E.front();
        E.pop();

        if (!makeUnion(e, parent, count)) {
            coutinue;
        }
        mst.push(e);
    }
    return mst.size();
}

bool makeUnion(Edge e, int parent[], int count[]) {
    int parentStart = e.start;
    while (parent[parentStart] != -1) parentStart = parent[parentStart]; // tìm gốc của cây start
    int parentEnd = e.end;
    while (parent[parentStart] != -1) parentEnd = parent[parentEnd]; // tìm gốc của cây end


    if(parentEnd == parentStart) return false;

    if(count[parentStart] > count[parentEnd]) { // nếu cây của nút start nhiều hơn => gộp cây
        parent[parentEnd] = parentStart;
        count[parentStart] += count[parentEnd]; // tăng số lượng nút của cây start
    } else {
        parent[parentStart] = parentEnd;
        count[parentEnd] += count[parentStart]; // tăng số lượng nút của cây end
    }

    return true;
}
 
```

### 2.3 Thuật toán prime

### 2.4 Thuật toán dijkstra



























