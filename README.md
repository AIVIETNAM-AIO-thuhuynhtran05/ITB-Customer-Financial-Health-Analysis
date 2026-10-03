# ITB Customer Financial Health Analysis

Project BI10 Round 1 phân tích tình hình tài chính và mức độ tương tác của khách hàng tại Việt Nam. Dự án sử dụng bộ dữ liệu tổng hợp năm 2025, kết hợp phân tích khám phá, phân khúc khách hàng và đề xuất hành động kinh doanh có trách nhiệm.

## Nội dung dự án

| Tệp | Mô tả |
| --- | --- |
| `Visualize_fixed.ipynb` | Notebook phân tích khám phá, kiểm tra chất lượng dữ liệu và tạo các bảng/biểu đồ đầu ra. Đây là phiên bản đã chỉnh sửa; nên dùng notebook này thay cho `Visualize.ipynb`. |
| `Visualize.ipynb` | Phiên bản trực quan hóa trước khi chỉnh sửa, được giữ lại để tham khảo. |
| `Customer_segment_v5.ipynb` | Xây dựng và đánh giá mô hình phân khúc khách hàng dựa trên sức khỏe tài chính, mức độ tương tác và nhịp giao dịch. |
| `Customer_segment_v4.ipynb` | Phiên bản phân khúc trước đó, được giữ lại để tham khảo. |
| `Task5.ipynb` | Phân tích kết quả phân khúc và xây dựng kế hoạch hành động, kèm các nguyên tắc bảo vệ khách hàng. |
| `BI10_ROUND01.pdf` | Tài liệu PDF của dự án/vòng thi. |
| `BI10_ROUND01_DATASET.zip` | Bộ dữ liệu đầu vào và từ điển dữ liệu. Tệp này được lưu trên Git LFS do vượt giới hạn kích thước tệp thông thường của GitHub. |

Các tệp Word (`.docx`) không được đưa lên repository.

## Dữ liệu

Giải nén `BI10_ROUND01_DATASET.zip` vào thư mục `BI10_ROUND01_DATASET` ở cùng cấp với các notebook. Sau khi giải nén, cấu trúc thư mục cần có dạng:

```text
project/
├── BI10_ROUND01_DATASET/
│   ├── consumer_financial_health_engagement_2025.csv
│   ├── consumer_transactions_2025.csv
│   ├── data_dictionary.xlsx
│   └── consumer_financial_health_case_study.md
├── Customer_segment_v5.ipynb
├── Task5.ipynb
└── Visualize_fixed.ipynb
```

Notebook xử lý dữ liệu có thể tạo thêm các thư mục `data_cache_fixed`, `outputs_fixed`, `outputs_task4` và `outputs_task5`. Các tệp sinh ra này được tạo trong quá trình chạy và không cần có sẵn để bắt đầu.

## Bắt đầu

Yêu cầu Python 3 và Jupyter Notebook/JupyterLab. Tạo môi trường ảo (khuyến nghị), sau đó cài các thư viện được notebook sử dụng:

```bash
python -m venv .venv
```

Kích hoạt môi trường ảo rồi chạy:

```bash
python -m pip install jupyter pandas numpy matplotlib scipy scikit-learn pyarrow
jupyter lab
```

Nếu tải repository bằng Git, hãy cài Git LFS trước khi clone để nhận được nội dung dataset:

```bash
git lfs install
git clone https://github.com/AIVIETNAM-AIO-thuhuynhtran05/ITB-Customer-Financial-Health-Analysis.git
```

## Thứ tự chạy notebook

1. Mở `Visualize_fixed.ipynb` để đọc dữ liệu nguồn, kiểm tra và tạo các bảng đầu ra cho phân tích khám phá.
2. Mở `Customer_segment_v5.ipynb` để tạo phân khúc khách hàng. Notebook này có thể sử dụng một số bảng do bước trực quan hóa tạo ra.
3. Mở `Task5.ipynb` để tạo đề xuất hành động dựa trên kết quả phân khúc.

Chạy notebook từ thư mục dự án sau khi đã giải nén dataset vào đúng vị trí. `Customer_segment_v4.ipynb` và `Visualize.ipynb` là các phiên bản trước, có thể dùng để đối chiếu.

## Lưu ý

- Dữ liệu trong bộ case study được mô tả là dữ liệu tổng hợp, không phải dữ liệu giao dịch thực của khách hàng.
- Các thư mục đầu ra và cache được tạo khi chạy notebook; kết quả có thể khác nếu thay đổi phiên bản thư viện hoặc dữ liệu.
- Git LFS cần được cài đặt và cấu hình để tải/làm việc với tệp dataset ZIP.
