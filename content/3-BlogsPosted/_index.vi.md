---
title: "Các bài blogs đã đăng"
weight: 3
chapter: false
pre: " <b> 3. </b> "
---

Trong quá trình thực tập tại FCAJ, nhóm chúng tôi đã tiến hành nghiên cứu, thảo luận và hoàn thiện 2 bài viết chuyên sâu về hệ sinh thái AWS, được chia sẻ tới cộng đồng [AWS Study Group](https://www.facebook.com/groups/awsstudygroupfcj). Mỗi bài viết là kết quả của quá trình tổng hợp tri thức kỹ thuật và đúc kết kinh nghiệm thực tiễn từ các bài toán vận hành trên nền tảng AWS.

---

### [Blog 1 - BÀI TOÁN CẠN KIỆT KẾT NỐI VỚI RDS PROXY](3.1-Blog1/)

Phân tích chuyên sâu sự cố **Connection Exhaustion (Cạn kiệt kết nối)** phát sinh khi tích hợp kiến trúc Serverless (AWS Lambda) với các hệ cơ sở dữ liệu quan hệ (Amazon RDS). Bài viết làm rõ bản chất điểm nghẽn hệ thống khi hàng nghìn instance Lambda đồng thời khởi tạo kết nối, đồng thời trình bày giải pháp tối ưu thông qua **Amazon RDS Proxy** dựa trên 3 trụ cột kỹ thuật: Multiplexing, Failover linh hoạt và Xác thực IAM.


---

### [Blog 2 - BẢO MẬT TRONG PHÁT TRIỂN PHẦN MỀM TRÊN AWS](3.2-Blog2/)

Tổng hợp 5 nguyên tắc bảo mật cốt lõi trong quy trình phát triển và triển khai ứng dụng trên hạ tầng AWS: Quản lý thông tin xác thực an toàn, áp dụng nguyên tắc cấp quyền tối thiểu (**Least Privilege**), phân vùng mạng Public/Private Subnet, phân tầng bảo vệ ứng dụng với **AWS WAF**, và thiết lập hệ thống giám sát an ninh tự động (**GuardDuty, Inspector, Security Hub**). Bài viết cũng đưa ra các phương án giải quyết cho những thách thức vận hành thực tế như xử lý quá tải lưu lượng và quản lý trạng thái dữ liệu độc lập.



