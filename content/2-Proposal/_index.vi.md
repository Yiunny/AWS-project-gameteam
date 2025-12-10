---
title: "Đề xuất dự án"
date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Backend Game Multiplayer Serverless  
Một Giải Pháp AWS Mở Rộng Cho Game Thời Gian Thực & Xử Lý Avatar AI

---

## 1. Tóm tắt dự án
Dự án này nhằm xây dựng một hạ tầng backend serverless mạnh mẽ cho game Unity dạng multiplayer. Hệ thống phân chia rõ ràng trách nhiệm giữa DevOps, Frontend (Unity) và Backend.

Các dịch vụ AWS được sử dụng để xử lý:

- **Xác thực người dùng** → Amazon Cognito  
- **Logic Gameplay** → AWS Lambda  
- **Bảng xếp hạng thời gian thực** → API Gateway WebSocket + DynamoDB Streams  
- **Xử lý Avatar bằng AI** → Lambda dạng container (OpenCV/MediaPipe)  

Kiến trúc mang lại khả năng mở rộng cao, tự động triển khai (CI/CD) và tích hợp WebGL mượt mà (triển khai trên itch.io hoặc CloudFront).

---

## 2. Vấn đề đặt ra

### Vấn đề là gì?
Game multiplayer yêu cầu backend phức tạp: xác thực, giao tiếp thời gian thực, lưu trữ dữ liệu… Các mô hình server truyền thống tốn kém, khó bảo trì và khó mở rộng khi lượng người chơi tăng đột biến.  
Game cũng cần **xử lý avatar bằng AI**, tác vụ này đòi hỏi khả năng tính toán cao.

### Giải pháp
Một kiến trúc AWS **hoàn toàn serverless**:

- **Authentication:** Amazon Cognito (User Pools + Hosted UI)  
- **Logic & Compute:** Lambda (zip + container từ ECR)  
- **Realtime:** WebSocket API + DynamoDB Streams  
- **Storage:** S3 để lưu avatar / assets  

### Lợi ích & Hiệu quả
- **Tiết kiệm chi phí:** Trả theo mức sử dụng (Lambda, DynamoDB)  
- **Mở rộng tự động:** Tự scale khi nhiều người chơi  
- **Tự động hóa:** CI/CD giúp triển khai cực nhanh  

---

## 3. Kiến trúc giải pháp
Hệ thống theo mô hình microservices sự kiện. Unity giao tiếp qua REST (score, shop) và WebSocket (leaderboard). Việc xử lý avatar AI dùng Lambda container.

### Dịch vụ AWS sử dụng
- **Amazon Cognito** – User Pools, Hosted UI  
- **API Gateway (REST + WebSocket)**  
- **AWS Lambda (Zip + Container Image)**  
- **DynamoDB + Streams**  
- **S3**  
- **ECR**  
- **CodePipeline & CodeBuild**  

### Thiết kế thành phần

#### Frontend
Unity WebGL build chạy trên itch.io hoặc CloudFront.

#### Luồng dữ liệu
1. User đăng nhập → Nhận token Cognito  
2. Unity gọi API REST → Lambda → DynamoDB  
3. Upload avatar → Presigned URL → S3 → Lambda AI container  
4. Cập nhật điểm → DynamoDB Stream → WebSocket broadcast  

---

## 4. Triển khai kỹ thuật

### Các giai đoạn triển khai
1. **Thiết lập hạ tầng (DevOps)** – Cognito, DynamoDB, S3, API Gateway  
2. **Backend Skeleton (BE)** – API spec, Postman, Lambda base code  
3. **Tích hợp Login (FE)** – Unity AuthManager  
4. **Kết nối & Streams (DevOps)** – API ↔ Lambda, kích hoạt Streams  
5. **Gameplay Integration (FE)** – DataManager kết nối REST APIs  
6. **Kiểm thử end-to-end** – Login, Shop, Leaderboards, Avatar  
7. **Triển khai** – WebGL + Redirect URL  

### Yêu cầu kỹ thuật
- **Frontend:** Unity C# – AwsConfig, AuthManager, DataManager, RealtimeManager  
- **Backend:** Node.js/Python cho Lambda, Docker cho container AI  
- **DevOps:** IAM roles, CloudFormation (tùy chọn), WAF (tùy chọn)  

---

## 5. Timeline & Milestones

### Giai đoạn 1: Nền tảng (Ngày 1–3)
- DevOps thiết lập Cognito, DynamoDB, S3, API Gateway.

### Giai đoạn 2: Phát triển logic (Ngày 3–8)
- Backend xây Lambda + Container AI Avatar  
- Frontend làm Login  

### Giai đoạn 3: Tích hợp (Ngày 8–12)
- DevOps nối Streams  
- Frontend tích hợp API  

### Giai đoạn 4: Kiểm thử & Ra mắt (Ngày 13–15)
- Kiểm thử realtime leaderboard + avatar pipeline  
- Triển khai WebGL  

---

## 6. Ước tính chi phí
*(Dựa theo AWS Pricing Calculator)*

### Chi phí hạ tầng
- **Lambda:** Hầu hết nằm trong Free Tier  
- **DynamoDB:** Free Tier (25GB)  
- **S3:** ~0.023 USD/GB  
- **CloudWatch:** ~0.5–1 USD/tháng  
- **ECR:** ~0.10 USD/GB  

**Tổng chi phí ước tính:** **< 5 USD/tháng** trong giai đoạn phát triển.

---

## 7. Đánh giá rủi ro

### Ma trận rủi ro
- **Độ phức tạp tích hợp:** Ảnh hưởng cao / Xác suất trung bình  
- **Độ trễ:** Ảnh hưởng trung bình / Xác suất thấp  
- **Chi phí vượt dự kiến:** Ảnh hưởng thấp / Xác suất thấp  

### Giảm thiểu rủi ro
- Dùng Postman Mock Server cho FE phát triển trước  
- Dùng placeholder cho leaderboard / task  
- CloudWatch Logs + alarm (error > 10/min)  

---

## 8. Kết quả kỳ vọng

### Cải thiện kỹ thuật
- Backend game serverless hoàn chỉnh  
- Bảo mật danh tính và dữ liệu người chơi  
- Bảng xếp hạng realtime  
- Xử lý avatar AI tự động  

### Giá trị lâu dài
- Kiến trúc có thể tái sử dụng cho các game tiếp theo  
- Tự động mở rộng mà không cần server  
- Chi phí vận hành thấp  
