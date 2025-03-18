# Đặc tả Kiến trúc System Architecture AIchitecture

![blu drawio (1)](https://github.com/user-attachments/assets/374162b3-11d6-40a9-bdde-6af9d96382f5)

## I. Tổng quan

AIchitecture được xây dựng trên nền tảng Amazon Web Services với kiến trúc event-driven và serverless.

## II. Các Thành phần Cốt lõi

### A. Tầng Frontend
- **Amazon S3**: Lưu trữ các static resource của web
- **Amazon CloudFront**: CDN phân phối nội dung tới người dùng với độ trễ thấp
- **AWS Amplify**: Frontend tích hợp với các dịch vụ AWS backend

### B. Tầng Authentication
- **Amazon Cognito**: Quản lý danh tính người dùng và cung cấp token xác thực
- **AWS Lambda Authenticator**: Xử lý logic xác thực người dùng
- **External Identity Providers**: Hỗ trợ đăng nhập qua các nhà cung cấp danh tính bên ngoài

### C. Tầng API
- **Amazon API Gateway (HTTP)**: Điểm cuối HTTP cho các yêu cầu từ frontend
- **Amazon API Gateway (WebSocket)**: Kết nối thời gian thực giữa backend và frontend
- **Amazon EventBridge**: Điều phối giao tiếp không đồng bộ giữa các dịch vụ trong hệ thống

### D. Tầng AI
1. **Mô hình Thiết kế Ngoại thất**:
   - **AWS Lambda Function**: Tiếp nhận và định tuyến đầu vào người dùng
   - **Amazon SageMaker Endpoint**: Triển khai GeminiAI cho xử lý ngôn ngữ tự nhiên
   - **WhisperAI trên SageMaker**: Chuyển đổi giọng nói thành văn bản
   - **Amazon Bedrock**: Điểm truy cập vào các mô hình AI như GeminiAI
   - **Amazon EC2 GPU Instances**: Chạy Stable Diffusion để tạo sinh hình ảnh

2. **Mô hình Thiết kế Nội thất**:
   - **AWS Step Functions**: Điều phối luồng công việc 
   - **Amazon Rekognition**: Phân loại hình ảnh và xác thực nội dung
   - **SageMaker (Mask R-CNN)**: Phân đoạn hình ảnh, nhận diện đối tượng
   - **SageMaker (ResNet)**: Trích xuất đặc trưng từ hình ảnh
   - **SageMaker (CLIP)**: Đảm bảo sự tương thích giữa văn bản và hình ảnh
   - **Lambda Amazon API Integration**: Tìm kiếm sản phẩm trên sàn Amazon

### E. Tầng Dữ liệu
- **Amazon S3**: Lưu trữ hình ảnh và tệp tin
- **Amazon PostgreSQL (RDS)**: Lưu trữ image dưới dạng vector
- **MongoDB**: Lưu trữ session chat
- **Amazon ElastiCache**: cache session chat tối ưu hóa hiệu suất
- **Lambda Scheduler**: Quản lý việc đồng bộ dữ liệu từ bộ nhớ đệm vào cơ sở dữ liệu

### F. Tầng Bảo mật và Giám sát
- **AWS Identity and Access Management (IAM)**: Kiểm soát quyền truy cập vào tài nguyên AWS
- **AWS Web Application Firewall (WAF)**: Bảo vệ ứng dụng web khỏi các mối đe dọa phổ biến
- **Amazon CloudWatch**: Giám sát hiệu suất và hoạt động của hệ thống
- **AWS X-Ray**: Phân tích và gỡ lỗi các ứng dụng phân tán
- **Google Perspective AI**: Xác thực nội dung văn bản đầu ra

## III. Luồng Dữ liệu và Quy trình Xử lý

### A. Quy trình Đăng nhập và Xác thực
1. Người dùng truy cập ứng dụng thông qua CloudFront từ giao diện web được lưu trữ trên S3
2. Yêu cầu đăng nhập được chuyển đến API Gateway HTTP
3. Lambda Authenticator xử lý yêu cầu và tương tác với Cognito
4. Cognito xác thực người dùng thông qua cơ chế nội bộ hoặc nhà cung cấp danh tính bên ngoài
5. Token xác thực được trả về cho người dùng

### B. Quy trình Thiết kế Ngoại thất
1. Người dùng gửi yêu cầu thiết kế ngoại thất (văn bản hoặc giọng nói)
2. Lambda tiếp nhận đầu vào:
   - Nếu là văn bản: Chuyển trực tiếp đến SageMaker Endpoint chạy GeminiAI
   - Nếu là giọng nói: Xử lý qua WhisperAI để chuyển đổi thành văn bản
3. Python SDK truy vấn cơ sở dữ liệu PostgreSQL tìm kiếm thiết kế tương tự
4. Nếu tìm thấy: Trả về kết quả có sẵn
5. Nếu không tìm thấy:
   - Khởi tạo phiên làm việc mới
   - Lambda gọi API đến Amazon Bedrock để truy cập GeminiAI tạo mô tả văn bản
   - Song song đó, Lambda gọi đến EC2 GPU chạy Stable Diffusion để tạo hình ảnh
6. Kết quả được xác thực và trả về cho người dùng

### C. Quy trình Thiết kế Nội thất
1. Người dùng gửi yêu cầu thiết kế nội thất (hình ảnh và/hoặc văn bản)
2. Lambda tiếp nhận đầu vào:
   - Nếu là hình ảnh: Tải lên S3 và kích hoạt Step Functions
   - Step Functions điều phối quy trình xử lý hình ảnh:
     - Lambda gọi Amazon Rekognition để phân loại hình ảnh
     - SageMaker triển khai Mask R-CNN để phân đoạn hình ảnh
     - SageMaker triển khai ResNet để trích xuất đặc trưng
     - Kết quả trung gian được lưu trữ lại trên S3
   - Văn bản đầu vào được xử lý qua SageMaker Endpoint chạy GeminiAI
   - Lambda gọi SageMaker triển khai CLIP để đảm bảo sự tương thích giữa hình ảnh và văn bản
   - Lambda thực hiện gọi API Amazon tìm kiếm sản phẩm dựa trên kết quả phân tích
3. Kết quả được xác thực và trả về cho người dùng

### D. Quản lý Phiên Người dùng
1. Phiên trò chuyện được lưu trữ tạm thời trong ElastiCache để tối ưu hiệu suất
2. Lambda Scheduler định kỳ đồng bộ dữ liệu:
   - Văn bản từ bộ nhớ đệm được lưu vào MongoDB
   - Hình ảnh được lưu vào PostgreSQL
3. EventBridge đảm bảo giao tiếp không đồng bộ giữa các dịch vụ

### E. Xác thực Đầu ra và Phản hồi
1. Lambda gọi Amazon Rekognition để xác thực hình ảnh đầu ra
2. Lambda gọi Google Perspective AI để xác thực nội dung văn bản
3. Sau khi xác thực, Lambda kết nối với PostgreSQL để lưu trữ kết quả
4. API Gateway WebSocket đẩy kết quả lên frontend người dùng

## IV. Đặc điểm Kỹ thuật Nổi bật

1. **Kiến trúc Serverless**: Giảm thiểu chi phí vận hành và tự động mở rộng theo nhu cầu
2. **Kiến trúc Event-driven**: Tăng khả năng phản hồi và độ tin cậy của hệ thống
3. **Xử lý Multimodal Processing**: Hỗ trợ đầu vào đa dạng (văn bản, giọng nói, hình ảnh)
4. **Tích hợp Công nghệ AI tiên tiến**: Kết hợp nhiều mô hình AI chuyên biệt cho từng tác vụ
5. **Hệ thống Bộ nhớ đệm Phân tầng**: Tối ưu hiệu suất và thời gian phản hồi
6. **Quản lý Phiên thông minh**: Tiết kiệm tài nguyên bằng cách lưu trữ có chọn lọc
7. **Giám sát và Phân tích toàn diện**: Cho phép phát hiện và xử lý sự cố kịp thời
8. **Tích hợp Thương mại điện tử**: Liên kết trực tiếp với API Amazon để gợi ý sản phẩm
