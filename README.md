# ADY201m — Huy Dung: phân loại CARDIO và phân tích sai số

**Họ tên:** Huy Dung · **Mã phiên bản:** HD-2026

Gói bài tập trình bày lưu trữ dữ liệu SQL, phân tích khám phá, kiểm định thống kê và các mô hình phân loại theo đề ADY201m. Phương án SQLite thay IBM Cloud được sử dụng theo thông tin giảng viên đã chấp nhận. Hai notebook đã chạy cục bộ và lưu kết quả; chưa đăng gói này lên GitHub, chưa thực thi IBM Cloud.

## Thiết kế riêng của phiên bản

- 70% train / 30% test, phân tầng CARDIO, seed **2026**; lưu danh sách ID để tái lập.
- Tinh chỉnh **Random Forest** trên train với **5 fold**, chọn theo **F1**; lưới 4 cấu hình, 200 cây.
- So sánh đủ RidgeClassifier, RandomForest, GradientBoosting, AdaBoost, Bagging KNN, ExtraTrees và Stacking, kèm Dummy baseline.
- Bổ sung tỷ lệ theo nhóm tuổi với khoảng tin cậy Wilson 95%, ROC/precision–recall, reliability/Brier và sai số theo mã giới tính.
- SQLite chạy 13 truy vấn theo đề và một truy vấn tổng hợp tuổi–giới tính; kiểm tra dữ liệu sau khi mở lại tệp.

## Kết quả đã chạy

| Chỉ tiêu | Giá trị |
|---|---:|
| Số hồ sơ | 70.000 |
| CARDIO = 1 | 34.979 |
| Train / test | 49.000 / 21.000 |
| F1 CV train, 5 fold | 72.27% |
| Accuracy test, Random Forest tinh chỉnh | 73.43% |
| Precision / Recall test | 75.43% / 69.45% |
| F1 test | 72.32% |
| ROC-AUC test | 0.8010 |
| Brier score test | 0.1807 |

Mô hình cuối được chọn bằng F1 CV của train trong gia đình Random Forest, không chọn lại sau khi xem bảng test. Đánh giá giữa hai phiên bản có tập test khác nhau không cho phép khẳng định bản nào tốt hơn từ chênh lệch điểm đơn lẻ.

## Nội dung

| Tệp / thư mục | Nội dung |
|---|---|
| `Bao_cao_Huy_Dung.html` | Báo cáo tiếng Việt, tự chứa biểu đồ, mở trực tiếp bằng trình duyệt. |
| `01_SQL_Huy_Dung.ipynb` | Tạo hoặc mở SQLite, 13 truy vấn, tổng hợp bổ sung và xuất CSV. |
| `02_Analysis_Huy_Dung.ipynb` | EDA, IQR, kiểm định, OLS, 7 thuật toán và tinh chỉnh Random Forest. |
| Hai bản `.html` của notebook | Mã và kết quả để đọc khi chưa cài Jupyter. |
| `DOI_CHIEU_PHIEN_BAN.html` | Bảng khác biệt thiết kế và số lượng ID test giao nhau thực tế. |
| `data/` | CSV gốc, CSV xuất, CSV EDA, SQLite và kiểm tra nguồn. |
| `sql/` | Schema SQLite, truy vấn theo đề và truy vấn tuổi–giới tính. |
| `figures/` | 12 biểu đồ được tạo từ notebook. |
| `results/` | Kết quả SQL, thống kê, CV, dự đoán, mô hình và kiểm tra gói. |
| `MANIFEST.json` | Danh sách kích thước và SHA-256. |

## Tái lập

1. Giải nén toàn bộ gói, giữ cấu trúc thư mục.
2. Cài Python và các thư viện trong `requirements.txt` bằng `python -m pip install -r requirements.txt`.
3. Khởi động Jupyter từ thư mục gói bằng `python -m notebook`.
4. Chạy toàn bộ `01_SQL_Huy_Dung.ipynb`, sau đó `02_Analysis_Huy_Dung.ipynb` theo thứ tự từ trên xuống.

Notebook dùng thư mục làm việc chứa `data/` và hai hàm xử lý. SQLite được tạo nếu chưa có; lần chạy sau xác minh dữ liệu hiện có, không nhập thêm bản ghi trùng hoặc xóa bảng. AGE trong CSV gốc và SQLite vẫn tính bằng ngày; notebook phân tích đổi sang năm nguyên đúng một lần. CSV đã làm sạch chỉ dùng cho EDA, không thay dữ liệu gốc để huấn luyện.

Pipeline học cận IQR và StandardScaler trên train của từng fold. Stacking đặt tiền xử lý trong từng bộ học cơ sở để tránh rò rỉ các dự đoán ngoài fold. Ngưỡng phân loại giữ mặc định; reliability chỉ đánh giá xác suất, không phải bộ hiệu chỉnh đã huấn luyện.

## Phạm vi, nguồn và hỗ trợ

SQL và kiểm định đúng có thể cho cùng kết quả với bài dùng cùng dữ liệu, giả thuyết và thứ tự truy vấn. Sự khác biệt của HD-2026 nằm ở thiết kế chọn mô hình, tập test và phân tích bổ sung; thay đổi tên hoặc văn phong không được xem là khác biệt phương pháp.

Dữ liệu khôi phục từ PDF cardio_train_raw 649 trang; yêu cầu từ đề ADY201m 35 trang. Gói dữ liệu nguồn và một số hàm cơ bản dùng chung với phiên bản trước. Quá trình xây dựng có sự hỗ trợ của công cụ AI; người nộp cần kiểm tra, hiểu phương pháp và tuân thủ quy định khai báo hỗ trợ của học phần.

Kết quả phản ánh quan hệ quan sát; không chứng minh nguyên nhân bệnh. OLS/ANOVA/T-test tái hiện yêu cầu thống kê của đề. Mô hình chưa được kiểm định trên dữ liệu độc lập và chưa đủ cơ sở cho ứng dụng chẩn đoán.
