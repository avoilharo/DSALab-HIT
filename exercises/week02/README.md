# Tuần 2: Mảng & Con Trỏ — Bài tập

## 🎯 Mục tiêu tuần này
Thành thạo mảng 1D/2D, con trỏ, cấp phát động trong C++.

---

### Bài 1: Mảng cơ bản ⭐
Nhập mảng n phần tử. Tính min, max, trung bình, tổng. Không dùng STL.
#include <iostream>

using namespace std;

int main() {
    // 1. Nhập và kiểm tra số lượng phần tử n
    int n;
    cout << "Nhap so luong phan tu n (1 <= n <= 1000): ";
    while (!(cin >> n) || n <= 0 || n > 1000) {
        cout << "Gia tri khong hop le! Vui long nhap lai n trong khoang [1, 1000]: ";
        cin.clear(); // Xóa trạng thái lỗi của cin
        cin.ignore(1000, '\n'); // Loại bỏ các ký tự thừa trong bộ đệm
    }

    // Khởi tạo mảng tĩnh có kích thước tối đa
    int arr[1000];

    // 2. Nhập các phần tử vào mảng
    cout << "Nhap " << n << " so nguyen:\n";
    for (int i = 0; i < n; i++) {
        cout << "  arr[" << i << "] = ";
        while (!(cin >> arr[i])) {
            cout << "  Loi! Vui long nhap dung so nguyen: arr[" << i << "] = ";
            cin.clear();
            cin.ignore(1000, '\n');
        }
    }

    // 3. Thuật toán tuyến tính O(n) tìm Min, Max, Tổng
    // Gán giá trị ban đầu là phần tử đầu tiên của mảng
    int min_val = arr[0];
    int max_val = arr[0];
    long long tong = arr[0]; // Dùng long long đề phòng tràn số khi n lớn

    for (int i = 1; i < n; i++) {
        if (arr[i] < min_val) {
            min_val = arr[i]; // Cập nhật Min
        }
        if (arr[i] > max_val) {
            max_val = arr[i]; // Cập nhật Max
        }
        tong += arr[i]; // Cộng dồn dải tổng
    }

    // Tính trung bình cộng (Ép kiểu sang double để lấy phần thập phân chính xác)
    double trung_binh = static_cast<double>(tong) / n;

    // 4. In kết quả ra màn hình
    cout << "\n===============================\n";
    cout << "       KET QUA THONG KE\n";
    cout << "===============================\n";
    cout << "  - Gia tri nho nhat (Min) : " << min_val << "\n";
    cout << "  - Gia tri lon nhat (Max) : " << max_val << "\n";
    cout << "  - Tong cua mang          : " << tong << "\n";
    cout << "  - Trung binh cong        : " << trung_binh << "\n";
    cout << "===============================\n";

    return 0;
}

### Bài 2: Mảng 2D ⭐⭐
Nhân 2 ma trận n×n. Tính định thức ma trận 3×3. Hiển thị đẹp.
#include <iostream>
#include <iomanip>

using namespace std;

// Hằng số giới hạn kích thước tối đa để khai báo mảng tĩnh
const int MAX_SIZE = 50;

// Hàm hiển thị ma trận đẹp, các cột thẳng hàng
void InMaTran(int matrix[][MAX_SIZE], int n) {
    for (int i = 0; i < n; i++) {
        cout << "  [ ";
        for (int j = 0; j < n; j++) {
            // Căn lề phải độ rộng 6 ký tự để các số âm/dương không làm lệch cột
            cout << setw(6) << matrix[i][j] << " ";
        }
        cout << " ]\n";
    }
}

// Hàm nhân 2 ma trận n x n: C = A * B
void NhanMaTran(int A[][MAX_SIZE], int B[][MAX_SIZE], int C[][MAX_SIZE], int n) {
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            C[i][j] = 0; // Khởi tạo phần tử tích
            for (int k = 0; k < n; k++) {
                C[i][j] += A[i][k] * B[k][j];
            }
        }
    }
}

// Hàm tính định thức ma trận 3x3 bằng quy tắc Sarrus (hoặc khai triển Laplace)
int TinhDinhThuc3x3(int M[][MAX_SIZE]) {
    // Công thức Sarrus đường chéo:
    // det = a*e*i + b*f*g + c*d*h - c*e*g - b*d*i - a*f*h
    int duong_cheo_chinh = M[0][0]*M[1][1]*M[2][2] + M[0][1]*M[1][2]*M[2][0] + M[0][2]*M[1][0]*M[2][1];
    int duong_cheo_phu   = M[0][2]*M[1][1]*M[2][0] + M[0][1]*M[1][0]*M[2][2] + M[0][0]*M[1][2]*M[2][1];
    
    return duong_cheo_chinh - duong_cheo_phu;
}

int main() {
    // ============================================================
    // PHẦN 1: NHÂN HAI MA TRẬN N x N
    // ============================================================
    cout << "====================================================\n";
    cout << " 1. PHÉP NHÂN HAI MA TRẬN N x N\n";
    cout << "====================================================\n";
    
    int n;
    cout << "Nhap kich thuoc n cua ma tran vuong (n <= 50): ";
    while (!(cin >> n) || n <= 0 || n > MAX_SIZE) {
        cout << "Kich thuoc khong hop le! Vui long nhap lai: ";
        cin.clear();
        cin.ignore(1000, '\n');
    }

    int A[MAX_SIZE][MAX_SIZE], B[MAX_SIZE][MAX_SIZE], C[MAX_SIZE][MAX_SIZE];

    cout << "\n-> Nhap cac phan tu cho ma tran A (" << n << "x" << n << "):\n";
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            cout << "   A[" << i << "][" << j << "] = ";
            cin >> A[i][j];
        }
    }

    cout << "\n-> Nhap cac phan tu cho ma tran B (" << n << "x" << n << "):\n";
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            cout << "   B[" << i << "][" << j << "] = ";
            cin >> B[i][j];
        }
    }

    // Thực hiện nhân ma trận
    NhanMaTran(A, B, C, n);

    // Hiển thị kết quả trực quan
    cout << "\n[Ma tran A]:\n"; InMaTran(A, n);
    cout << "\n[Ma tran B]:\n"; InMaTran(B, n);
    cout << "\n[Ma tran Tich C = A * B]:\n"; InMaTran(C, n);


    // ============================================================
    // PHẦN 2: TÍNH ĐỊNH THỨC MA TRẬN 3x3
    // ============================================================
    cout << "\n====================================================\n";
    cout << " 2. TÍNH ĐỊNH THỨC MA TRẬN 3x3\n";
    cout << "====================================================\n";

    int M[MAX_SIZE][MAX_SIZE];
    cout << "-> Nhap các phan tu cho ma tran 3x3:\n";
    for (int i = 0; i < 3; i++) {
        for (int j = 0; j < 3; j++) {
            cout << "   M[" << i << "][" << j << "] = ";
            cin >> M[i][j];
        }
    }

    cout << "\n[Ma tran 3x3 Da Nhap]:\n";
    InMaTran(M, 3);

    int det = TinhDinhThuc3x3(M);
    cout << "\n => Dinh thuc cua ma tran (det M) = " << det << "\n";
    cout << "====================================================\n";

    return 0;
}

### Bài 3: Con trỏ & cấp phát động ⭐⭐
Cài đặt mảng động tự resize (như `std::vector` đơn giản). Hỗ trợ push_back, pop_back, at(i).
#include <iostream>
#include <stdexcept> // Thư viện để dùng std::out_of_range
#include <string>

using namespace std;

template <typename T>
class VectorDonGian {
private:
    T* data;           // Con trỏ quản lý mảng động trên Heap
    int current_size;  // Số lượng phần tử thực tế đang có trong mảng
    int max_capacity;  // Sức chứa tối đa hiện tại của vùng nhớ đã cấp phát

    // Hàm bổ trợ để thay đổi kích thước vùng nhớ (Resize)
    void thayDoiDungLuong(int dung_luong_moi) {
        // 1. Cấp phát vùng nhớ mới
        T* temp = new T[dung_luong_moi];

        // 2. Sao chép dữ liệu cũ sang vùng nhớ mới
        for (int i = 0; i < current_size; i++) {
            temp[i] = data[i];
        }

        // 3. Giải phóng vùng nhớ cũ để tránh rò rỉ bộ nhớ (Memory Leak)
        delete[] data;

        // 4. Trỏ con trỏ sang vùng nhớ mới và cập nhật dung lượng tối đa
        data = temp;
        max_capacity = dung_luong_moi;
    }

public:
    // Hàm khởi tạo (Constructor)
    VectorDonGian() {
        current_size = 0;
        max_capacity = 1; // Khởi đầu với sức chứa bằng 1
        data = new T[max_capacity];
    }

    // Hàm hủy (Destructor) - Tự động gọi để dọn dẹp bộ nhớ khi biến bị hủy
    ~VectorDonGian() {
        delete[] data;
    }

    // 1. Thêm phần tử vào cuối mảng
    void push_back(T value) {
        // Nếu số phần tử đạt ngưỡng tối đa, tiến hành gấp đôi dung lượng
        if (current_size == max_capacity) {
            thayDoiDungLuong(2 * max_capacity);
        }
        
        // Chèn phần tử mới vào và tăng kích thước thực tế
        data[current_size] = value;
        current_size++;
    }

    // 2. Xóa phần tử cuối cùng của mảng
    void pop_back() {
        if (current_size > 0) {
            current_size--; // Giảm size là đủ, không cần xóa vật lý vì phần tử sau sẽ ghi đè lên
        }
    }

    // 3. Truy cập phần tử kèm theo kiểm tra an toàn biên
    T& at(int index) {
        if (index < 0 || index >= current_size) {
            throw out_of_range("Loi: Vi tri index nam ngoai pham vi cua mang!");
        }
        return data[index];
    }

    // Các hàm tiện ích lấy thông tin
    int size() const { return current_size; }
    int capacity() const { return max_capacity; }
};

// --- CHƯƠNG TRÌNH KIỂM THỬ (MAIN) ---
int main() {
    cout << "=== KIEM THU MANG DONG TU RESIZE ===\n\n";
    
    VectorDonGian<string> v; // Tạo mảng động chứa chuỗi ký tự

    // Thử nghiệm push_back và quan sát cơ chế tăng dung lượng (capacity)
    cout << "-> Thêm cac phan tu:\n";
    v.push_back("Data_A");
    cout << "   Size: " << v.size() << ", Capacity: " << v.capacity() << " -> " << v.at(0) << "\n";
    
    v.push_back("Data_B");
    cout << "   Size: " << v.size() << ", Capacity: " << v.capacity() << " -> " << v.at(1) << "\n";
    
    v.push_back("Data_C");
    cout << "   Size: " << v.size() << ", Capacity: " << v.capacity() << " -> " << v.at(2) << "\n";

    // Thử nghiệm pop_back
    cout << "\n-> Xoa phan tu cuoi (pop_back):\n";
    v.pop_back();
    cout << "   Size sau khi pop_back: " << v.size() << ", Capacity: " << v.capacity() << "\n";

    // Thử nghiệm bẫy lỗi truy cập sai index bằng try-catch
    cout << "\n-> Kiem tra truy cap sai vi tri:\n";
    try {
        cout << v.at(5); // Vị trí này không hợp lệ
    } catch (const out_of_range& e) {
        cout << "   [Bat duoc ngoai le] " << e.what() << "\n";
    }

    return 0;
}

### Bài 4: 🔥 Dự Án Mini — Student Score Manager ⭐⭐⭐
> **Cảm hứng:** BaiTapTongHop — Quản lý sinh viên (DSALab)

Xây dựng hệ thống quản lý điểm sinh viên bằng **mảng động**:
- Thêm / xóa / sửa sinh viên (tên, MSSV, điểm)
- Sắp xếp theo điểm (dùng Selection Sort hoặc Bubble Sort)
- Tìm kiếm theo tên hoặc MSSV (Linear Search)
- Thống kê: điểm cao nhất, thấp nhất, trung bình lớp
- Xuất danh sách ra file `diem_sinhvien.txt`

```
=== QUẢN LÝ ĐIỂM SINH VIÊN ===
1. Thêm sinh viên
2. Xóa sinh viên
3. Tìm kiếm
4. Xếp hạng lớp
5. Xuất báo cáo
0. Thoát
```

---
📁 Tham khảo: `Chuong1_TongQuan/Chuong1_TongQuan.cpp`
#include <iostream>
#include <fstream>
#include <iomanip>

using namespace std;

// Struct lưu trữ thông tin của một sinh viên
struct SinhVien {
    int mssv;
    char ho_ten[50];
    float diem;
};

// Lớp quản lý mảng động tự co giãn chứa danh sách sinh viên
class DanhSachSinhVien {
private:
    SinhVien* data;
    int current_size;
    int max_capacity;

    void thayDoiDungLuong(int dung_luong_moi) {
        SinhVien* temp = new SinhVien[dung_luong_moi];
        for (int i = 0; i < current_size; i++) {
            temp[i] = data[i];
        }
        delete[] data;
        data = temp;
        max_capacity = dung_luong_moi;
    }

    // Hàm bổ trợ so sánh chuỗi (Thuần C-style thay thế cho string.Khong dung STL)
    bool SoSanhChuoi(const char* s1, const char* s2) {
        int i = 0;
        while (s1[i] != '\0' && s2[i] != '\0') {
            if (s1[i] != s2[i]) return false;
            i++;
        }
        return s1[i] == s2[i];
    }

public:
    DanhSachSinhVien() {
        current_size = 0;
        max_capacity = 2;
        data = new SinhVien[max_capacity];
    }

    ~DanhSachSinhVien() {
        delete[] data;
    }

    int size() const { return current_size; }

    // 1. Thêm sinh viên (Khấu hao O(1))
    void ThemSinhVien(SinhVien sv) {
        if (current_size == max_capacity) {
            thayDoiDungLuong(2 * max_capacity);
        }
        data[current_size] = sv;
        current_size++;
        cout << "  [✓] Da them sinh vien successfully!\n";
    }

    // 2. Xóa sinh viên theo MSSV (O(n))
    void XoaSinhVien(int mssv_can_xoa) {
        int idx = -1;
        for (int i = 0; i < current_size; i++) {
            if (data[i].mssv == mssv_can_xoa) {
                idx = i;
                break;
            }
        }

        if (idx == -1) {
            cout << "  [X] Khong tim thay sinh vien co MSSV: " << mssv_can_xoa << "\n";
            return;
        }

        // Đẩy dồn các phần tử phía sau lên trước
        for (int i = idx; i < current_size - 1; i++) {
            data[i] = data[i + 1];
        }
        current_size--;
        cout << "  [✓] Da xoa sinh vien co MSSV " << mssv_can_xoa << " khoi he thong.\n";
    }

    // 3. Sửa thông tin điểm theo MSSV (O(n))
    void SuaDiem(int mssv_can_sua, float diem_moi) {
        for (int i = 0; i < current_size; i++) {
            if (data[i].mssv == mssv_can_sua) {
                data[i].diem = diem_moi;
                cout << "  [✓] Cập nhật diem moi thanh cong cho SV: " << data[i].ho_ten << "\n";
                return;
            }
        }
        cout << "  [X] Khong tim thay MSSV hop le.\n";
    }

    // 4. Tìm kiếm tuyến tính (Linear Search) theo MSSV hoặc Tên (O(n))
    void TimKiem(const char* tu_khoa, int mssv_so) {
        bool tim_thay = false;
        cout << "\n--- KET QUA TIM KIEM ---\n";
        for (int i = 0; i < current_size; i++) {
            if (data[i].mssv == mssv_so || SoSanhChuoi(data[i].ho_ten, tu_khoa)) {
                cout << "  -> MSSV: " << data[i].mssv 
                     << " | Ten: " << data[i].ho_ten 
                     << " | Diem: " << data[i].diem << "\n";
                tim_thay = true;
            }
        }
        if (!tim_thay) cout << "  Khong tim thay ket qua trung khop.\n";
    }

    // 5. Sắp xếp theo điểm giảm dần bằng Bubble Sort (O(n^2))
    void SapXepGiamDanTheoDiem() {
        for (int i = 0; i < current_size - 1; i++) {
            for (int j = 0; j < current_size - i - 1; j++) {
                if (data[j].diem < data[j + 1].diem) {
                    swap(data[j], data[j + 1]);
                }
            }
        }
        cout << "  [✓] Da sap xep danh sach theo diem giam dan.\n";
    }

    // 6. Thống kê số liệu lớp học (O(n))
    void ThongKe() {
        if (current_size == 0) {
            cout << "  [!] Danh sach rong, khong the thong ke.\n";
            return;
        }
        float max_d = data[0].diem, min_d = data[0].diem, tong = 0;
        for (int i = 0; i < current_size; i++) {
            if (data[i].diem > max_d) max_d = data[i].diem;
            if (data[i].diem < min_d) min_d = data[i].diem;
            tong += data[i].diem;
        }
        cout << "\n--- THONG KE CHUNG ---\n";
        cout << "  - Diem cao nhat lớp : " << max_d << "\n";
        cout << "  - Diem thap nhat lớp: " << min_d << "\n";
        cout << "  - Diem trung binh   : " << fixed << setprecision(2) << (tong / current_size) << "\n";
    }

    // 7. Xuất định dạng bảng điểm chuyên nghiệp (Ra luồng tổng quát ostream)
    void InBangDiem(ostream& os) {
        os << "\n" << string(52, '=') << "\n";
        os << "║ " << left << setw(10) << "MSSV" 
           << "║ " << setw(25) << "Ho va Ten" 
           << "║ " << setw(10) << "Diem So" << "║\n";
        os << string(52, '=') << "\n";
        for (int i = 0; i < current_size; i++) {
            os << "║ " << left << setw(10) << data[i].mssv 
               ReportRow:
               os << "║ " << setw(25) << data[i].ho_ten 
               << "║ " << setw(10) << fixed << setprecision(1) << data[i].diem << "║\n";
        }
        os << string(52, '=') << "\n";
    }
};

// --- CHƯƠNG TRÌNH ĐIỀU KHIỂN CHÍNH (MENU) ---
int main() {
    DanhSachSinhVien ds;
    int lua_chon;

    // Chèn sẵn một vài dữ liệu mẫu ban đầu để dễ test
    ds.ThemSinhVien({101, "Nguyen Van A", 8.5});
    ds.ThemSinhVien({102, "Tran Thi B", 9.2});
    ds.ThemSinhVien({103, "Le Van C", 6.8});

    do {
        cout << "\n===============================\n";
        cout << "   QUAN LY DIEM SINH VIEN\n";
        cout << "===============================\n";
        cout << "1. Them sinh vien\n";
        cout << "2. Xoa / Sua diem sinh vien\n";
        cout << "3. Tim kiem (Ten/MSSV)\n";
        cout << "4. Xep hang lop (Sort Diem)\n";
        cout << "5. Thong ke & Xuat bao cao file\n";
        cout << "0. Thoat chuong trinh\n";
        cout << "-------------------------------\n";
        cout << "Nhap lua chon cua ban: ";
        
        while (!(cin >> lua_chon)) {
            cin.clear();
            cin.ignore(1000, '\n');
            cout << "Vui long nhap dung con so: ";
        }
        cin.ignore(); // Xóa bộ đệm trôi lệnh nhập chuỗi sau đó

        switch (lua_chon) {
            case 1: {
                SinhVien sv;
                cout << "  - Nhap MSSV (so nguyen): "; cin >> sv.mssv;
                cin.ignore();
                cout << "  - Nhap Ho va Ten: "; cin.getline(sv.ho_ten, 50);
                cout << "  - Nhap Diem: "; cin >> sv.diem;
                ds.ThemSinhVien(sv);
                break;
            }
            case 2: {
                int sub_choice, mssv;
                cout << "  1. Xoa sinh vien | 2. Sua diem: "; cin >> sub_choice;
                cout << "  Nhap MSSV muc tieu: "; cin >> mssv;
                if (sub_choice == 1) {
                    ds.XoaSinhVien(mssv);
                } else {
                    float diem_moi;
                    cout << "  Nhap diem moi thay the: "; cin >> diem_moi;
                    ds.SuaDiem(mssv, diem_moi);
                }
                break;
            }
            case 3: {
                char tu_khoa[50] = "";
                int mssv_tim = -1;
                cout << "  Ban muon tim theo cach nao? (1. Ma so | 2. Tên chu): ";
                int thuc_thi; cin >> thuc_thi;
                cin.ignore();
                if (thuc_thi == 1) {
                    cout << "  Nhap MSSV can tim: "; cin >> mssv_tim;
                } else {
                    cout << "  Nhap dung ho va ten can tim: "; cin.getline(tu_khoa, 50);
                }
                ds.TimKiem(tu_khoa, mssv_tim);
                break;
            }
            case 4: {
                ds.SapXepGiamDanTheoDiem();
                ds.InBangDiem(cout);
                break;
            }
            case 5: {
                ds.InBangDiem(cout);
                ds.ThongKe();
                
                // Ghi dữ liệu đồng thời ra file văn bản
                ofstream f("diem_sinhvien.txt");
                if (f.is_open()) {
                    ds.InBangDiem(f);
                    f.close();
                    cout << "\n  [✓] Da ghi va xuat bao cao ra file 'diem_sinhvien.txt' thành công!\n";
                }
                break;
            }
            case 0:
                cout << "\n Tam biet! Cam on ban da su dung phan mem.\n";
                break;
            default:
                cout << " Lua chon khong hop le, vui long chon tu 0 -> 5!\n";
        }
    } while (lua_chon != 0);

    return 0;
}
