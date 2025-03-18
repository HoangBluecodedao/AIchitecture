# Đặc tả Class Diagram

![class drawio](https://github.com/user-attachments/assets/1ea55135-f2ad-4e11-811a-655bc585e5d3)

## 1. Các Thành Phần Hệ Thống

### Boundary Classes 
- **UserInterface**: Giao diện người dùng với các phương thức login(), register(), selectModel(), submitInput()
- **APIGateway**: Điểm vào của API với handleRequest(), validateRequest(), routeRequest()

### Entity Classes 
- **User**: Lưu trữ thông tin người dùng với userID, username, email, tokenBalance
- **BaseModel**: Lớp cơ sở cho tất cả các mô hình AI
- **IModelProcessor**: Giao diện xử lý cho các mô hình

### Control Classes 
- **IAuthService**: Dịch vụ xác thực với validateToken(), refreshToken(), login(), register()
- **Session Manager**: Quản lý phiên làm việc của người dùng
- **DataProcessor**: Xử lý và lưu trữ dữ liệu
- **ModelRegistry**: Đăng ký và quản lý các mô hình AI
- **ExteriorAIModel**: Xử lý thiết kế ngoại thất
- **InteriorAIModel**: Xử lý thiết kế nội thất

### Value Classes 
- **BuildingSuggestion**: Kết quả gợi ý thiết kế ngoại thất
- **FurnitureRecommendation**: Kết quả gợi ý nội thất

## 2. Mối Quan Hệ Giữa Các Class

- **Kế Thừa**: ExteriorialModel và InteriorAIModel kế thừa từ BaseModel
- **Phụ Thuộc**: SessionManager phụ thuộc vào IAuthService
- **Tổng Hợp**: ModelRegistry quản lý các đối tượng BaseModel
- **Liên Kết**: UserInterface liên kết với IAuthService

## 3. Chức Năng Chính Của Các Class

### Mô Hình Ngoại Thất
- **Phương Thức**: processTextInput(), processVoiceInput(), generateDescription(), estimateCost()
- **Thuộc Tính**: styleGenerator, costEstimator

### Mô Hình Nội Thất
- **Phương Thức**: processImageInput(), processTextInput(), analyzeSpace(), suggestFurniture(), getProductLinks()
- **Thuộc Tính**: imageProcessor, furnitureClassifier

### Quản Lý Phiên
- **Phương Thức**: createSession(), validateSession(), updateTokenCount()
- **Thuộc Tính**: sessionId, userId, tokenCount, lastActivity
