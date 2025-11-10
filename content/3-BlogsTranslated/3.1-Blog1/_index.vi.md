---
title: "Blog 1"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# Tăng tốc độ đồng bộ Ethereum với Amazon EC2 tối ưu lưu trữ

Việc đồng bộ một node Ethereum có thể mất nhiều ngày trên phần cứng phổ thông nếu không tối ưu. Bài viết này hướng dẫn cách **tăng tốc quá trình đồng bộ, giảm chi phí và vẫn đảm bảo an toàn** bằng cách tận dụng các nhóm Amazon EC2 khác nhau.

---

## Thách thức chính

- **Giai đoạn đồng bộ ban đầu** đòi hỏi hiệu năng tính toán và lưu trữ cao.  
- **Giai đoạn vận hành ổn định** chỉ cần xử lý block mới, khiến phần cứng mạnh bị lãng phí.  
- Bài toán là tìm điểm cân bằng giữa **tốc độ và hiệu quả chi phí**.  

---

## Giải pháp tổng quan

Cách tiếp cận theo từng giai đoạn:

1. Sử dụng **i8g (tối ưu lưu trữ)** để đồng bộ nhanh trên SSD cục bộ.  
2. **Chuyển dữ liệu blockchain** sang Amazon EBS gp3 với hiệu năng nâng cao.  
3. Gắn EBS vào **r8g (tối ưu bộ nhớ)** để vận hành dài hạn.  
4. **Hạ cấu hình EBS** sau khi hoàn tất đồng bộ để tiết kiệm chi phí.  

---

## Quá trình đồng bộ

- **Geth (Execution Layer)** → Snap Sync (tải trạng thái từ nhiều node trong mạng, nhanh và an toàn).  
- **Lighthouse (Consensus Layer)** → Checkpoint Sync (tin tưởng checkpoint đã hoàn tất, phù hợp với Proof-of-Stake).  

---

## Các bước triển khai

1. Khởi tạo **i8g.2xlarge** bằng CloudFormation.  
2. Cài đặt và cấu hình **Geth + Lighthouse**.  
3. Đồng bộ trên SSD cục bộ → hoàn tất sau ~8 giờ (thay vì nhiều ngày).  
4. Sao chép dữ liệu blockchain (~1.2TB) sang **EBS gp3** (16,000 IOPS, 1 GBps).  
5. Di chuyển EBS sang **r8g.large** để chạy ổn định.  
6. Hạ cấu hình EBS xuống **3,000 IOPS, 125 MiBps** để tiết kiệm.  

---

## Hiệu năng & Chi phí

- **CPU**: 30–80% khi đồng bộ, <10% sau đó.  
- **IOPS**: đạt đỉnh 160,000.  
- **Chi phí**:  
  - i8g.2xlarge → < 7 USD cho 10 giờ.  
  - EBS tăng hiệu năng → < 1 USD/giờ.  
- Tuỳ chọn: **x8g.24xlarge với filesystem trong RAM** (đồng bộ ~6.5 giờ) nhưng chi phí rất cao → hiệu quả giảm dần.  

---

## Truy cập & Bảo mật

- Bật JSON-RPC để kết nối với ứng dụng Web3 (vd: MetaMask).  
- Sử dụng **AWS PrivateLink** hoặc **AWS Client VPN** để kết nối riêng tư giữa nhiều VPC hoặc nhiều tài khoản.  

---

## Kết luận

Kết hợp các nhóm EC2 tối ưu lưu trữ và tối ưu bộ nhớ giúp AWS mang lại khả năng đồng bộ node Ethereum **nhanh, an toàn và tiết kiệm chi phí**.  

- **Đồng bộ nhanh** → i8g + SSD.  
- **Di chuyển dữ liệu linh hoạt** → Amazon EBS.  
- **Vận hành dài hạn** → r8g tiết kiệm hơn.  

Cách tiếp cận này rút ngắn thời gian đồng bộ xuống **~8 giờ** thay vì nhiều ngày, đồng thời vẫn kiểm soát chi phí. Giải pháp có thể được mở rộng cho nhiều client Ethereum hoặc workload Web3 khác, và càng hiệu quả hơn nếu kết hợp với automation bằng hạ tầng-as-code.  
