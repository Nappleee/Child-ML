

# Project Machine Learning 
# Child Mind Institute — Problematic Internet Use

# Contributors
Nguyễn Hồ Bắc - 22028147

Nguyễn Đình Tuấn Anh - 22028279

Nguyễn Quang Cảnh - 22028200

2425I_INT3405E_55 UET

# Giới thiệu
* Đây là bài thi của cuộc thi Child Mind Institute — Problematic Internet Use do Kaggle tổ chức.
* Cụ thể: Các phương pháp hiện tại để đo lường việc sử dụng internet có vấn đề ở trẻ em và thanh thiếu niên thường phức tạp và đòi hỏi phải có các đánh giá chuyên môn. Điều này tạo ra các rào cản về khả năng tiếp cận, văn hóa và ngôn ngữ cho nhiều gia đình. Do những hạn chế này, việc sử dụng internet có vấn đề thường không được đo lường trực tiếp mà thay vào đó lại liên quan đến các vấn đề như trầm cảm và lo âu ở thanh thiếu niên. Ngược lại, các biện pháp về thể chất và thể lực cực kỳ dễ tiếp cận và có sẵn rộng rãi với sự can thiệp tối thiểu hoặc chuyên môn lâm sàng. Những thay đổi về thói quen thể chất, chẳng hạn như tư thế kém hơn, chế độ ăn uống không điều độ và giảm hoạt động thể chất, là phổ biến ở những người sử dụng công nghệ quá mức. Chúng tôi đề xuất sử dụng các chỉ số thể lực dễ dàng có được này làm đại diện để xác định việc sử dụng internet có vấn đề, đặc biệt là trong bối cảnh thiếu chuyên môn lâm sàng hoặc công cụ đánh giá phù hợp. Cuộc thi này thách thức bạn phát triển một mô hình dự đoán có khả năng phân tích dữ liệu hoạt động thể chất của trẻ em để phát hiện sớm các chỉ số về việc sử dụng internet và công nghệ có vấn đề. Điều này sẽ cho phép can thiệp kịp thời nhằm thúc đẩy thói quen kỹ thuật số lành mạnh hơn. Công việc của bạn sẽ góp phần vào một tương lai lành mạnh hơn, hạnh phúc hơn, nơi trẻ em được trang bị tốt hơn để điều hướng bối cảnh kỹ thuật số một cách có trách nhiệm. 
* Bài tập lớn môn học Học máy - Machine Learning - 2425I_INT3405E_55.

# Mô Tả Code
## 1. Thư viện được sử dụng
Chúng tôi sử dụng các thư viện Python phổ biến để xử lý dữ liệu, trực quan hóa và xây dựng mô hình học máy:

* numpy & pandas: Xử lý dữ liệu và các phép tính số học.
* scikit-learn: Đánh giá mô hình và xây dựng đặc trưng.
* xgboost: Framework boosting gradient để giải quyết bài toán hồi quy và phân loại.
* optuna: Tối ưu siêu tham số.
* eli5: Hiển thị tầm quan trọng của đặc trưng.
* plotly, seaborn, matplotlib: Phân tích và vẽ biểu đồ trực quan.
## 2. Tiền xử lý dữ liệu
* Nạp dữ liệu: Đọc các file dữ liệu training, testing và từ điển dữ liệu.
* Xử lý giá trị thiếu: Các cột không phải kiểu số được điền giá trị mặc định là 0. Các mùa trong dữ liệu phân loại được mã hóa thành số nguyên (Spring: 1, Summer: 2,...).
* Lựa chọn đặc trưng: Các cột có chứa từ khóa PCIAT được phân tích để tìm mối tương quan với điểm tổng. Các đặc trưng có trên 50% giá trị bị thiếu sẽ bị loại bỏ.
## 3. Tạo đặc trưng
* Scaled Impact Index (SII): Được tạo từ cột PCIAT-PCIAT_Total dựa trên ngưỡng cụ thể:
0–30: SII = 0

31–49: SII = 1

50–79: SII = 2

80–100: SII = 3

* Điều này chuyển bài toán thành một bài toán phân loại với 4 lớp.
* Tầm quan trọng của đặc trưng: Loại bỏ các đặc trưng có ảnh hưởng thấp đến mục tiêu.
## 4. Trực quan hóa dữ liệu
* Ma trận tương quan: Hiển thị mối quan hệ giữa các đặc trưng và mục tiêu.
* Boxplot & Histogram: Khám phá phân phối dữ liệu và tìm ra giá trị bất thường.
* Tầm quan trọng của đặc trưng: Tính toán thông qua phương pháp tích hợp sẵn của XGBoost và tầm quan trọng hoán vị.
## 5. Xây dựng mô hình
* Quadratic Weighted Kappa (QWK): Hàm đánh giá tùy chỉnh để đo lường độ chính xác của dự đoán.
* Tối ưu siêu tham số: optuna được sử dụng để tối ưu các tham số của XGBoost như max_depth, n_estimators, learning_rate,...
* Mục tiêu: Tối đa hóa điểm QWK.
* Cross-Validation: StratifiedKFold đảm bảo sự phân bố đồng đều giữa các lớp trong các tập chia.
## 6. Huấn luyện và dự đoán
* Huấn luyện: Mô hình XGBoost Regressor được huấn luyện trên tập đặc trưng đã chọn.
* Dự đoán: Dự đoán trên tập test được chuyển đổi thành các danh mục SII và lưu vào file submission.csv.
## 7. Kết quả chính
* Tầm quan trọng của đặc trưng: Hiển thị mức độ đóng góp của từng đặc trưng đến hiệu suất mô hình.
* Hiệu suất mô hình: Điểm QWK trung bình được tính để đánh giá độ chính xác.

## 8. Hướng dẫn chạy
Cài đặt các thư viện cần thiết:
"pip install numpy pandas scikit-learn xgboost optuna plotly seaborn eli5"

Đặt dữ liệu vào thư mục chỉ định (../input/child-mind-institute-problematic-internet-use/).
Chạy script để tạo file dự đoán.

# Kết quả
* Điểm số cuối: 0.435

# Link video báo cáo
* https://drive.google.com/file/d/1b5fghB9R47xVxmvuCkor6LwCe8UM85jc/view?usp=drive_link









