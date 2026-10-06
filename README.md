# ITB Customer Financial Health Analysis

> Khách hàng thẻ của ITB trung thành và hoạt động nhiều, nhưng sức khỏe tài chính của họ lên xuống theo chính nhịp chi tiêu của họ. Từ một năm dữ liệu thẻ, dự án xác định nhóm nào đang ổn, nhóm nào đang căng và nhóm nào đang rời xa, rồi đề xuất các cách hỗ trợ **không mang tính trừng phạt** để mỗi nhóm chi tiêu trong khả năng.

**Câu hỏi kinh doanh:** Làm sao để ITB vừa cải thiện sức khỏe tài chính của khách hàng, vừa tăng mức độ gắn kết, mà không phạt ai?

**Vai trò:** Data Analyst · Cuộc thi BI10 Round 1
**Công cụ:** Python (pandas, scipy, scikit-learn, matplotlib), SQL, Jupyter

---

## 1. Dữ liệu

| | |
| --- | --- |
| Phạm vi | 999 khách hàng thẻ tại Việt Nam, năm 2025 |
| Giao dịch | 1.852.394 giao dịch, tổng chi tiêu **3.244,6 tỷ VND** |
| Bảng theo tháng | 10.992 dòng *khách hàng × tháng*: thu nhập, chi tiêu, dư nợ, hạn mức, điểm sức khỏe tài chính, điểm gắn kết |
| Bảng giao dịch | Ngày giờ, ngành hàng (14 nhóm), kênh (POS, online, QR, recurring…), số tiền |
| Đặc điểm | Dữ liệu **tổng hợp (synthetic)** do ban tổ chức cung cấp, không phải dữ liệu thật của khách hàng |

**Xử lý dữ liệu**
- Kiểm tra chất lượng: không có giá trị thiếu cần điền; đối chiếu bảng tháng với toàn bộ file giao dịch.
- Tách tập khách hàng: **908 khách hàng đủ 12 tháng** được đưa vào mô hình; **91 khách hàng chỉ có 1–2 tháng** được xếp nhóm riêng bằng quy tắc, vì không đủ lịch sử để kết luận.
- Vì thu nhập và hạn mức là dữ liệu tổng hợp, phân tích chỉ so sánh **các tỷ lệ** (chi tiêu/thu nhập, mức sử dụng tín dụng, tỷ trọng chi tiêu thiết yếu…), không so sánh số tuyệt đối.

---

## 2. Pattern trong dữ liệu

Nhìn trung bình, khách hàng có vẻ ổn: 72,5% số tháng ở trạng thái Stable và điểm gắn kết trung bình 78/100. Đi sâu hơn thì thấy sáu pattern sau.

### 2.1 Tháng 12 là đỉnh chi tiêu, do mua *nhiều lần hơn* chứ không phải mua *đắt hơn*
- Chi tiêu tháng 12 đạt **488 tỷ VND (15% cả năm)**, gấp 2,8 lần tháng 2 (174 tỷ).
- Số giao dịch tăng 2,9 lần, trong khi giá trị trung bình mỗi giao dịch còn **giảm 2,6%**. Cùng một nhóm khách hàng, cơ cấu ngành hàng gần như không đổi.
- Tỷ lệ chi tiêu/thu nhập tháng 12 lên **1,25**, tức khách hàng tiêu nhiều hơn thu nhập, so với 0,45–0,79 ở các tháng khác.

### 2.2 Một biến quyết định sức khỏe tài chính: chi tiêu so với thu nhập
- Riêng tỷ lệ chi tiêu/thu nhập đã giải thích **81% điểm sức khỏe tài chính** (ρ = −0,95).
- Có một ngưỡng rõ rệt: tháng khó khăn gần như không xuất hiện khi chi tiêu dưới khoảng 70% thu nhập, nhưng chiếm **93%** khi chi tiêu trên 87% thu nhập.
- **87% khách hàng** có ít nhất một tháng điểm dưới 60; riêng tháng 12 có **84%** khách hàng dưới 60.
- Nghề nghiệp, tỉnh thành và tuổi chỉ làm điểm lệch 1–6 điểm, và cũng chỉ lệch vì khác nhau về tỷ lệ chi tiêu/thu nhập. Trong khi đó, riêng biến này làm điểm chênh khoảng 25 điểm giữa các nhóm ngũ phân vị.

### 2.3 Tháng khủng hoảng do chi tiêu không thiết yếu, không phải nhu yếu phẩm
- Trong các tháng khủng hoảng, chi tiêu không thiết yếu chiếm **72%** tổng chi (so với 41% ở tháng khỏe mạnh) và bằng **1,54 lần thu nhập** (so với 0,11 lần).
- Du lịch tăng nhiều nhất (+22,2 điểm %), tiếp theo là mua sắm online (+11 điểm %).

### 2.4 Kênh số được dùng rộng nhưng chưa sâu, và khoảng cách là do tuổi chứ không phải vùng miền
- 925/999 khách hàng dùng cả 5 kênh, nhưng POS vẫn chiếm 59% số giao dịch và online chỉ chiếm **21% chi tiêu**.
- Tỷ trọng online là biến tác động mạnh nhất đến điểm gắn kết: từ 71,3 lên 83,9 qua các nhóm ngũ phân vị (ρ = +0,83).
- Tỷ trọng giao dịch số giảm dần theo tuổi, từ 43,9% xuống 39,1% (ρ = −0,52, p < 0,001). Không tỉnh nào thấp hơn mức chung một cách có ý nghĩa thống kê (p ≥ 0,31).

### 2.5 Xăng xe được quẹt nhiều nhất, siêu thị chiếm nhiều tiền nhất
- Xăng & di chuyển dẫn đầu về số giao dịch (188.029 giao dịch, 10,2%).
- Siêu thị & tạp hóa dẫn đầu về chi tiêu (513,8 tỷ VND, 15,8%), với giá trị mỗi giao dịch lớn gấp 1,84 lần xăng xe.
- Tần suất tạo gắn kết, còn quy mô giỏ hàng tạo giá trị.

### 2.6 Gắn kết và sức khỏe tài chính đi ngược nhau
- **Trung thành nhưng căng thẳng:** 106 khách hàng (10,6%) tạo ra 16,5% chi tiêu, thực hiện 240 giao dịch/tháng (so với 161) và có tỷ lệ chi tiêu/thu nhập 0,79 (so với 0,69). Nhóm này trẻ hơn (tuổi trung bình 39), và áp lực đến từ rất nhiều khoản mua online nhỏ.
- **Khỏe mạnh nhưng đang rời xa:** 85 khách hàng (8,5%) có điểm sức khỏe tốt (71,5) nhưng ít giao dịch hơn 63%. Nhóm này lớn tuổi hơn (59% từ 55 tuổi trở lên). Đây là bài toán *share of wallet*, không phải rủi ro tín dụng.

---

## 3. Phân khúc khách hàng

Dự án dùng K-Means (k = 4) trên 8 biến hành vi, gồm hai khối:
- **Tài chính:** chi tiêu/thu nhập, mức sử dụng tín dụng, tỷ trọng chi thiết yếu, độ biến động chi tiêu.
- **Gắn kết:** số giao dịch, số ngày hoạt động, tỷ trọng online, recency.

Các biến được winsorize, log, chuẩn hóa z-score rồi PCA theo từng khối. Thêm 1 nhóm quy tắc cho khách hàng mới.

- Mô hình ổn định: ARI = 1,00 qua 10 seed và trung bình 0,97 qua 50 lần bootstrap.
- Biến nhân khẩu học **không** được đưa vào mô hình, chỉ dùng để mô tả.
- Hai điểm có sẵn (sức khỏe, gắn kết) được giữ làm thước đo kiểm chứng độc lập: chỉ số tự xây khớp với chúng ở mức r = 0,89 và r = 0,94.

| Phân khúc | Khách hàng | % chi tiêu | Đặc điểm chính | Ưu tiên hỗ trợ |
| --- | --- | --- | --- | --- |
| **Stretched but Highly Engaged** | 226 (22,6%) | 26,8% | Chi tiêu 81% thu nhập, 36% số tháng dưới 60, giao dịch 204 lần/tháng | 1 – Ngân sách + cảnh báo chi tiêu |
| **Low Engagement & Financially Vulnerable** | 199 (19,9%) | 8,5% | Chỉ 63 giao dịch/tháng, sức khỏe dưới trung bình | 2 – Giáo dục tài chính + khuyến khích dùng kênh số |
| **New / Low-history** | 91 (9,1%) | 0,4% | Chỉ có 1–2 tháng dữ liệu, chưa đủ để kết luận về thói quen | 3 – Onboarding, kích hoạt từ tháng thứ hai |
| **Emerging Digital Customers** | 184 (18,4%) | 31,9% | Trẻ nhất (TB 36 tuổi), gắn kết cao nhất, 211 giao dịch/tháng | 4 – Mục tiêu tiết kiệm + nhắc kế hoạch cuối năm |
| **Financially Healthy Core** | 299 (29,9%) | 32,4% | Chi tiêu 59% thu nhập, sức khỏe tốt nhất (69,3) | 5 – Ưu đãi, sản phẩm tiết kiệm |

---

## 4. Recommendation

Có sáu công cụ hỗ trợ. Mỗi công cụ gắn với một quy tắc chọn khách hàng có thể chạy lại hằng tháng, và được xếp hạng theo **độ phủ** (số khách hàng tiếp cận được) và **độ mạnh của driver** (|ρ| giữa hành vi mà công cụ tác động và kết quả cần cải thiện).

| # | Công cụ | Nhóm mục tiêu | Khách hàng | Bằng chứng từ dữ liệu | \|ρ\| |
| --- | --- | --- | --- | --- | --- |
| 1 | **Nhắc lập kế hoạch tài chính**: nhắc lập ngân sách cuối năm vào tháng 10–11, kiểm tra giữa tháng 12 | Chi tiêu vượt thu nhập trong tháng 12 | 687 (68,8%) | Chi tiêu/thu nhập tháng 12 là 1,25 (các tháng khác 0,45–0,79); 84,1% khách hàng dưới 60 điểm | 0,82 |
| 2 | **Cảnh báo chi tiêu** theo thời gian thực khi mức sử dụng tín dụng vượt 30% (khách hàng tự chỉnh được ngưỡng) | Mức sử dụng tín dụng ≥ 30% trong ít nhất 1 tháng | 592 (59,3%) | 96,9% số tháng vượt ngưỡng có điểm dưới 60, so với 14,1% ở các tháng còn lại | 0,87 |
| 3 | **Công cụ ngân sách**: dự báo chi tiêu cuối tháng so với thu nhập và gợi ý kế hoạch theo ngành hàng | Chi tiêu vượt thu nhập trong ít nhất 1 tháng ngoài tháng 12 | 489 (48,9%) | 99,3% số tháng chi tiêu vượt thu nhập có điểm dưới 60, so với 11,5% | 0,95 |
| 4 | **Khuyến khích dùng kênh số**: hỗ trợ cài QR/app và thanh toán tự động cho khách hàng 55+, hành trình kích hoạt cho khách hàng mới | Khách hàng 55+ có tỷ trọng giao dịch số dưới 40% (203) + nhóm New (91) | 294 (29,4%) | Điểm gắn kết tăng từ 71,3 lên 83,9 theo tỷ trọng online | 0,83 |
| 5 | **Nội dung giáo dục tài chính** ngắn: "cần hay muốn", lên kế hoạch cho khoản chi lớn như du lịch | Có ít nhất 1 tháng khủng hoảng (điểm dưới 40) | 70 (7,0%) | Tháng khủng hoảng có 71,6% chi tiêu không thiết yếu, so với 40,5% ở tháng khỏe mạnh | 0,93 |
| 6 | **Gợi ý sản phẩm phù hợp**: mục tiêu tiết kiệm, hoàn tiền cho xăng xe và siêu thị | Khỏe mạnh nhưng ít gắn kết | 85 (8,5%) | Sức khỏe TB 72,1 nhưng ít giao dịch hơn 63% | 0,50 |

- Công cụ 1–3 tạo thành một hành trình kiểm soát chi tiêu xoay quanh driver mạnh nhất: nhắc trước mùa cao điểm, cảnh báo khi vượt ngưỡng, và công cụ ngân sách để duy trì thói quen.
- Kế hoạch tiếp cận **908/999 khách hàng (90,9%)**, và phân khúc nào cũng có ít nhất một công cụ.

**Nguyên tắc bảo vệ khách hàng**
- Điểm sức khỏe tài chính, các phân khúc và mọi cờ chọn khách hàng **chỉ dùng để đề xuất hỗ trợ tùy chọn**. Chúng không bao giờ được dùng để từ chối tín dụng, giảm hạn mức hay khóa tài khoản.
- File đầu ra `t5_customer_offers.csv` chỉ chứa các cột `offer_*`. Notebook có bước kiểm tra tự động rằng không có cột nào liên quan đến quyết định tín dụng.
- Khách hàng có thể từ chối mọi công cụ. Tỷ lệ được đề xuất giữa nam và nữ tương đương nhau. Chênh lệch theo tuổi chỉ đến từ mục đích của từng quy tắc (ví dụ khuyến khích dùng kênh số nhắm vào nhóm 55+).

**Lộ trình triển khai**
- Tuần 1–2: chuẩn bị baseline KPI và quy tắc opt-out.
- Tuần 3–6: triển khai công cụ ngân sách, cảnh báo, giáo dục, nhắc kế hoạch, khuyến khích kênh số và gợi ý sản phẩm.
- Tuần 7–10: đo tỷ lệ sử dụng, mức phục hồi sức khỏe tài chính và tình trạng "mệt mỏi vì cảnh báo", từ đó quyết định mở rộng, điều chỉnh hay tạm dừng.

---

## 5. Hạn chế và hướng phát triển

- **Dữ liệu là một dải liên tục:** silhouette chỉ ở mức vừa phải (0,23–0,30) và HDBSCAN không tìm thấy khoảng trống mật độ rõ ràng. Các phân khúc là cách chia hữu ích, không phải các nhóm tách biệt tự nhiên.
- **Trung bình 12 tháng làm mất tính mùa vụ** (ví dụ đỉnh tháng 12). Bước tiếp theo là theo dõi khách hàng dịch chuyển giữa các phân khúc theo từng tháng.
- **Vòng lặp định nghĩa:** điểm sức khỏe có sẵn nhiều khả năng được tính từ chính các tỷ lệ đã dùng, nên mức khớp giữa phân khúc và điểm một phần là do cách xây dựng.
- **Hướng phát triển:** thử GMM (gán mềm) để có xác suất cho khách hàng ở ranh giới, phân khúc lại mỗi quý, và A/B test từng công cụ.

---

## Cấu trúc repository

| Tệp | Nội dung |
| --- | --- |
| `Fintech Company Analysis.pptx` | Slide trình bày kết quả phân tích (nên xem đầu tiên) |
| `Visualize_fixed.ipynb` | EDA, kiểm tra chất lượng dữ liệu, các pattern ở mục 2 |
| `Customer_segment_v5.ipynb` | Feature engineering, phân khúc K-Means, profile từng nhóm (mục 3) |
| `Task5.ipynb` | Quy tắc chọn khách hàng, xếp hạng 6 công cụ, kiểm tra nguyên tắc bảo vệ (mục 4) |
| `BI10_ROUND01.pdf` | Đề bài của vòng thi |
| `BI10_ROUND01_DATASET.zip` | Dữ liệu đầu vào và từ điển dữ liệu (lưu bằng Git LFS) |


## Chạy lại phân tích

```bash
git lfs install
git clone https://github.com/AIVIETNAM-AIO-thuhuynhtran05/ITB-Customer-Financial-Health-Analysis.git
cd ITB-Customer-Financial-Health-Analysis
python -m venv .venv
# kích hoạt môi trường ảo, sau đó:
python -m pip install jupyter pandas numpy matplotlib scipy scikit-learn pyarrow
jupyter lab
```

1. Giải nén `BI10_ROUND01_DATASET.zip` thành thư mục `BI10_ROUND01_DATASET/`, đặt cùng cấp với các notebook.
2. Chạy lần lượt `Visualize_fixed.ipynb` → `Customer_segment_v5.ipynb` → `Task5.ipynb`. Các thư mục `data_cache_fixed/` và `outputs_*` sẽ được tạo tự động.
