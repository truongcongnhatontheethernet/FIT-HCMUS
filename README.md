# BÁO CÁO ĐỒ ÁN THUẬT TOÁN SORT

## THÔNG TIN CHUNG
- **Tên môn học:** Cấu trúc Dữ liệu và Giải thuật
- **Thời gian học:** Học kỳ 2 - 2025-2026
- **Giáo viên giảng dạy, hướng dẫn:**
  1. Huỳnh Lâm Hải Đăng
  2. Phạm Trọng Nghĩa
  3. Nguyễn Thanh Tình 
  4. Nguyễn Ngọc Đức
- **Danh sách sinh viên nhóm thực hiện đồ án:**
  1. Nguyễn An Thạch MSSV: 25120435
  2. Trương Công Nhật MSSV: 25120405
  3. Trần Nhất Phong MSSV: 25120413

---

## 1. PHÂN TÍCH LẦN CHẠY ĐẦU TIÊN (LẦN 1)

### 1.1. Bài A - Integer Sort
- **Đặc điểm bài toán:** Yêu cầu sắp xếp một dãy gồm N số nguyên. 
- **Thuật toán cài đặt tốt nhất ở lần 1:** Thuật toán Quick Sort phiên bản Đệ quy kết hợp kỹ thuật Median-of-Three và Insertion Sort.
- **Các phương thức tối ưu hóa:**
  - Tối ưu hóa việc chọn pivot : Lấy trung vị của 3 phần tử (Đầu, Giữa, Cuối) để làm chốt. Kỹ thuật này giúp thuật toán né được trường hợp xấu nhất O(N^2) khi đối mặt với các test case mảng đã được sắp xếp sẵn hoặc sắp xếp giảm dần.
  - Kỹ thuật Ngưỡng giới hạn : Thiết lập một ngưỡng an toàn (`right - left <= 15`). Khi mảng được chẻ nhỏ xuống dưới 15 phần tử, thuật toán tự động dừng gọi đệ quy và chuyển sang dùng Insertion Sort. Với các mảng nhỏ, Insertion Sort vượt trội hơn hẳn Quick Sort do hằng số tính toán cực kỳ thấp.
  - Fast I/O: Sử dụng `ios::sync_with_stdio(false);` và `cin.tie(nullptr);` để mở khóa tốc độ đọc/ghi dữ liệu tối đa của C++.
- **Lý giải lựa chọn thuật toán:**
  - Về việc chọn Quick Sort Đệ quy kết hợp Threshold: Bản code này ưu tiên sự cân bằng giữa tính dễ đọc và hiệu năng. Thay vì để cây đệ quy đâm rễ quá sâu gây tốn bộ nhớ Stack, thuật toán đã chủ động cắt tỉa toàn bộ các nhánh lá (những mảng con có kích thước <= 15). Việc kết hợp giữa khả năng phân hoạch mảng lớn xuất sắc của Quick Sort và tốc độ "vét đáy" nhanh gọn của Insertion Sort tạo ra một phiên bản lai ổn định, giúp giữ vững thời gian chạy thực tế ở mức O(N log N).
### 1.2. Bài B - Lexicographic Sort

### 1.3. Bài C - Length-aware Lexicographic String Sort 
- **Đặc điểm bài toán:** Ưu tiên sắp xếp theo độ dài chuỗi trước, nếu độ dài bằng nhau mới xét thứ tự từ điển. Giới hạn  (1s, 256 MB).
- **Thuật toán cài đặt tốt nhất ở lần 1:** Bài C sử dụng thuật toán Randomized QuickSort kết hợp sử dụng mảng con trỏ.
- **Các phương thức tối ưu hóa:**
  - Tối ưu hoá việc chọn pivot: Sử dụng hàm rand() để tiến hành chọn pivot 1 cách ngẫu nhiên, theo mặt xác suất toán học gần như loại bỏ hoàn toàn rơi vào trường hợp tệ nhất.
  - Tối ưu hoán vị: Đổi địa chỉ con trỏ (chi phí O(1)) thay vì sao chép từng ký tự của chuỗi (chi phí O(L)).
  - Kỹ thuật Thoát sớm: Hàm so sánh trả về kết quả ngay khi thấy độ dài chuỗi khác nhau, không cần duyệt tiếp các ký tự.
  - Fast I/O: Sử dụng ios_base::sync_with_stdio(false); và cin.tie(NULL); để tăng tốc độ đọc/ghi dữ liệu tối đa.
- **Lý giải lựa chọn thuật toán:**
  - Về việc chọn thuật toán Randomize QuickSort: Bằng cách dùng hàm rand() để chọn một vị trí ngẫu nhiên làm chốt, bạn dựa vào xác suất toán học để chia đều mảng. Xác suất để rand() liên tục bốc trúng phần tử tệ nhất trong hàng chục ngàn lần lặp là vô cùng nhỏ (gần như bằng 0). Nhờ đó, thời gian chạy thực tế luôn được giữ ổn định ở mức $O(N \log N)$.
  - 

---

## 2. CÁCH THỨC SINH TEST CASE (`test_gen.cpp`)

### 2.1. Cấu trúc và logic sinh test case


Bài A
1.  **Test 1 :** Mảng ngẫu nhiên hoàn toàn, các giá trị được phân phối đều từ âm vô cùng (`-2147483648`) đến dương vô cùng (`2147483647`).
2.  **Test 2 :** Mảng có cấu trúc tăng dần các số chẵn từ đầu đến giữa mảng, và giảm dần từ giữa về cuối mảng.
3.  **Test 3 :** Mảng được sinh thông qua hàm đệ quy `generate_anti_merge()`, cố tình sắp xếp đan xen các phần tử chẵn/lẻ ở mọi phân đoạn để tối đa hóa số lần hoán đổi chéo.
4.  **Test 4 :** Mảng đã được sắp xếp tăng dần tuyệt đối (từ 1 đến 100.000).
5.  **Test 5 :** Mảng chứa cực kỳ nhiều giá trị trùng lặp, chỉ xoay vòng lặp lại ngẫu nhiên đúng 3 giá trị: `INT_MIN`, `0`, và `INT_MAX`.

### 2.2. Thuật toán mục tiêu
Bài A
-   **Quick Sort cơ bản :** Sử dụng chốt là phần tử đầu, phần tử cuối, hoặc phần tử ở chính giữa.
-   **Quick Sort thiếu tối ưu:** Các phiên bản không có cơ chế phân hoạch 3 ngã (3-way partitioning).
-   **Merge Sort:** Thuật toán nổi tiếng với sự ổn định.
-   **Heap Sort:** Thuật toán dựa trên cấu trúc cây nhị phân.

### 2.3. Lý giải việc chọn thuật toán và tác dụng của Test

Bài A
-   **Tác dụng của Test 1 :** Không mang tính chất gài bẫy. Test này đóng vai trò làm thước đo chuẩn (Benchmark) để đánh giá tốc độ thực tế (Average-case) của mọi thuật toán trong điều kiện dữ liệu lý tưởng nhất.
-   **Tác dụng của Test 2 :** Đòn chí mạng nhắm vào **Quick Sort chọn chốt ở giữa (Middle-Pivot)**. Vì mảng đối xứng qua trục giữa, chốt giữa liên tục rơi vào các giá trị cực đại lớn nhất. Điều này khiến Quick Sort không thể chia đôi mảng đều đặn mà liên tục chẻ mảng thành các phần lệch nhau nghiêm trọng, đẩy thời gian chạy từ O(N log N) lên O(N^2).
-   **Tác dụng của Test 3 :** Nhắm vào **Merge Sort**. Dù Merge Sort rất ổn định, nhưng bằng cách phân bố các phần tử đan xen nhau một cách có tính toán, hàm `merge` ở mọi tầng đệ quy sẽ bị ép phải thực hiện số lượng phép so sánh và hoán vị chéo tối đa, vắt kiệt hằng số thời gian chạy của CPU.
-   **Tác dụng của Test 4 :** -   Hủy diệt **Quick Sort (chọn chốt đầu/cuối)**: Mảng đã sắp xếp khiến cây đệ quy của thuật toán này trở thành một danh sách liên kết thẳng đứng (chia mảng thành 0 và N-1 phần tử), gây tràn bộ nhớ Stack và đẩy độ phức tạp lên O(N^2).
    -   Bẫy **Heap Sort**: Khi mảng đã sắp xếp tăng dần, mọi phần tử được đưa lên làm gốc của Max-Heap đều là những số nhỏ nhất. Thuật toán buộc phải thực hiện thao tác "sift-down" sàng lọc phần tử đó rơi xuống tận đáy cây, làm tiêu tốn tối đa chi phí khôi phục cấu trúc Heap.
-   **Tác dụng của Test 5 :** Nhắm vào các phiên bản **Quick Sort không có cơ chế xử lý trùng lặp**. Khi đối mặt với một mảng có quá nhiều giá trị giống hệt nhau, nếu không dùng phân hoạch 3 ngã (chia mảng thành 3 phần: `< pivot`, `= pivot`, `> pivot`), Quick Sort sẽ liên tục thực hiện các phép hoán đổi vô ích giữa các phần tử bằng nhau, dẫn đến tắc nghẽn cục bộ và làm chậm đáng kể thời gian thực thi.
---

## 3. PHÂN TÍCH LẦN CHẠY THỨ HAI (LẦN 2)

### 3.1. Thuật toán và phương thức tối ưu hóa
- **Thuật toán cài đặt tốt nhất:** [Tên thuật toán được dùng ở lần 2]
- **Các phương thức tối ưu hóa liên quan:** - [Liệt kê các kỹ thuật tối ưu mới được áp dụng]

### 3.2. Cách thức tiếp tục tối ưu so với Lần 1
[Giải thích quá trình nâng cấp. Ở lần 1 thuật toán bị nghẽn (bottleneck) ở đâu? Bạn đã làm thế nào để khắc phục nó ở lần 2? (Ví dụ: Thay vì dùng mảng 2 chiều tốn bộ nhớ, đã chuyển sang dùng danh sách kề; hoặc áp dụng Bitwise operations, thêm pragma GCC optimize...)]
