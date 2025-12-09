---
title: "Blog 3"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 3.3. </b> "
---


# Xây dựng ứng dụng lái xe tự động hiệu quả về chi phí và có khả năng mở rộng với AWS RoboMaker

**MORAI** là một startup công nghệ xe tự hành, chuyên cung cấp nền tảng mô phỏng để xác thực và kiểm chứng các hệ thống lái xe tự động.
Trình mô phỏng của họ – **MORAI SIM** – đang được ứng dụng rộng rãi trong nhiều lĩnh vực như xe tự hành, hệ thống hỗ trợ lái xe nâng cao (ADAS), ô tô, robot và hàng không vũ trụ.
Các công ty trong nhiều ngành, từ xe tự hành, vận tải hàng không đô thị, robot đến logistics, đang sử dụng **MORAI** SIM để thử nghiệm hàng loạt kịch bản phức tạp nhằm giảm chi phí, rút ngắn thời gian ra thị trường và giảm thiểu rủi ro pháp lý.
Trong khu vực công, nhiều cơ quan chính phủ và tổ chức giáo dục – như cơ quan an toàn giao thông Hàn Quốc và các trường đại học – cũng sử dụng **MORAI SIM** để phục vụ mục đích đào tạo và đáp ứng yêu cầu pháp lý.
**MORAI SIM** cho phép tiến hành thử nghiệm ảo cho xe tự hành bằng cách tạo ra môi trường mô phỏng có độ chính xác cao, bao gồm mô hình cảm biến, mô hình xe và mô phỏng kịch bản thực tế.
Trình mô phỏng có thể tái tạo lại các tình huống ngoài đời thực nhờ vào công cụ render hiệu năng cao hoặc nhập dữ liệu từ nhật ký thực tế của xe.
Trong bài viết này, chúng tôi chia sẻ cách **MORAI**, khách hàng của AWS, đã tận dụng AWS RoboMaker để giúp các nhà phát triển chạy và mở rộng thử nghiệm xe tự hành mà không cần tự quản lý hạ tầng.
Bằng việc xây dựng kiến trúc serverless kết hợp AWS RoboMaker cùng các dịch vụ AWS khác, **MORAI** đã tạo ra một nền tảng thử nghiệm hiệu quả, tiết kiệm chi phí và dễ mở rộng cho việc kiểm thử xe tự hành ở quy mô lớn.


---

## Thách thức trong việc phát triển xe tự hành

Có một sự khác biệt lớn giữa việc tạo ra bản demo xe tự hành và phát triển một sản phẩm thương mại — vì sản phẩm thực tế cần bằng chứng đáng tin cậy rằng hệ thống đủ an toàn.
Tuy nhiên, việc thử nghiệm xe tự hành trong điều kiện thực tế lại vô cùng tốn kém và phức tạp.
Khi có bất kỳ thay đổi nào trong hệ thống, việc chạy lại toàn bộ thử nghiệm gần như không khả thi, khiến quá trình phát triển trở nên mất thời gian và tốn chi phí.
**Phát triển xe tự hành** là một trong những thách thức công nghệ tiêu tốn tài nguyên nhất trong ngành ô tô.
Để rút ngắn quá trình phát triển, **MORAI** cần một giải pháp cho phép tăng tốc xây dựng, thử nghiệm và quản lý ứng dụng robot một cách linh hoạt.
Bên cạnh đó, quản lý phiên bản và điều phối hàng trăm kịch bản mô phỏng với các tài nguyên tính toán đa dạng cũng là một thách thức lớn.
Do phục vụ nhiều nhóm khách hàng với nhu cầu thử nghiệm khác nhau, **MORAI** phải đối mặt với sự biến động tài nguyên bất ngờ.
Vì vậy, họ cần một cách thức hiệu quả và linh hoạt hơn để mở rộng hoặc thu hẹp tài nguyên theo nhu cầu — mà vẫn tối ưu chi phí.

---
## Xây dựng ứng dụng lái xe tự động hiệu quả và có khả năng mở rộng
Nhờ vào AWS RoboMaker Batch API, **MORAI** đã có thể tự động hóa quá trình điều phối và quản lý các tác vụ mô phỏng.
AWS RoboMaker giúp các nhà phát triển tập trung vào việc phát triển nền tảng thử nghiệm cho xe tự hành, bao gồm thử nghiệm dựa trên kịch bản và tạo bản đồ độ nét cao (HD maps) một cách tự động.
Kết quả, các nhà phát triển tại **MORAI** đã rút ngắn thời gian triển khai phần mềm xe lên đến 4 tuần.
![Morai Stimulation Camera](/images/MoraiStimulation.png)
> *Hình 1. Trình mô phỏng lái xe tự hành của **MORAI** mô phỏng các tình huống chuyển làn khác nhau.*

---

Với **MORAI** SIM Cloud, các nhà phát triển xe tự hành có thể thử nghiệm các thuật toán lập lịch, định tuyến và điều hướng ở quy mô chưa từng có trước đây.
Công cụ này cho phép họ tái tạo và tùy chỉnh kịch bản bằng một ngôn ngữ mô tả tự nhiên, dễ hiểu, cùng giao diện đồ họa trực quan để thay đổi các điều kiện như thời tiết, phương tiện hoặc vật cản.
Các nhà phát triển cũng có thể chọn và cấu hình các loại cảm biến xác thực khác nhau — như camera, LiDAR và Radar — tùy theo nhu cầu.
Ngoài ra, **MORAI** SIM còn cung cấp môi trường trực quan để cài đặt, hiệu chỉnh và giám sát động lực học (dynamics) của phương tiện.
Cách tiếp cận mô phỏng dựa trên dữ liệu (data-driven simulation) này mang lại giá trị trong mọi giai đoạn phát triển xe tự hành — từ khởi động dự án, thử nghiệm cho đến giai đoạn chấp nhận cuối cùng.
Kiến trúc của **MORAI** SIM được thiết kế từ đầu để trở thành nền tảng mô phỏng có độ chính xác cao, hỗ trợ mô phỏng đa cảm biến ở quy mô lớn.
Hệ thống đa lớp và linh hoạt này có thể dễ dàng mở rộng để kiểm thử và xác thực phần mềm tự động hóa trong nhiều lĩnh vực khác nhau.
Điều này giúp tiết kiệm hàng trăm nghìn đô la mỗi năm cho khách hàng bằng cách giảm thiểu rủi ro thử nghiệm và xác thực.


---

## Kiến trúc giải pháp

Nhóm **MORAI** sử dụng các dịch vụ serverless của AWS để giảm thiểu gánh nặng quản lý hạ tầng và chi phí máy chủ.
Vì việc dự đoán mức sử dụng tài nguyên là rất khó trong môi trường mô phỏng phức tạp, **MORAI** đã chọn cách xây dựng kiến trúc serverless linh hoạt và tiết kiệm chi phí.
Hình dưới đây minh họa kiến trúc backend của **MORAI** SIM, được xây dựng trên:
- AWS Lambda,
- Amazon API Gateway,
- Amazon Simple Queue Service (Amazon SQS),
- và Amazon DynamoDB.


Các dịch vụ này tự động mở rộng dựa trên nhu cầu, không yêu cầu quản lý máy chủ thủ công.
![Morai's Structure](/images/MoraiStructure.png)
> *Hình 2. Kiến trúc mô phỏng của **MORAI** sử dụng AWS RoboMaker API.*

## Quy trình hoạt động
 ---
1. Người dùng gửi yêu cầu mô phỏng (bao gồm thuật toán và cấu hình). **MORAI** SIM tiếp nhận yêu cầu và chuyển đến API Gateway.
2. AWS Lambda xác thực và phân tích yêu cầu. Nếu hợp lệ, metadata mô phỏng sẽ được chuyển thành định dạng JSON và gửi đến hàng đợi SQS.
 Thông thường, một yêu cầu người dùng có thể chứa 50–100 tác vụ mô phỏng.
3. Để chạy song song, Lambda sẽ chia các tác vụ lớn thành nhiều lô nhỏ (batch) và gửi đến hàng đợi SQS thứ hai.
4. Lambda tiếp theo sẽ lấy yêu cầu từ hàng đợi này và kích hoạt AWS Step Functions để bắt đầu mô phỏng khi AWS RoboMaker sẵn sàng.
5. Máy trạng thái gồm hai hàm Lambda:


- Lambda đầu tiên tạo ứng dụng robot và ứng dụng mô phỏng, rồi khởi chạy tác vụ RoboMaker.
- Lambda thứ hai theo dõi trạng thái mô phỏng và ghi lại kết quả.


6. Sau khi hoàn tất, kết quả và log được lưu trong Amazon DynamoDB, đồng thời thông báo hoàn thành được gửi lại cho người dùng.


---

## Kết luận

**MORAI** đã tìm thấy một cách tiếp cận mang tính mở rộng và hiệu quả về chi phí để chạy mô phỏng xe tự hành và ứng dụng robot trên AWS Cloud.
Nhờ tận dụng AWS RoboMaker cùng các dịch vụ serverless khác, **MORAI** đã rút ngắn đáng kể thời gian phát triển, giảm chi phí vận hành, và triển khai mô phỏng quy mô lớn mà không cần tự quản lý hạ tầng.

---