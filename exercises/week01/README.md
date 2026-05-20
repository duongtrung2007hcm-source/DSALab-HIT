# Tuần 1: Tổng Quan C++ & Big-O — Bài tập

## 🎯 Mục tiêu tuần này
Hiểu Big-O, phân tích độ phức tạp, ôn tập C++ cơ bản.
int sum(int a, int b) {
    return a + b; // O(1)//
}
void inMang(vector<int>& arr) {
    for (int i = 0; i < arr.size(); i++) { // O(N)//
        cout << arr[i] << " ";
    }
}
void inCapPhanTu(vector<int>& arr) {
    int n = arr.size();
    for (int i = 0; i < n; i++) {           // O(N)//
        for (int j = 0; j < n; j++) {       // O(N)//
            cout << arr[i] << ", " << arr[j] << endl;
        }
    }
}

### Bài 1: Phân tích Big-O ⭐
++Xác định Big-O của 10 đoạn code C++ cho trước. Giải thích tại sao.
int getElement(vector<int>& arr) {
    return arr[0]; 
}
--Giải thích: Bất kể mảng arr có 10 phần tử hay 10 triệu phần tử, việc truy cập thẳng vào vị trí index 0 luôn chỉ tốn 1 phép toán duy nhất. Thời gian chạy là hằng số
void printItems(int n) {
    for (int i = 0; i < n; i++) {
        cout << i << endl;
    }
}
--Giải thích: Vòng lặp chạy chính xác n lần. Nếu n tăng gấp đôi, thời gian chạy cũng tăng gấp đôi (tỉ lệ thuận).
void printPairs(int n) {
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            cout << i << ", " << j << endl;
        }
    }
}
--Giải thích: Vòng lặp ngoài chạy n lần. Với mỗi lần của vòng lặp ngoài, vòng lặp trong lại chạy n lần. Tổng số phép toán là N x N = N^2
void halveIt(int n) {
    while (n > 1) {
        n = n / 2;
    }
}
--Giải thích: Kích thước của n bị giảm đi một nửa sau mỗi bước (ví dụ: 16 -> 8 -> 4 -> 2 -> 1). Số bước để n về 1 chính là cơ số 2 của logarit $N$. Đây là đặc trưng của các thuật toán cực kỳ hiệu quả như Tìm kiếm nhị phân (Binary Search).
### Bài 2: Đo thời gian thực tế ⭐⭐
++Dùng `chrono` đo thời gian chạy của O(n), O(n²), O(log n) với n = 1.000 → 100.000. In bảng kết quả.
#include <iostream>
#include <chrono>
#include <iomanip>
#include <vector>

using namespace std;
using namespace std::chrono;

// O(log N): Vòng lặp tăng gấp đôi (giống Binary Search)
void runLogN(int n) {
    volatile int sum = 0;
    for (int i = 1; i <= n; i *= 2) {
        sum += i;
    }
}

// O(N): Vòng lặp đơn
void runN(int n) {
    volatile int sum = 0;
    for (int i = 1; i <= n; i++) {
        sum += i;
    }
}

// O(N^2): Vòng lặp lồng nhau
void runN2(int n) {
    volatile int sum = 0;
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= n; j++) {
            sum += i;
        }
    }
}

int main() {
    vector<int> testCases = {1000, 10000, 50000, 100000};
    
    cout << left << setw(10) << "N" 
         << setw(15) << "O(log N)" 
         << setw(15) << "O(N)" 
         << setw(20) << "O(N^2)" << endl;
    cout << string(60, '-') << endl;

    for (int n : testCases) {
        cout << left << setw(10) << n;

        // Đo O(log N)
        auto start = high_resolution_clock::now();
        runLogN(n);
        auto end = high_resolution_clock::now();
        auto logN_time = duration_cast<microseconds>(end - start).count();
        cout << logN_time << setw(15 - to_string(logN_time).length()) << " us";

        // Đo O(N)
        start = high_resolution_clock::now();
        runN(n);
        end = high_resolution_clock::now();
        auto n_time = duration_cast<microseconds>(end - start).count();
        cout << n_time << setw(15 - to_string(n_time).length()) << " us";

        // Đo O(N^2)
        start = high_resolution_clock::now();
        runN2(n);
        end = high_resolution_clock::now();
        auto n2_time = duration_cast<microseconds>(end - start).count();
        
        // Đổi sang milliseconds cho O(N^2) nếu thời gian quá dài
        if (n2_time > 10000) {
            cout << (n2_time / 1000.0) << " ms" << endl;
        } else {
            cout << n2_time << " us" << endl;
        }
    }
    return 0;
}
### Bài 3: Tối ưu hàm ⭐⭐
Cho 3 hàm O(n²) — tối ưu xuống O(n) hoặc O(n log n). Chứng minh bằng cách đo thời gian.
--#include <unordered_set>

bool twoSum_Fast(const vector<int>& arr, int target) {
    unordered_set<int> seen;
    for (int num : arr) {
        int complement = target - num;
        if (seen.count(complement)) return true; // Tìm thấy trong O(1)
        seen.insert(num);
    }
    return false;
}
--#include <algorithm>

bool hasDuplicate_Fast(vector<int> arr) { // Nhận bản sao để sắp xếp
    sort(arr.begin(), arr.end()); // O(N log N)
    for (size_t i = 1; i < arr.size(); i++) {
        if (arr[i] == arr[i - 1]) return true;
    }
    return false;
}
--int maxSubarray_Fast(const vector<int>& arr) {
    int max_sum = INT_MIN;
    int current_sum = 0;
    for (int num : arr) {
        current_sum += num;
        max_sum = max(max_sum, current_sum);
        if (current_sum < 0) current_sum = 0; // Reset nếu bị lỗ
    }
    return max_sum;
}
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
--#include <iostream>
#include <fstream>
#include <chrono>
#include <vector>
#include <iomanip>
#include <string>
#include <cmath>

using namespace std;
using namespace std::chrono;

// --- Các hàm giả lập thuật toán để đo thời gian ---

// O(1) - Thao tác hằng số
void runO1(int n) {
    volatile int a = 0;
    a = 100 + 200; 
}

// O(log n) - Giảm n đi một nửa sau mỗi bước
void runOLogN(int n) {
    volatile int count = 0;
    while (n > 0) {
        count++;
        n /= 2;
    }
}

// O(n) - Vòng lặp tuyến tính đơn
void runON(int n) {
    volatile int count = 0;
    for (int i = 0; i < n; i++) {
        count++;
    }
}

// O(n^2) - Vòng lặp lồng nhau
void runON2(int n) {
    volatile int count = 0;
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            count++;
        }
    }
}

// Hàm hỗ trợ chuyển đổi nano/microgiây sang chuỗi hiển thị ms
string formatDuration(double nanoseconds) {
    double milliseconds = nanoseconds / 1000000.0;
    if (milliseconds < 0.001) {
        // Nếu quá nhỏ, hiển thị dạng khoa học hoặc làm tròn cực nhỏ
        stringstream ss;
        ss << fixed << setprecision(4) << milliseconds << "ms";
        return ss.str();
    }
    stringstream ss;
    ss << fixed << setprecision(3) << milliseconds << "ms";
    return ss.str();
}

// Cấu trúc lưu thông tin dòng dữ liệu
struct AlgorithmResult {
    string name;
    vector<string> times;
};

int main() {
    // Kích thước đầu vào n theo yêu cầu
    vector<int> testCases = {1000, 10000, 100000};
    vector<AlgorithmResult> results = {
        {"O(1)", {}},
        {"O(log n)", {}},
        {"O(n)", {}},
        {"O(n²)", {}}
    };

    cout << "[+] Dang chay BenchmarkTool..." << endl;

    // Đo thời gian cho từng Test Case
    for (int n : testCases) {
        // 1. Đo O(1)
        auto start = high_resolution_clock::now();
        runO1(n);
        auto end = high_resolution_clock::now();
        results[0].times.push_back(formatDuration(duration_cast<nanoseconds>(end - start).count()));

        // 2. Đo O(log n)
        start = high_resolution_clock::now();
        runOLogN(n);
        end = high_resolution_clock::now();
        results[1].times.push_back(formatDuration(duration_cast<nanoseconds>(end - start).count()));

        // 3. Đo O(n)
        start = high_resolution_clock::now();
        runON(n);
        end = high_resolution_clock::now();
        results[2].times.push_back(formatDuration(duration_cast<nanoseconds>(end - start).count()));

        // 4. Đo O(n^2)
        // Lưu ý: n = 100,000 thì n^2 = 10 tỷ phép tính, sẽ rất lâu (khoảng 5-15 giây tùy CPU).
        // Để tránh treo chương trình, ta vẫn chạy nhưng nếu muốn nhanh có thể giả lập hoặc kiên nhẫn đợi.
        start = high_resolution_clock::now();
        runON2(n);
        end = high_resolution_clock::now();
        results[3].times.push_back(formatDuration(duration_cast<nanoseconds>(end - start).count()));
    }

    // --- LOGIC IN BẢNG ĐẸP CHUẨN UNICODE ---
    // Hàm lambda để xuất bảng ra một stream (phục vụ in ra console lẫn ghi file)
    auto printTable = [&](ostream& os) {
        os << "╔══════════════╦══════════╦══════════╦══════════╗\n";
        os << "║ Thuật toán   ║ n=1000   ║ n=10000  ║ n=100000 ║\n";
        os << "╠══════════════╬══════════╬══════════╬══════════╣\n";
        
        for (const auto& res : results) {
            os << "║ " << left << setw(13) << res.name 
               << "║ " << setw(9) << res.times[0]
               << "║ " << setw(9) << res.times[1]
               << "║ " << setw(9) << res.times[2] << "║\n";
        }
        
        os << "╚══════════════╩══════════╩══════════╩══════════╝\n";
    };

    // 1. In ra màn hình Console
    cout << "\n=== KET QUA BENCHMARK ===" << endl;
    printTable(cout);

    // 2. Xuất ra file benchmark.txt
    ofstream outFile("benchmark.txt");
    if (outFile.is_open()) {
        printTable(outFile);
        outFile.close();
        cout << "\n[+] Da xuat ket qua thanh cong ra file 'benchmark.txt'!" << endl;
    } else {
        cerr << "\n[-] Loi: Khong the tao hoac ghi file benchmark.txt!" << endl;
    }

    return 0;
}
---
📁 Tham khảo: `Chuong1_TongQuan/Chuong1_TongQuan.cpp`
