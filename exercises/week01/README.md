# Tuần 1: Tổng Quan C++ & Big-O — Bài tập

## 🎯 Mục tiêu tuần này
Hiểu Big-O, phân tích độ phức tạp, ôn tập C++ cơ bản.

---

### Bài 1: Phân tích Big-O ⭐
Xác định Big-O của 10 đoạn code C++ cho trước. Giải thích tại sao.
Để rèn luyện tư duy nhẩm nhanh độ phức tạp thuật toán (Big-O), chúng ta sẽ cùng đi qua **10 mẫu code C++ điển hình** từ dễ đến khó. Mỗi đoạn code đều đại diện cho một cấu trúc lặp hoặc quy luật toán học thường gặp trong các bài thi và phỏng vấn giải thuật.

---

### Đoạn số 1: Vòng lặp nhảy cóc (Step-increment)

```cpp
void ViDu1(int n) {
    int dem = 0;
    for (int i = 0; i < n; i += 3) {
        dem++;
    }
}

```

* **Kết quả:** $O(n)$
* **Giải thích:** Vòng lặp chạy từ $0$ đến $n$, mỗi lần `i` tăng thêm $3$ đơn vị. Số lần lặp thực tế là $\frac{n}{3}$. Theo quy tắc Big-O, ta bỏ qua hằng số nhân hoặc chia ($\frac{1}{3}$), do đó tốc độ tăng trưởng vẫn là tuyến tính $O(n)$.

---

### Đoạn số 2: Hai vòng lặp độc lập không lồng nhau

```cpp
void ViDu2(int n) {
    int dem = 0;
    for (int i = 0; i < n; i++) {
        dem++;
    }
    for (int j = 0; j < n; j++) {
        dem--;
    }
}

```

* **Kết quả:** $O(n)$
* **Giải thích:** Vòng lặp thứ nhất tốn $n$ bước, vòng lặp thứ hai tốn $n$ bước. Tổng số bước thực hiện là $n + n = 2n$. Khi chuyển sang Big-O, ta lược bỏ hằng số số $2$, kết quả giữ lại là $O(n)$.

---

### Đoạn số 3: Vòng lặp lồng phụ thuộc vào biến ngoài

```cpp
void ViDu3(int n) {
    int dem = 0;
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < i; j++) {
            dem++;
        }
    }
}

```

* **Kết quả:** $O(n^2)$
* **Giải thích:** Vòng lặp bên trong chạy phụ thuộc vào `i`.
* Khi `i = 0`, vòng trong chạy `0` lần.
* Khi `i = 1`, vòng trong chạy `1` lần...
* Khi `i = n-1`, vòng trong chạy `n-1` lần.
Tổng số lần chạy là một cấp số cộng: $0 + 1 + 2 + \dots + (n-1) = \frac{n(n-1)}{2} = 0.5n^2 - 0.5n$. Giữ lại số hạng bậc cao nhất và bỏ hằng số, ta được $O(n^2)$.



---

### Đoạn số 4: Hai biến đầu vào khác nhau

```cpp
void ViDu4(int n, int m) {
    int dem = 0;
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            dem++;
        }
    }
}

```

* **Kết quả:** $O(n \times m)$
* **Giải thích:** Vòng ngoài lặp đúng $n$ lần, vòng trong lặp đúng $m$ lần. Vì $n$ và $m$ là hai biến độc lập hoàn toàn và ta chưa biết biến nào lớn hơn biến nào, ta không thể gộp chung thành $n^2$. Độ phức tạp phải biểu diễn qua cả hai biến: $O(n \times m)$.

---

### Đoạn số 5: Vòng lặp tăng trưởng theo cấp số nhân

```cpp
void ViDu5(int n) {
    int dem = 0;
    for (int i = 1; i < n; i *= 2) {
        dem++;
    }
}

```

* **Kết quả:** $O(\log n)$
* **Giải thích:** Giá trị của `i` qua từng bước sẽ tăng theo cấp số nhân: $1, 2, 4, 8, 16, \dots$. Vòng lặp sẽ dừng lại khi $2^k \ge n$ (với $k$ là số lần lặp). Lấy logarit cơ số 2 hai vế, ta có $k \approx \log_2(n)$. Đây chính là định nghĩa của $O(\log n)$.

---

### Đoạn số 6: Vòng lặp lồng giữa tuyến tính và logarit

```cpp
void ViDu6(int n) {
    int dem = 0;
    for (int i = 0; i < n; i++) {
        for (int j = 1; j < n; j *= 2) {
            dem++;
        }
    }
}

```

* **Kết quả:** $O(n \log n)$
* **Giải thích:** Vòng lặp ngoài chạy tuyến tính $n$ lần. Với **mỗi** lần vòng ngoài chạy, vòng lặp bên trong lại thực hiện chia đôi/nhân đôi dữ liệu mất $\log n$ lần (như phân tích ở Ví dụ 5). Nhân hai đại lượng phụ thuộc này lại ta thu được $O(n \log n)$. Cấu trúc này rất kinh điển trong các thuật toán sắp xếp nhanh như *Merge Sort* hoặc *Quick Sort*.

---

### Đoạn số 7: Vòng lặp giảm dần theo căn bậc hai

```cpp
void ViDu7(int n) {
    int dem = 0;
    for (int i = 0; i * i < n; i++) {
        dem++;
    }
}

```

* **Kết quả:** $O(\sqrt{n})$
* **Giải thích:** Điều kiện dừng của vòng lặp là $i^2 < n$, điều này tương đương với $i < \sqrt{n}$. Do biến `i` tăng tuần tự từng đơn vị một (`i++`), vòng lặp sẽ chạy chính xác $\sqrt{n}$ lần. Cấu trúc này cực kỳ phổ biến trong các bài toán tối ưu kiểm tra số nguyên tố hoặc tìm ước số.

---

### Đoạn số 8: Vòng lặp lồng nhau với giới hạn hằng số

```cpp
void ViDu8(int n) {
    int dem = 0;
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < 100; j++) {
            dem++;
        }
    }
}

```

* **Kết quả:** $O(n)$
* **Giải thích:** Rất nhiều bạn nhìn thấy 2 vòng lặp lồng nhau là kết luận ngay $O(n^2)$. Tuy nhiên, vòng lặp phía trong có số lần lặp cố định luôn là `100` lần (một hằng số), không hề phụ thuộc vào độ lớn của `n`. Tổng số lần chạy là $100n$. Khi chuyển sang Big-O, hằng số bị loại bỏ, chỉ còn lại $O(n)$.

---

### Đoạn số 9: Đệ quy tuyến tính đơn giản

```cpp
int ViDu9(int n) {
    if (n <= 1) return 1;
    return ViDu9(n - 1) + 1;
}

```

* **Kết quả:** $O(n)$
* **Giải thích:** Hàm đệ quy này thực hiện giảm giá trị `n` đi 1 đơn vị sau mỗi lần gọi cấu trúc: $n \rightarrow n-1 \rightarrow n-2 \rightarrow \dots \rightarrow 1$. Sẽ có tổng cộng $n$ tầng gọi đệ quy sâu xuống trước khi chạm vào điều kiện dừng (`if (n <= 1)`). Do mỗi tầng chỉ thực hiện phép tính cộng toán học mất $O(1)$, tổng thời gian chạy là $O(n)$.

---

### Đoạn số 10: Đệ quy nhánh nhị phân (Cây đệ quy)

```cpp
int ViDu10(int n) {
    if (n <= 1) return 1;
    return ViDu10(n - 1) + ViDu10(n - 1);
}

```

* **Kết quả:** $O(2^n)$
* **Giải thích:** Đây là hàm đệ quy có tốc độ "bùng nổ" khủng khiếp. Mỗi một lần gọi hàm, nó lại sinh ra **2** lời gọi hàm con khác.
* Tầng 0: 1 lệnh gọi (`ViDu10(n)`)
* Tầng 1: 2 lệnh gọi (`ViDu10(n-1)`)
* Tầng 2: 4 lệnh gọi...
* Tầng $n$: $2^n$ lệnh gọi.
Tổng số thao tác xử lý tương đương với tổng số nút trên một cây nhị phân đầy đủ có chiều cao $n$, tức là $2^n - 1$ thao tác. Độ phức tạp là $O(2^n)$ (Thời gian mũ).

### Bài 2: Đo thời gian thực tế ⭐⭐
Dùng `chrono` đo thời gian chạy của O(n), O(n²), O(log n) với n = 1.000 → 100.000. In bảng kết quả.

### Bài 3: Tối ưu hàm ⭐⭐
Cho 3 hàm O(n²) — tối ưu xuống O(n) hoặc O(n log n). Chứng minh bằng cách đo thời gian.

### Bài 4: 🔥 Dự Án Mini — Big-O Benchmark Tool ⭐⭐⭐
> **Cảm hứng:** [algorithm-visualizer.org](https://algorithm-visualizer.org)

Viết chương trình **BenchmarkTool** hiển thị bảng so sánh tốc độ các thuật toán:
```
╔══════════════╦══════════╦══════════╦══════════╗
║   Thuật toán ║  n=1000  ║  n=10000 ║ n=100000 ║
╠══════════════╬══════════╬══════════╬══════════╣
║    O(1)      ║  0.001ms ║  0.001ms ║  0.001ms ║
║    O(log n)  ║  0.003ms ║  0.004ms ║  0.005ms ║
║    O(n)      ║  0.12ms  ║  1.2ms   ║  12ms    ║
║    O(n²)     ║  8ms     ║  800ms   ║  80000ms ║
╚══════════════╩══════════╩══════════╩══════════╝
```

**Yêu cầu:** dùng `std::chrono`, hiển thị bảng căn chỉnh đẹp, xuất ra file `benchmark.txt`.

---
📁 Tham khảo: `Chuong1_TongQuan/Chuong1_TongQuan.cpp`
