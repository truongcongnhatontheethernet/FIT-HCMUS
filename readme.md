# BÁO CÁO ĐỒ ÁN THUẬT TOÁN SORT

## THÔNG TIN CHUNG
- **Môn học:** Cấu trúc Dữ liệu và Giải thuật — HK2 2025-2026
- **Giáo viên:** Huỳnh Lâm Hải Đăng, Phạm Trọng Nghĩa, Nguyễn Thanh Tình, Nguyễn Ngọc Đức
- **Nhóm:** Nguyễn An Thạch (25120435), Trương Công Nhật (25120405), Trần Nhất Phong (25120413)

---

## 1. PHÂN TÍCH LẦN CHẠY ĐẦU TIÊN

### Bài A – Integer Sort
- **Bài toán:** Sắp xếp N ≤ 10⁵ số nguyên, giá trị −2³¹ ≤ x ≤ 2³¹−1, giới hạn 10s/1024MB.
- **Thuật toán tốt nhất lần 1:** Quick Sort đệ quy kết hợp Median-of-Three và Insertion Sort.
- **Tối ưu hóa:**
  - *Median-of-Three Pivot:* Chốt là trung vị của 3 phần tử (đầu, giữa, cuối), giúp tránh trường hợp xấu nhất O(N²) khi mảng đã sắp xếp tăng hoặc giảm dần — tình huống khiến Quick Sort chọn chốt cố định chia mảng thành hai phần 0 và N−1 liên tục, gây cây đệ quy mất cân bằng hoàn toàn.
  - *Ngưỡng Insertion Sort (≤ 15):* Khi đoạn con ≤ 15 phần tử, dừng đệ quy và chuyển sang Insertion Sort. Với mảng nhỏ, Insertion Sort vượt trội nhờ hằng số tính toán cực thấp, tính cục bộ bộ nhớ tốt, và tránh cây đệ quy quá sâu gây áp lực không cần thiết lên bộ nhớ stack.
  - *Fast I/O:* `ios::sync_with_stdio(false)` và `cin.tie(nullptr)` mở khóa tốc độ đọc/ghi tối đa của luồng chuẩn C++.
- **Lý giải:** Cân bằng hiệu năng và tính dễ đọc. Quick Sort phân hoạch nhanh trên đoạn lớn; Insertion Sort tận dụng tính cục bộ cache trên đoạn nhỏ. Không tiếp tục đệ quy các mảng con nhỏ còn tiết kiệm bộ nhớ stack và giảm overhead lời gọi hàm, đạt O(N log N) ổn định trên thực tế.

### Bài B – Lexicographic Sort
- **Bài toán:** Sắp xếp từ điển N ≤ 10⁴ chuỗi (dài 10–100 ký tự), giới hạn 10s/1024MB.
- **Thuật toán tốt nhất lần 1:** Bucket Sort theo 2 ký tự đầu (676 nhóm) + Iterative Quick Sort + Insertion Sort, toàn bộ thao tác qua mảng chỉ số `idx`.
- **Tối ưu hóa:**
  - *Bucket Sort 676 nhóm:* Tiền xử lý chia mảng lớn thành các nhóm nhỏ độc lập theo 2 ký tự đầu tiên ($26 \times 26 = 676$ bucket), giảm đáng kể số lần gọi sắp xếp và phạm vi mỗi lần so sánh.
  - *Sắp xếp gián tiếp qua `idx`:* Không bao giờ hoán vị trực tiếp đối tượng `std::string`. Chỉ đổi số nguyên trong mảng chỉ số `idx` với chi phí O(1), loại bỏ hoàn toàn bottleneck cấp phát và di chuyển bộ nhớ chuỗi.
  - *Iterative Quick Sort:* Dùng `stack_left`/`stack_right` giả lập stack thủ công, khử đệ quy, tránh stack overflow và giảm function-call overhead.
  - *Median-of-Three pivot* + *ngưỡng ≤ 15* chuyển Insertion Sort + Fast I/O.
- **Lý giải:** Phân nhóm 2 ký tự đầu hoạt động như bước radix, triệt tiêu so sánh tiền tố trên toàn bộ 10⁴ chuỗi. Thao tác gián tiếp qua mảng chỉ số loại bỏ nút thắt cổ chai bộ nhớ chuỗi. Việc tự triển khai hệ thống quản lý stack cho Quick Sort đẩy tốc độ và sự ổn định của chương trình lên mức tối đa để vượt qua các bộ test hiệu năng khắt khe.

### Bài C – Length-aware Lexicographic Sort
- **Bài toán:** Ưu tiên sắp xếp theo độ dài chuỗi trước, cùng độ dài mới xét thứ tự từ điển. Giới hạn 1s/256MB.
- **Thuật toán tốt nhất lần 1:** Randomized Quick Sort kết hợp mảng con trỏ ký tự.
- **Tối ưu hóa:**
  - *Pivot ngẫu nhiên `rand()`:* Xác suất chọn liên tục phần tử tệ nhất trong hàng chục nghìn lần phân hoạch gần bằng 0 theo lý thuyết xác suất, loại trừ thực tế trường hợp O(N²). Thời gian chạy thực tế luôn được giữ ổn định ở O(N log N).
  - *Hoán vị con trỏ O(1):* Đổi địa chỉ con trỏ thay vì sao chép từng ký tự chuỗi O(L), giảm mạnh chi phí hoán đổi ở mỗi bước phân hoạch.
  - *Thoát sớm:* Hàm so sánh trả kết quả ngay khi phát hiện độ dài khác nhau, không duyệt tiếp ký tự không cần thiết.
  - *Fast I/O.*
- **Lý giải:** Randomized pivot đảm bảo phân hoạch đều theo xác suất, giữ O(N log N) ổn định trên thực tế. Hoán vị con trỏ và thoát sớm cùng nhau giảm mạnh chi phí ẩn trong từng bước so sánh và phân hoạch.

---

## 2. CÁCH THỨC SINH TEST CASE

### Bài A – Integer Sort

**Logic sinh test và tác dụng:**
- **Test001 — Ngẫu nhiên hoàn toàn (INT_MIN → INT_MAX):** Không chứa bẫy, đóng vai trò baseline thước đo tốc độ thực tế cho mọi thuật toán trong điều kiện dữ liệu lý tưởng.
- **Test002 — Tăng dần rồi giảm dần:** Nhắm Quick Sort chọn chốt giữa. Cấu trúc mảng gây phân hoạch mất cân bằng đáng kể ở nhiều bước liên tiếp, làm số lời gọi đệ quy và chi phí phân hoạch tăng rõ rệt so với dữ liệu ngẫu nhiên.
- **Test003 — Anti-Merge (hàm đệ quy `generate_anti_merge`, đan xen chẵn/lẻ mọi tầng):** Nhắm Merge Sort. Cấu trúc này khiến mỗi bước merge phải thực hiện gần như tối đa số phép so sánh giữa hai mảng con, làm tăng hằng số thời gian thực thi đáng kể.
- **Test004 — Sắp xếp tăng dần hoàn toàn:** Nhắm Quick Sort chốt đầu/cuối → phân hoạch liên tục 0 vs N−1, tiệm cận O(N²). Đồng thời tạo áp lực Heap Sort: khi xây Max-Heap từ mảng tăng dần, nhiều phần tử nhỏ ở gần đỉnh phải sift-down rất dài, tăng chi phí thực tế đáng kể.
- **Test005 — Chỉ 3 giá trị lặp (INT_MIN, 0, INT_MAX):** Nhắm Quick Sort không phân hoạch 3 ngã. Quá nhiều phần tử bằng nhau khiến thuật toán thực hiện nhiều phép hoán đổi và so sánh vô ích, tăng độ sâu đệ quy và làm suy giảm hiệu năng nghiêm trọng.

**Thuật toán mục tiêu:** Quick Sort chốt cố định (đầu/cuối/giữa), Quick Sort thiếu phân hoạch 3 ngã, Merge Sort, Heap Sort. Lý do: nhắm vào các thuật toán chưa có khả năng tối ưu hóa và ứng biến trước dữ liệu bất lợi.

### Bài B – Lexicographic Sort

**Logic sinh test và tác dụng:**
- **Test001 — Trùng lặp hoàn toàn:** Quick Sort cơ bản không thể chia đều mảng, liên tục phân hoạch thành 0 và N−1, đẩy độ phức tạp lên O(N²) và dễ gây stack overflow.
- **Test002 — Gần giống, khác 1 ký tự cuối:** Ép phép so sánh chuỗi phải duyệt hết chiều dài L mới tìm khác biệt. Chi phí một phép so sánh bị đẩy lên tối đa O(L), làm chậm toàn bộ tiến trình.
- **Test003 — Sắp xếp ngược hoàn toàn:** Insertion Sort và Bubble Sort phải thực hiện tối đa O(N²) phép hoán đổi và dời chỗ. Đồng thời Quick Sort chốt đầu/cuối bị lệch hoàn toàn ở mọi bước phân hoạch.
- **Test004 — Chỉ ký tự {a, b} — độ đa dạng thấp:** Quá nhiều giá trị gần bằng nhau khiến Quick Sort cơ bản khó phân hoạch đều, dễ mất cân bằng cây đệ quy và giảm hiệu năng thực tế.
- **Test005 — Ngẫu nhiên hoàn toàn {a–z}:** Baseline kiểm tra liệu thuật toán có đạt O(N log N) kỳ vọng trong Average-case.

**Thuật toán mục tiêu:** Quick Sort cơ bản (Lomuto/Hoare, pivot cố định), Radix Sort MSD, các O(N²) sorts, thuật toán chuẩn (Merge/Heap/IntroSort) làm mốc so sánh. Lý do: nhắm vào các thuật toán có điểm yếu chí mạng trong thiết kế như cách chọn pivot cố định, phân hoạch khi có khóa trùng, hoặc chi phí duyệt chuỗi.

### Bài C – Length-aware Lexicographic Sort

Bộ test khai thác "chi phí ẩn" O(L) của phép so sánh chuỗi thay vì O(1) như số nguyên. Sinh N = 100.000 chuỗi dài 100 ký tự, tập trung vào các cấu trúc tiền tố giống nhau kéo dài để làm khó các thuật toán dựa trên phép so sánh.

**Logic sinh test và tác dụng:**
- **Test001 — Tiền tố 90`a` + 10 ký tự ngẫu nhiên:** Buộc duyệt gần toàn bộ chuỗi mới tìm khác biệt, chi phí so sánh tiệm cận O(L).
- **Test002 — Ngẫu nhiên hoàn toàn {a–z}:** Baseline kiểm tra Average-case khách quan, xác định thuật toán có đạt O(N log N) chuẩn hay không.
- **Test003 — 8`a` + 91`b` + 1 ký tự đột biến cuối:** Chi phí mỗi phép so sánh bị đẩy lên cực đại O(L), tạo nghẽn cổ chai nghiêm trọng cho toàn bộ tiến trình sắp xếp.
- **Test004 — 96`a` + 3 ký tự ngẫu nhiên + `a` cuối:** Bẫy thuật toán tối ưu ngây thơ chỉ xét ký tự đầu/cuối. Ký tự cuối luôn là `a` nên thuật toán đó không thể phân biệt đúng thứ tự mà vẫn tưởng đã tối ưu.
- **Test005 — Tiền tố 86`a` + đuôi 14 ký tự chỉ {a, b}:** Rất nhiều chuỗi trùng/gần trùng → Quick Sort cơ bản phân hoạch kém, nguy cơ sụp đổ từ O(N log N) xuống O(N²).

**Thuật toán mục tiêu:** Quick Sort cơ bản, Radix Sort MSD, các thuật toán O(N²), các thuật toán tối ưu chuẩn làm mốc so sánh.

---

## 3. PHÂN TÍCH LẦN CHẠY THỨ HAI

### Bài A – Integer Sort
- **Thuật toán tốt nhất:** IntroSort tối ưu thủ công.
- **Tối ưu hóa:**
  - *Heap Sort fallback:* Khi độ sâu đệ quy > `2 * __lg(n)`, tự động chuyển sang Heap Sort, đảm bảo O(N log N) worst-case kể cả với test cố tình khai thác điểm yếu Quick Sort.
  - *Tail Recursion Optimization:* Luôn đệ quy vào nhánh nhỏ hơn trước, giảm mạnh số lời gọi hàm và độ sâu call stack.
  - *Hoare Partition:* Giảm số phép hoán đổi, cải thiện cache locality CPU so với partition kiểu cũ.
  - *Ngưỡng Insertion Sort tăng 15 → 24* để tối ưu hằng số thời gian trên đoạn nhỏ.
  - *Final Insertion Sort* một lần toàn mảng sau phân hoạch gần xong — tận dụng tính "gần sorted" tổng thể, hiệu quả hơn nhiều lần gọi rời rạc như ở Lần 1.
  - `inline` các hàm nhỏ (`swap_`, `insertion_sort`, `sift_down`); dùng `__lg(n)` thay `log2(n)` để tính ngưỡng nhanh hơn ở cấp độ instruction.
- **Cải tiến so với Lần 1:** Lần 1 đệ quy cả 2 nhánh gây áp lực stack; Lần 2 Tail Recursion tối ưu. Lần 1 partition cũ nhiều swap; Lần 2 Hoare Partition cải thiện. Lần 1 Quick Sort thuần nguy cơ O(N²) với test đặc biệt; Lần 2 IntroSort tự fallback Heap Sort bảo vệ worst-case. Lần 1 Insertion Sort gọi rời rạc chưa tận dụng tính gần sorted tổng thể; Lần 2 Final Insertion Sort một lần khai thác toàn diện.

### Bài B – Lexicographic Sort
- **Thuật toán tốt nhất:** MSD Radix/Bucket Sort (2 ký tự đầu) + Iterative Quick Sort gián tiếp + Insertion Sort.
- **Tối ưu hóa:**
  - *Sắp xếp gián tiếp `idx` + `temp_idx`:* Toàn bộ hoán đổi trong Quick Sort thực hiện trên `int`, triệt tiêu hoàn toàn overhead bộ nhớ `std::string`.
  - *676 bucket:* Sau khi băm 2 ký tự đầu vào nhóm, các bước phân hoạch tiếp theo không cần so sánh lại tiền tố đã biết — loại bỏ lượng lớn phép so sánh dư thừa trong toàn bộ quá trình.
  - *Lọc bucket rỗng:* Chỉ kích hoạt sắp xếp với bucket có `count > 1`, loại bỏ hàng nghìn lần gọi hàm vô nghĩa, tiết kiệm chu kỳ CPU.
  - *Iterative Quick Sort* bằng stack thủ công, triệt tiêu nguy cơ stack overflow dưới áp lực test lớn.
  - Median-of-Three + ngưỡng ≤ 15 chuyển Insertion Sort.
- **Cải tiến so với Lần 1:** Lần 1 Quick Sort dễ phân hoạch xấu; Lần 2 bổ sung lọc bucket rỗng và Median-of-Three. Lần 1 thao tác trực tiếp `std::string` tốn kém; Lần 2 toàn bộ trên `int` gián tiếp. Lần 1 tiền tố chưa tận dụng; Lần 2 băm 2 ký tự đầu cắt bỏ hoàn toàn phép so sánh tiền tố trong các bước phân hoạch sau. Nhờ các cải tiến này, thuật toán Lần 2 ổn định và nhanh hơn đáng kể trên mọi loại test.

### Bài C – Length-aware Lexicographic Sort
- **Thuật toán tốt nhất:** Counting Sort theo chiều dài + Iterative Quick Sort gián tiếp + Insertion Sort.
- **Tối ưu hóa:**
  - *Counting Sort O(N):* Tách pha phân nhóm theo chiều dài thành bước xử lý riêng; chiều dài chỉ được tính 1 lần duy nhất trong pha nhập liệu, loại bỏ hoàn toàn việc so sánh và tính chiều dài trong tất cả các bước phân hoạch sau.
  - *Indirect Sorting qua `idx`:* Swap chuỗi từ O(L) về O(1). Mảng toàn cục tĩnh (`arr`, `idx`, `temp_idx` với MAXN cố định) tránh overhead cấp phát động rải rác và phân mảnh heap.
  - *Iterative Quick Sort* bằng mảng stack thủ công, loại bỏ hoàn toàn rủi ro runtime error do đệ quy sâu quá mức.
  - Quick Sort chỉ chạy giới hạn trong từng nhóm cùng chiều dài + fallback Insertion Sort khi nhóm ≤ 15 phần tử.
  - Trích xuất `.length()` đúng 1 lần trong pha nhập liệu, triệt tiêu chi phí đếm độ dài dư thừa ở mọi bước sau.
- **Cải tiến so với Lần 1:** Lần 1 `strlen()` gọi lại mỗi bước phân hoạch tạo bottleneck O(L) lặp hàng chục nghìn lần; Lần 2 trích xuất 1 lần duy nhất triệt tiêu hoàn toàn. Lần 1 đệ quy 2 nhánh gây áp lực call stack, nguy cơ runtime error; Lần 2 Iterative Quick Sort khử hoàn toàn. Lần 1 Quick Sort phải xử lý toàn bộ tập dữ liệu chiều dài lộn xộn; Lần 2 Counting Sort tách nhóm, Quick Sort chỉ chạy trong nhóm cùng chiều dài. Lần 1 cấp phát `new/delete` tốn kém, phân mảnh heap; Lần 2 mảng tĩnh toàn cục không overhead động.
