# Đặc tả user flow - AIchitecture

## Phase authentication

Phase authentication cung cấp hai điểm truy cập chính:

1. **Sign in**: 
   - Người dùng điều hướng đến Sign in page
   - Hệ thống xác thực thông tin đăng nhập
   - Nếu thành công, người dùng tiếp tục đến trang AI
   - Nếu không thành công, người dùng quay lại lựa chọn xác thực

2. **Log in** (Đăng ký mới):
   - Người dùng điều hướng đến Log in page
   - Hệ thống xử lý đăng ký
   - Nếu thành công, người dùng tiếp tục đến trang AI
   - Nếu không thành công, người dùng quay lại lựa chọn xác thực

## Phase Services

Sau khi xác thực thành công, người dùng vào trang AI nơi có hai dịch vụ chính:

### Exterior AI model
Khi chọn thiết kế ngoại thất, người dùng thực hiện theo trình tự sau:
1. Chọn loại đầu vào (văn bản hoặc âm thanh)
2. Nhập thông số ngân sách
3. Nhận đề xuất thiết kế được điều chỉnh theo thông số kỹ thuật
4. Xem xét thiết kế được đề xuất
5. Điểm quyết định: Lưu thiết kế hoặc yêu cầu thiết kế mới

### Interior AI model
Khi chọn thiết kế nội thất, người dùng thực hiện theo trình tự sau:
1. Tải lên ảnh tham khảo
2. Thêm thông tin mô tả
3. Chọn phong cách ưa thích
4. Nhận:
   - Đề xuất thiết kế
   - Đề xuất sản phẩm phù hợp với thiết kế
5. Điểm quyết định: Lưu thiết kế hoặc yêu cầu thiết kế mới

## Xử Lý Cuối Cùng

Cả hai đường dẫn thiết kế đều kết thúc tại điểm quyết định:
- **Lưu Thiết Kế**: Nếu được chọn, thiết kế sẽ được lưu vào tài khoản người dùng
- **Thiết Kế Mới**: Nếu được chọn, người dùng sẽ quay lại giai đoạn lựa chọn dịch vụ
- **Kết Thúc**: Nếu không chọn tùy chọn nào, phiên làm việc kết thúc
