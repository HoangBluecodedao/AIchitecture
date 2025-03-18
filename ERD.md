# Đặc tả Sơ Đồ ERD AIchitecture
![ERD3 drawio](https://github.com/user-attachments/assets/7b5ce1ec-63ad-4233-8609-8f29d9e17c6e)


## 1. Các Entity Chính và Mối Quan Hệ

### Entity User
- Primary Key: `user_id`
- Thuộc tính: username, email, password_hash, session_id, plan_type, location
- Theo dõi thời gian: created_at, updated_at
- Mối quan hệ: Một user có thể tạo nhiều sessions (1:N)

### Entity Tokens
- Primary Key: `token_id`
- Thuộc tính: user_id (Foreign Key), token_value, token_type
- Thông tin hết hạn: expires_at
- Theo dõi thời gian: created_at, updated_at

### Entity Sessions
- Primary Key: `session_id`
- Thuộc tính: status (ENUM), input_id, output_id, version
- Mối quan hệ: N:1 với User, 1:1 với User Input và Model Output
- Theo dõi thời gian: created_at, ended_at

### Entity Plans
- Primary Key: `plan_type`
- Thuộc tính: price, token_amount, validity_days
- Theo dõi thời gian: created_at

### Entity User Input
- Primary Key: `input_id`
- Thuộc tính: input_type, status_code, nlp_id, image_processing_id
- Theo dõi thời gian: created_at

### Các Entity Xử Lý Dữ Liệu
- **NLP Processing**: Xử lý đầu vào dạng văn bản
- **Image Processing**: Xử lý ban đầu cho hình ảnh
- **Image Classification**: Phân loại hình ảnh
- **Image Segmentation**: Phân đoạn hình ảnh
- **Image Feature Selection**: Trích xuất đặc trưng hình ảnh
- **Model Output**: Kết quả đầu ra của mô hình

## 2. Đặc Điểm Quan Trọng của ERD

- **Theo Dõi Token**: Hệ thống giám sát số lượng token được sử dụng trong mỗi phiên làm việc
- **Lưu Vết Thời Gian**: Mọi thao tác đều được ghi nhận thời gian tạo/cập nhật
- **Phân Loại Đầu Vào**: Hệ thống phân biệt rõ giữa đầu vào văn bản và hình ảnh

