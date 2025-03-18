# AIchitecture: Hệ thống Thiết kế Kiến trúc Thông minh

## Tổng quan Dự án
AIchitecture là một dự án sáng tạo được phát triển bởi nhóm năm sinh viên từ khóa K20 của Đại học FPT cho cuộc thi Hackaithon. Hệ thống ứng dụng trí tuệ nhân tạo để hỗ trợ người dùng trong việc tạo ra và hình dung thiết kế kiến trúc ngoại thất và nội thất dựa trên mô tả văn bản hoặc hình ảnh đầu vào.

## Link ý tưởng: 
[Link presentation:]([url](https://www.canva.com/design/DAGek9QegFs/MLxSBl8W7dvX2v47SAZZhg/edit?utm_content=DAGek9QegFs&utm_campaign=designshare&utm_medium=link2&utm_source=sharebutton)) 

## Thành viên Nhóm

| Mã số SV | Họ và tên | Vai trò & Trách nhiệm |
|------------|------|-------------------------|
| DE200402 | Hồ Tất Bảo Hoàng | **Nhóm trưởng**<br>• Tổ chức các cuộc họp nhóm, thảo luận ý tưởng<br>• Thiết kế bản kiến trúc hệ thống<br>• Lập sơ đồ luồng dữ liệu (data flow chart)<br>• Hỗ trợ các thành viên và đảm bảo chất lượng |
| DE200459 | Nguyễn Đình Hoàng | • Thiết kế Figma AI page<br>• Phát triển prototype<br>• Xây dựng bản user flow |
| DE200048 | Dương Tiến Thành | • Thiết kế Figma AI page<br>• Tạo sơ đồ lớp (class diagram)<br>• Làm slide thuyết trình |
| DE200389 | Đinh Quốc Trường | • Thiết kế Figma trang landing page<br>• Phát triển prototype<br>• Làm slide thuyết trình |
| DE200165 | Lê Quảng Hà | • Thiết kế Figma trang landing page<br>• Phát triển prototype<br>• Làm slide thuyết trình dự án |

## Kiến trúc Hệ thống

### 1. Hệ thống Gợi ý Thiết kế Ngoại thất
Hệ thống xử lý đầu vào của người dùng thông qua quy trình sau:

1. **Xử lý Đầu vào**
   - Nhận input dạng văn bản hoặc giọng nói về phong cách ngoại thất mong muốn và ước tính chi phí.
   - Xử lý NLP qua GeminiAI

2. **Kiểm tra Cơ sở Dữ liệu**
   - Truy vấn cơ sở dữ liệu để tìm kết quả liên quan phù hợp với input
   - Trả về kết quả đã có nếu tìm thấy các kết quả phù hợp

3. **Tạo Nội dung (khi không có kết quả phù hợp)**
   - Tạo mô tả chi tiết về phong cách ngoại thất sử dụng GeminiAI
   - Tạo hình ảnh minh họa cho thiết kế ngoại thất sử dụng Stable Diffusion
   - Ước tính chi phí thực hiện dựa trên thông số thiết kế

### 2. Hệ thống Gợi ý Thiết kế Nội thất
Hệ thống xử lý hai loại đầu vào:

1. **Xử lý Dựa trên Văn bản**
   - Xử lý mô tả văn bản sử dụng khả năng NLP của GeminiAI
   - Tạo gợi ý thiết kế nội thất dựa trên phân tích văn bản

2. **Xử lý Dựa trên Hình ảnh**
   - Áp dụng quy trình xử lý hình ảnh phức tạp:
     - Phân loại hình ảnh sử dụng Amazon Rekognition
     - Phân đoạn hình ảnh qua Mask C-NN
     - Trích xuất đặc trưng thông qua kiến trúc ResNet
     - Xác minh tính tương thích giữa văn bản và hình ảnh sử dụng CLIP

3. **Tạo Kết quả**
   - Cung cấp gợi ý phong cách nội thất dựa trên dữ liệu đã xử lý
   - Đề xuất liên kết đến các sản phẩm liên quan có sẵn trên Amazon thông qua tích hợp API

## Quy trình Làm việc của Hệ thống

1. Xác thực và đăng nhập người dùng
2. Xác minh thông tin đăng nhập
3. Lựa chọn mô hình (thiết kế Ngoại thất hoặc Nội thất)
4. Khởi tạo session
5. Xác minh số lượng token khả dụng
6. Xác thực đầu vào trước khi xử lý mô hình
7. Tạo và trình bày kết quả

## Kết quả Hệ thống

### Mô hình Thiết kế Ngoại thất
- Mô tả chi tiết về phong cách
- Hình ảnh minh họa thiết kế
- Ước tính chi phí thực hiện

### Mô hình Thiết kế Nội thất
- Gợi ý thiết kế nội thất tùy chỉnh
- Đề xuất sản phẩm với liên kết mua hàng trực tiếp

## So Sánh Ưu Điểm của AIchitecture với Các Giải Pháp Hiện Có

| Tiêu chí | AIchitecture | Các giải pháp thiết kế truyền thống | Các nền tảng thiết kế trực tuyến hiện có |
|----------|--------------|-----------------------------------|----------------------------------------|
| **Thời gian phản hồi** | Phản hồi tức thì trong vài giây | Có thể mất từ vài ngày đến vài tuần tùy thuộc vào độ phức tạp của thiết kế | Thường mất vài giờ đến vài ngày để hoàn thiện |
| **Chi phí** | Phải chăng, tiết kiệm đáng kể so với thuê kiến trúc sư | Cao, đặc biệt với các kiến trúc sư chuyên nghiệp | Thường có chi phí trung bình, phụ thuộc vào độ phức tạp của dự án |
| **Tính cá nhân hóa** | Cao, dựa trên yêu cầu chi tiết từ người dùng qua văn bản hoặc hình ảnh | Phụ thuộc vào kỹ năng và hiểu biết của kiến trúc sư về nhu cầu khách hàng | Hạn chế, thường sử dụng các mẫu có sẵn với khả năng tùy chỉnh thấp |
| **Công nghệ AI tích hợp** | Đa dạng (GeminiAI, Stable Diffusion, Amazon Rekognition, Mask C-NN, ResNet, CLIP) | Không có hoặc rất hạn chế | Thường chỉ sử dụng một số công nghệ cơ bản |
| **Khả năng xử lý đa dạng đầu vào** | Xử lý cả văn bản, giọng nói và hình ảnh | Chủ yếu dựa trên trao đổi trực tiếp và bản vẽ | Thường chỉ tiếp nhận một loại đầu vào nhất định |
| **Kết nối thương mại điện tử** | Tích hợp trực tiếp với API Amazon để gợi ý sản phẩm phù hợp | Không có | Hiếm khi có tích hợp hoặc chỉ liên kết với một số nhà cung cấp giới hạn |
| **Khả năng mở rộng** | Cao, có thể dễ dàng tích hợp thêm công nghệ và đối tác | Hạn chế do phụ thuộc vào nguồn nhân lực | Thường phụ thuộc vào nền tảng cụ thể và khó mở rộng |
| **Kiến trúc hạ tầng** | Serverless trên AWS, tiết kiệm chi phí vận hành | Không áp dụng | Thường sử dụng hạ tầng truyền thống với chi phí cao |
| **Khả năng học hỏi và cải thiện** | Liên tục cải thiện dựa trên dữ liệu người dùng | Phụ thuộc vào kinh nghiệm cá nhân của kiến trúc sư | Cập nhật không thường xuyên và phụ thuộc vào nhà phát triển |
| **Tốc độ làm việc** | Rất nhanh, đáp ứng tức thì | Chậm, phụ thuộc vào lịch trình của kiến trúc sư | Trung bình, phụ thuộc vào độ phức tạp của dự án |
| **Ước tính chi phí xây dựng** | Tự động dựa trên thiết kế được đề xuất | Thường do kiến trúc sư cung cấp sau khi hoàn thành thiết kế | Hiếm khi có tính năng này hoặc không chính xác |
| **Khả năng đáp ứng xu hướng thiết kế mới** | Liên tục cập nhật dựa trên dữ liệu mới | Phụ thuộc vào việc kiến trúc sư cập nhật kiến thức | Cập nhật không thường xuyên |
