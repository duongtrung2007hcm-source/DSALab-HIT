# Tuần 2: Mảng & Con Trỏ — Bài tập

## 🎯 Mục tiêu tuần này
Thành thạo mảng 1D/2D, con trỏ, cấp phát động trong C++.

---

### Bài 1: Mảng cơ bản ⭐
Nhập mảng n phần tử. Tính min, max, trung bình, tổng. Không dùng STL.
#include <iostream>

using namespace std;

int main() {
    int n;
    cout << "Nhap so luong phan tu n: ";
    cin >> n;

    if (n <= 0) {
        cout << "So luong phan tu phai lon hon 0!" << endl;
        return 1;
    }

    // Cấp phát mảng động (do không được dùng vector)
    int* arr = new int[n];

    cout << "Nhap " << n << " so nguyen:" << endl;
    for (int i = 0; i < n; i++) {
        cout << "arr[" << i << "] = ";
        cin >> arr[i];
    }

    // Khởi tạo các giá trị ban đầu dựa trên phần tử đầu tiên
    int min_val = arr[0];
    int max_val = arr[0];
    
    // Dùng kiểu long long cho tổng để phòng trường hợp tràn số (overflow) nếu mảng lớn
    long long sum = arr[0]; 

    // Duyệt mảng từ vị trí thứ 1 để tính toán
    for (int i = 1; i < n; i++) {
        sum += arr[i];
        
        if (arr[i] < min_val) {
            min_val = arr[i];
        }
        if (arr[i] > max_val) {
            max_val = arr[i];
        }
    }

    // Tính trung bình (ép kiểu double để có số thập phân)
    double average = (double)sum / n;

    // In kết quả
    cout << "\n=== KET QUA ===" << endl;
    cout << "Tong        : " << sum << endl;
    cout << "Gia tri Min : " << min_val << endl;
    cout << "Gia tri Max : " << max_val << endl;
    cout << "Trung binh  : " << average << endl;

    // Giải phóng bộ nhớ của mảng động
    delete[] arr;

    return 0;
}
### Bài 2: Mảng 2D ⭐⭐
Nhân 2 ma trận n×n. Tính định thức ma trận 3×3. Hiển thị đẹp.
#include <iostream>
#include <vector>
#include <iomanip>
#include <string>

using namespace std;

// Hàm hiển thị ma trận đẹp mắt
void xuatMaTran(const vector<vector<int>>& mat) {
    int rows = mat.size();
    int cols = mat[0].size();
    for (int i = 0; i < rows; i++) {
        cout << "  [ ";
        for (int j = 0; j < cols; j++) {
            cout << setw(6) << mat[i][j] << " ";
        }
        cout << "]\n";
    }
}

// 1. Hàm nhân hai ma trận n x n - Độ phức tạp: O(n³)
void nhanMaTran() {
    int n;
    cout << "\n--- NHÂN HAI MA TRẬN N x N ---\n";
    cout << "Nhap kich thuoc n: ";
    cin >> n;

    vector<vector<int>> A(n, vector<int>(n));
    vector<vector<int>> B(n, vector<int>(n));
    vector<vector<int>> C(n, vector<int>(n, 0));

    cout << "\n-> Nhap ma tran A (" << n << "x" << n << "):\n";
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            cout << "A[" << i << "][" << j << "] = ";
            cin >> A[i][j];
        }
    }

    cout << "\n-> Nhap ma tran B (" << n << "x" << n << "):\n";
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            cout << "B[" << i << "][" << j << "] = ";
            cin >> B[i][j];
        }
    }

    // Thuật toán nhân ma trận kinh điển sử dụng 3 vòng lặp
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            for (int k = 0; k < n; k++) {
                C[i][j] += A[i][k] * B[k][j];
            }
        }
    }

    // Hiển thị kết quả trực quan
    cout << "\nMa tran A:\n"; xuatMaTran(A);
    cout << "\nMa tran B:\n"; xuatMaTran(B);
    cout << "\n====> KET QUA TICH A x B:\n"; xuatMaTran(C);
}

// 2. Hàm tính định thức ma trận 3 x 3 - Độ phức tạp: O(1) do kích thước cố định
void tinhDinhThuc3x3() {
    cout << "\n--- TÍNH ĐỊNH THỨC MA TRẬN 3 x 3 ---\n";
    vector<vector<int>> M(3, vector<int>(3));

    cout << "Nhap ma tran M (3x3):\n";
    for (int i = 0; i < 3; i++) {
        for (int j = 0; j < 3; j++) {
            cout << "M[" << i << "][" << j << "] = ";
            cin >> M[i][j];
        }
    }

    cout << "\nMa tran M vua nhap:\n";
    xuatMaTran(M);

    // Áp dụng quy tắc Sarrus / Khai triển đồng cấu
    int det = M[0][0] * (M[1][1] * M[2][2] - M[1][2] * M[2][1])
            - M[0][1] * (M[1][0] * M[2][2] - M[1][2] * M[2][0])
            + M[0][2] * (M[1][0] * M[2][1] - M[1][1] * M[2][0]);

    cout << "\n====> KẾT QUẢ: det(M) = " << det << "\n";
}

int main() {
    int luaChon;
    do {
        cout << "\n====================================\n";
        cout << "        UNG DUNG MA TRAN C++\n";
        cout << "====================================\n";
        cout << "1. Nhan hai ma tran n x n\n";
        cout << "2. Tinh dinh thuc ma tran 3 x 3\n";
        cout << "0. Thoat\n";
        cout << "------------------------------------\n";
        cout << "Nhap lua chon cua ban: ";
        cin >> luaChon;

        switch (luaChon) {
            case 1: nhanMaTran(); break;
            case 2: tinhDinhThuc3x3(); break;
            case 0: cout << "=> Tam biet!\n"; break;
            default: cout << "=> Lua chon khong hop le, vui long chon lai!\n";
        }
    } while (luaChon != 0);

    return 0;
}
### Bài 3: Con trỏ & cấp phát động ⭐⭐
Cài đặt mảng động tự resize (như `std::vector` đơn giản). Hỗ trợ push_back, pop_back, at(i).
#include <iostream>
#include <string>
#include <stdexcept> // Để dùng std::out_of_range

using namespace std;

template <typename T>
class MyVector {
private:
    T* data;         // Con trỏ quản lý mảng động
    int capacity;    // Sức chứa tối đa hiện tại
    int currentSize; // Số lượng phần tử thực tế đang có

    // Hàm tự động nhân đôi bộ nhớ (Resize)
    void resize() {
        capacity *= 2;                  // 1. Gấp đôi sức chứa
        T* newData = new T[capacity];   // 2. Xin cấp phát một mảng mới to hơn

        // 3. Chép toàn bộ dữ liệu từ mảng cũ sang mảng mới
        for (int i = 0; i < currentSize; i++) {
            newData[i] = data[i];
        }

        delete[] data;  // 4. Giải phóng mảng cũ (rất quan trọng để không rò rỉ bộ nhớ)
        data = newData; // 5. Trỏ lại data vào mảng mới
    }

public:
    // Constructor: Khởi tạo mảng với sức chứa ban đầu là 2
    MyVector() {
        capacity = 2;
        currentSize = 0;
        data = new T[capacity];
    }

    // Destructor: Dọn dẹp bộ nhớ khi vector bị hủy
    ~MyVector() {
        delete[] data;
    }

    // Thêm phần tử vào cuối mảng
    void push_back(T value) {
        if (currentSize == capacity) {
            resize(); // Nếu đầy thì tự động mở rộng
        }
        data[currentSize] = value;
        currentSize++;
    }

    // Xóa phần tử ở cuối mảng
    void pop_back() {
        if (currentSize > 0) {
            currentSize--; // Chỉ cần giảm biến đếm, phần tử sẽ bị ghi đè sau này
        }
    }

    // Truy cập phần tử an toàn (có kiểm tra tràn viền)
    T& at(int index) {
        if (index < 0 || index >= currentSize) {
            throw out_of_range("Loi: Truy cap ngoai pham vi mang!");
        }
        return data[index];
    }

    // Các hàm tiện ích để xem trạng thái mảng
    int size() const { return currentSize; }
    int get_capacity() const { return capacity; }
};

int main() {
    cout << "=== TEST MYVECTOR VOI KIEU INT ===\n";
    MyVector<int> vec;
    
    // Thêm liên tục 5 phần tử để xem cách mảng tự mở rộng
    for (int i = 1; i <= 5; i++) {
        vec.push_back(i * 10);
        cout << "+ Them " << (i * 10) 
             << " | Size: " << vec.size() 
             << " | Capacity: " << vec.get_capacity() << "\n";
    }

    cout << "\nTruy cap vec.at(2): " << vec.at(2) << "\n";

    cout << "\nThu pop_back() 2 lan:\n";
    vec.pop_back();
    vec.pop_back();
    cout << "Size hien tai: " << vec.size() << " | Capacity: " << vec.get_capacity() << "\n";

    // In mảng còn lại
    cout << "Cac phan tu con lai: ";
    for (int i = 0; i < vec.size(); i++) {
        cout << vec.at(i) << " ";
    }
    cout << "\n\n";

    cout << "=== TEST MYVECTOR VOI KIEU STRING ===\n";
    MyVector<string> strVec;
    strVec.push_back("Hello");
    strVec.push_back("C++");
    strVec.push_back("DSA");
    
    for (int i = 0; i < strVec.size(); i++) {
        cout << strVec.at(i) << " ";
    }
    cout << "\n";

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
+++
#include <iostream>
#include <vector>
#include <string>
#include <iomanip>
#include <fstream>

using namespace std;

// Cấu trúc sinh viên
struct SinhVien {
    string maSV;
    string hoTen;
    double diem;
};

// 1. Hàm thêm sinh viên
void themSinhVien(vector<SinhVien>& ds) {
    SinhVien sv;
    cout << "\n--- THEM SINH VIEN ---\n";
    cout << "Nhap Ma SV: ";
    cin >> sv.maSV;
    
    cin.ignore();
    cout << "Nhap Ho Ten: ";
    getline(cin, sv.hoTen);
    
    cout << "Nhap Diem (0-10): ";
    cin >> sv.diem;
    
    ds.push_back(sv);
    cout << "=> Da them sinh vien thanh cong!\n";
}

// 2. Hàm xóa / sửa sinh viên (Tích hợp)
void xoaSuaSinhVien(vector<SinhVien>& ds) {
    if (ds.empty()) {
        cout << "\n=> Danh sach hien dang trong!\n";
        return;
    }

    string maTim;
    cout << "\n--- XOA / SUA SINH VIEN ---\n";
    cout << "Nhap Ma SV can thao tac: ";
    cin >> maTim;
    
    for (int i = 0; i < ds.size(); i++) {
        if (ds[i].maSV == maTim) {
            cout << "Tim thay SV: " << ds[i].hoTen << " | Diem: " << ds[i].diem << "\n";
            cout << "Ban muon lam gi? (1: Xoa, 2: Sua, 0: Huy): ";
            int chon;
            cin >> chon;
            
            if (chon == 1) {
                ds.erase(ds.begin() + i);
                cout << "=> Da xoa sinh vien!\n";
            } else if (chon == 2) {
                cout << "Nhap Ma SV moi (nhap lai neu khong doi): ";
                cin >> ds[i].maSV;
                
                cin.ignore();
                cout << "Nhap Ho Ten moi: ";
                getline(cin, ds[i].hoTen);
                
                cout << "Nhap Diem moi: ";
                cin >> ds[i].diem;
                cout << "=> Da cap nhat thong tin!\n";
            }
            return;
        }
    }
    cout << "=> Khong tim thay sinh vien co ma: " << maTim << "\n";
}

// 3. Tìm kiếm theo Mã SV hoặc Tên (Linear Search)
void timKiem(const vector<SinhVien>& ds) {
    if (ds.empty()) {
        cout << "\n=> Danh sach hien dang trong!\n";
        return;
    }

    string tuKhoa;
    cout << "\n--- TIM KIEM SINH VIEN ---\n";
    cout << "Nhap Ma SV hoac Ten can tim: ";
    cin.ignore();
    getline(cin, tuKhoa);
    
    bool timThay = false;
    cout << "\n" << string(50, '-') << "\n";
    for (const auto& sv : ds) {
        // Tìm kiếm tuyến tính: Khớp mã SV hoặc một phần của Tên
        if (sv.maSV == tuKhoa || sv.hoTen.find(tuKhoa) != string::npos) {
            cout << left << setw(10) << sv.maSV 
                 << setw(25) << sv.hoTen 
                 << "Diem: " << sv.diem << "\n";
            timThay = true;
        }
    }
    cout << string(50, '-') << "\n";
    
    if (!timThay) cout << "=> Khong co ket qua nao phu hop!\n";
}

// 4. Xếp hạng lớp bằng Bubble Sort (Giảm dần)
void xepHang(vector<SinhVien>& ds) {
    if (ds.empty()) {
        cout << "\n=> Danh sach hien dang trong!\n";
        return;
    }
    
    int n = ds.size();
    for (int i = 0; i < n - 1; i++) {
        for (int j = 0; j < n - i - 1; j++) {
            if (ds[j].diem < ds[j + 1].diem) {
                // Hoán đổi vị trí nếu điểm người sau lớn hơn người trước
                swap(ds[j], ds[j + 1]);
            }
        }
    }
    cout << "\n=> Da sap xep danh sach theo diem giam dan (Bubble Sort)!\n";
}

// 5. Thống kê & Xuất báo cáo ra File
void xuatBaoCao(const vector<SinhVien>& ds) {
    if (ds.empty()) {
        cout << "\n=> Danh sach hien dang trong, khong co gi de xuat!\n";
        return;
    }

    // Tính toán thống kê
    double minDiem = ds[0].diem;
    double maxDiem = ds[0].diem;
    double tongDiem = 0;
    
    for (const auto& sv : ds) {
        if (sv.diem < minDiem) minDiem = sv.diem;
        if (sv.diem > maxDiem) maxDiem = sv.diem;
        tongDiem += sv.diem;
    }
    double trungBinh = tongDiem / ds.size();

    // Mở file để ghi
    ofstream outFile("diem_sinhvien.txt");
    
    if (!outFile.is_open()) {
        cout << "\n[-] Loi: Khong the tao file diem_sinhvien.txt!\n";
        return;
    }

    // Lambda helper để in ra cả Console lẫn File cùng một lúc (tránh lặp code)
    auto printLine = [&](const string& text) {
        cout << text << "\n";
        outFile << text << "\n";
    };

    printLine("\n================ BÁO CÁO ĐIỂM ==================");
    
    // In Header
    cout << left << setw(10) << "MA SV" << setw(25) << "HO TEN" << setw(10) << "DIEM" << "XEP LOAI\n";
    outFile << left << setw(10) << "MA SV" << setw(25) << "HO TEN" << setw(10) << "DIEM" << "XEP LOAI\n";
    
    printLine(string(55, '-'));
    
    // In dữ liệu từng sinh viên
    for (const auto& sv : ds) {
        string xepLoai = (sv.diem >= 8.5) ? "Gioi" : (sv.diem >= 7.0) ? "Kha" : (sv.diem >= 5.0) ? "Trung Binh" : "Yeu";
        
        cout << left << setw(10) << sv.maSV << setw(25) << sv.hoTen << setw(10) << sv.diem << xepLoai << "\n";
        outFile << left << setw(10) << sv.maSV << setw(25) << sv.hoTen << setw(10) << sv.diem << xepLoai << "\n";
    }
    
    printLine(string(55, '-'));
    
    // In Thống kê
    cout << fixed << setprecision(2);
    outFile << fixed << setprecision(2);
    
    printLine(">> THONG KE LOP <<");
    printLine("- Si so      : " + to_string(ds.size()));
    printLine("- Diem cao nhat : " + to_string(maxDiem));
    printLine("- Diem thap nhat: " + to_string(minDiem));
    printLine("- Trung binh lop: " + to_string(trungBinh));
    printLine("================================================");
    
    outFile.close();
    cout << "\n[+] Da xuat bao cao ra man hinh va luu vao file 'diem_sinhvien.txt' thanh cong!\n";
}

int main() {
    vector<SinhVien> danhSachSV;
    int luaChon;
    
    do {
        cout << "\n=== QUAN LY DIEM SINH VIEN ===\n";
        cout << "1. Them sinh vien\n";
        cout << "2. Xoa / Sua sinh vien\n";
        cout << "3. Tim kiem\n";
        cout << "4. Xep hang lop (Bubble Sort)\n";
        cout << "5. Xuat bao cao & Thong ke\n";
        cout << "0. Thoat\n";
        cout << "==============================\n";
        cout << "Nhap lua chon: ";
        cin >> luaChon;
        
        switch (luaChon) {
            case 1: themSinhVien(danhSachSV); break;
            case 2: xoaSuaSinhVien(danhSachSV); break;
            case 3: timKiem(danhSachSV); break;
            case 4: xepHang(danhSachSV); break;
            case 5: xuatBaoCao(danhSachSV); break;
            case 0: cout << "=> Tam biet!\n"; break;
            default: cout << "=> Khong hop le!\n";
        }
    } while (luaChon != 0);

    return 0;
}
```

---
📁 Tham khảo: `Chuong1_TongQuan/Chuong1_TongQuan.cpp`
