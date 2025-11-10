---
title: "Đề Xuất Dự Án"
date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Alien Spy: Hạ Tầng Game Dựa Trên AWS
## Game Unity Serverless cho Phát Triển Kỹ Năng Cloud


### 1. Tóm tắt điều hành
**Alien Spy** là một trò chơi endless runner serverless phát triển bằng Unity và AWS, nhằm làm nền tảng học tập thực hành tích hợp cloud. Nền tảng này giải quyết khoảng trống kỹ năng cloud ở các trường đại học Việt Nam: sinh viên học các dịch vụ AWS như Lambda, DynamoDB, API Gateway nhưng hiếm khi trải nghiệm tích hợp nhiều dịch vụ thực tế.  


**Các tính năng chính**:  
- **Chế độ đua 1v1 thời gian thực**  
- **Cá nhân hóa Avatar bằng AI**  
- **Hệ thống vật phẩm & boss**  
- **Backend serverless**  
- **Bảng xếp hạng & sự kiện thời gian thực**  


**Lợi ích & ROI**  
- Sinh viên có dự án portfolio AWS thực hành.  
- Giảng viên có mô-đun lab sẵn sàng triển khai.  
- Trường đại học nâng cao khả năng tuyển dụng & cơ hội hợp tác AWS.  
- Đầu tư: $0–100 cho hạ tầng, tổng 600 giờ làm việc (12 tuần × 5 thành viên × 10 giờ/tuần).  


---


### 2. Vấn đề
#### Vấn đề là gì?
- Hầu hết trò chơi endless runner chỉ tối ưu cho mobile.  
- Sinh viên hiếm khi có dự án cloud tích hợp thực tế.  
- Thiếu cá nhân hóa avatar và tính năng cạnh tranh → giảm tương tác.  


#### Giải pháp
- **Kiến trúc serverless trên AWS** tích hợp với Unity WebGL.  
- Multiplayer thời gian thực (WebSocket), avatar AI, cơ chế vật phẩm & boss.  
- Backend DynamoDB có khả năng mở rộng, lưu trữ tài nguyên S3, CDN CloudFront, xác thực Cognito.  


#### Các bên liên quan
| Bên liên quan | Mối quan tâm |
|------------|---------|
| Người chơi | Trải nghiệm game desktop cạnh tranh, cá nhân hóa |
| Nhà phát triển | Dự án học thuật tích hợp Unity & AWS |
| Giảng viên | Dự án kỹ thuật tốt, có thể tái sử dụng |


---


### 3. Kiến trúc giải pháp
**Kiến trúc tổng thể**  


![Alien Spy Architecture Placeholder](/images/2-Proposal/platform_architecture.jpeg)


**Dịch vụ AWS sử dụng**  
- **Route 53** – DNS & khả năng sẵn sàng cao  
- **WAF** – Bảo mật  
- **CloudFront** – CDN cho WebGL  
- **S3** – Game builds, tài nguyên, avatar  
- **API Gateway** – REST/WebSocket APIs  
- **Cognito** – Xác thực người dùng  
- **Lambda** – Backend serverless  
- **DynamoDB** – Dữ liệu người chơi, bảng xếp hạng  
- **SNS / DynamoDB Streams** – Thông báo thời gian thực  
- **CodePipeline & CodeBuild** – CI/CD tự động  
- **CloudFormation** – Hạ tầng dưới dạng mã  


**Thiết kế thành phần**  
- **Frontend**: Unity WebGL qua CloudFront + S3  
- **Backend**: API Gateway → Lambda → DynamoDB → Streams → Lambda → Player  
- **Pipeline Avatar AI**: Lambda xử lý avatar → lưu S3  
- **Leaderboard thời gian thực**: DynamoDB Streams + Lambda đẩy cập nhật  
- **Quản lý người dùng**: Cognito quản lý 5–50 tài khoản sinh viên  


**Bảo mật & tuân thủ**  
- WAF, TLS 1.2+, mã hóa AES-256 S3, IAM role theo nguyên tắc least privilege  


---


### 4. Triển khai kỹ thuật
**Các giai đoạn triển khai**  
1. Thiết kế & thiết lập hạ tầng  
2. Phát triển API Backend  
3. Tích hợp Frontend / Game Client  
4. Kiểm thử & tối ưu  
5. Triển khai CI/CD  


**Yêu cầu kỹ thuật**  
- Build Unity WebGL  
- AWS Lambda, API Gateway, DynamoDB, SNS, CloudFront, S3, Cognito  
- Python cho xử lý avatar AI  
- CloudFormation/CDK cho hạ tầng as code  


**Chiến lược kiểm thử**  
- Unit test (xUnit/NUnit)  
- Integration test (Postman)  
- Performance test (CloudWatch/K6)  
- End-to-end system test  


**Triển khai & rollback**  
- CodePipeline tự động deploy  
- Versioning Lambda & backup DynamoDB để rollback  


---


### 5. Timeline & Milestones
| Giai đoạn | Tuần | Kết quả giao hàng |
|-------|-------|--------------|
| Pre-production | 1–3 | GDD, Kiến trúc AWS |
| Core Gameplay | 4–7 | Bản đồ, chướng ngại, vật phẩm, leaderboard |
| Boss & Avatar | 8–9 | AI boss, cutscenes, pipeline avatar AI |
| Testing & Optimization | 10–11 | QA, tuning hiệu năng |
| Deployment & Demo | 12 | Demo WebGL, tài liệu |


---


### 6. Ước tính ngân sách
**Dự kiến Free Tier AWS**  
- Lambda: $0  
- API Gateway: ~$0.01/tháng  
- S3 + CloudFront: ~$0.15/tháng  
- DynamoDB: $0  
- SNS/WebSocket: $0  
- Tổng: $0–5/tháng (tối đa $50–100 nếu vượt Free Tier)  


**Chi phí phát triển & dịch vụ bên thứ 3**  
- 0 VND, Python AI mã nguồn mở, Unity Personal  


---


### 7. Đánh giá rủi ro
| Rủi ro | Tác động | Xác suất | Giải pháp |
|------|--------|------------|------------|
| Misconfiguration AWS | Cao | Trung bình | CloudFormation templates, kiểm thử |
| WebSocket không ổn định | Trung bình | Cao | Reconnect & fallback sang REST |
| Tích hợp Unity-AWS | Cao | Trung bình | Wrapper C# riêng, test độc lập |
| Vượt Free Tier | Thấp | Trung bình | Cảnh báo ngân sách, tối ưu payload |
| Trễ tiến độ | Trung bình | Cao | Agile sprint, buffer 1 tuần |
| Lộ dữ liệu | Cao | Thấp | S3 private, auth Cognito, TLS |


---


### 8. Kết quả kỳ vọng
- Latency API <100ms; cập nhật real-time <2s  
- Hỗ trợ 50+ người chơi đồng thời  
- Xử lý avatar AI <5s  
- ≥90% test pass  
- Chi phí < $30/tháng  


**Lợi ích dài hạn**  
- Nền tảng multiplayer có khả năng mở rộng  
- Học AWS thực hành & dự án portfolio  
- Hạ tầng tái sử dụng cho game/lab tương lai  


---


### 9. Tổng quan Game Design – Alien Spy
- **Chủ đề**: Sci-fi Việt Nam  
- **Nhân vật**: Alien trên UFO, avatar cá nhân hóa AI  
- **Chướng ngại**: Dashable & non-dashable  
- **Vật phẩm**: Buffs/Debuffs  
- **Tiền & Shop**: Thuốc lào  
- **Bản đồ & Level**: 3 vùng, 5 level mỗi vùng, boss tại các level mốc  
- **Leaderboard**: Real-time, local & global  
- **Sự kiện thời gian thực**: Thử thách trực tiếp qua SNS/DynamoDB Streams