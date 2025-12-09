---
title: "Các bài blogs đã dịch"
date: "`r Sys.Date()`"
weight: 3
chapter: false
pre: " <b> 3. </b> "
---


Tại đây sẽ là phần liệt kê, giới thiệu các blogs mà các bạn đã dịch. Ví dụ:

### [Blog 1 - Tăng tốc độ đồng bộ Ethereum với Amazon EC2 tối ưu lưu trữ](3.1-Blog1/)
Blog này giới thiệu cách tăng tốc độ đồng bộ một node Ethereum bằng cách sử dụng AWS EC2. Bằng cách kết hợp **i8g (tối ưu lưu trữ)** để đồng bộ nhanh, **Amazon EBS** để di chuyển dữ liệu linh hoạt, và **r8g (tối ưu bộ nhớ)** cho giai đoạn vận hành ổn định, giải pháp giúp rút ngắn thời gian đồng bộ xuống chỉ còn ~8 giờ với chi phí thấp. Bài viết cũng giải thích cách Geth (Snap Sync) và Lighthouse (Checkpoint Sync) đảm bảo bảo mật, đồng thời cung cấp các bước triển khai, phân tích hiệu năng và chi phí—trở thành một hướng dẫn thực tế để vận hành Ethereum node hiệu quả trên AWS.

### [Blog 2 - Di chuyển từ Anthropic’s Claude 3.5 Sonnet sang Claude 4 Sonnet trên Amazon Bedrock](3.2-Blog2/)
Blog này hướng dẫn cách di chuyển từ model **Claude 3.5 Sonnet** (đã ngừng hỗ trợ) sang **Claude 4 Sonnet** trên Amazon Bedrock. Bài viết nêu bật sự khác biệt chính: **cửa sổ ngữ cảnh 1 triệu tokens**, cơ chế **suy luận nâng cao** tích hợp sẵn, và khả năng **chạy công cụ song song**. Ngoài ra, blog còn đưa ra ví dụ cập nhật API, cách viết prompt hiệu quả, minh họa việc **extended thinking** cải thiện độ chính xác suy luận. Cuối cùng, bài viết đề xuất cách đánh giá, kiểm thử an toàn với **Amazon Bedrock Guardrails**, và triển khai an toàn qua **shadow testing, A/B testing, canary rollout** để vừa duy trì **liên tục dịch vụ**, vừa tận dụng công nghệ AI thế hệ mới.

###  [Blog 3 - Xây dựng nền tảng mô phỏng xe tự hành tiết kiệm và dễ mở rộng với AWS RoboMaker](3.3-Blog3/)
Blog trình bày cách MORAI dùng AWS RoboMaker và kiến trúc serverless để chạy mô phỏng xe tự hành quy mô lớn mà không cần quản lý hạ tầng. Nhờ Lambda, API Gateway, SQS, Step Functions và DynamoDB, MORAI tự động hóa chạy mô phỏng, giảm thời gian triển khai tới 4 tuần, và hỗ trợ kiểm thử hàng loạt kịch bản chính xác cao.
###  [Blog 4 - Pearson tại AWS DC Summit 2025: Chuyển đổi giáo dục với giải pháp học tập dùng AI](3.4-Blog4/)
Blog nêu bật hợp tác giữa Pearson và AWS nhằm thu hẹp khoảng cách kỹ năng bằng AI trên Amazon Bedrock. Pearson ra mắt các công cụ như Pearson Revise và Smart Lesson Generator, giúp học sinh và giáo viên học hiệu quả hơn và tiết kiệm thời gian. Bài viết nhấn mạnh học tập cá nhân hóa, tối ưu hóa đánh giá, và đào tạo suốt đời nhờ AI.
###  [Blog 5 - Cách kết nối robot với AWS Cloud và thúc đẩy đổi mới dựa trên dữ liệu](3.5-Blog5/)
Blog giải thích tầm quan trọng của dữ liệu trong robotics và hướng dẫn kết nối robot ROS2 với AWS IoT Core bằng chứng chỉ và MQTT. Bài viết mô tả cách gửi telemetry lên cloud và các use case IoRT như quản lý thiết bị, giám sát robot fleet, teleoperation và triển khai ML với AWS IoT Greengrass.