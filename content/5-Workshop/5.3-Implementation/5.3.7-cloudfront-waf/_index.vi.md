---
title : "Cấu hình Amazon CloudFront Distribution & AWS WAF"
date : 2026-07-30
weight : 7
chapter : false
pre : " <b> 5.3.7 </b> "
---

Tạo CDN distribution phía trước API Gateway để tối ưu hiệu năng truyền tải và kích hoạt tường lửa AWS WAF bảo vệ API khỏi các cuộc tấn công DDoS và độc hại.

#### Thiết lập thông tin chung 

- Truy cập dịch vụ **CloudFront** $\rightarrow$ chọn **Create distribution**.
- Tại mục **Distribution options**:
  - **Distribution name**: Nhập `telco-churn-cloudfront-waf`.
  - **Distribution type**: Chọn **Single website or app**.
- Tại mục **Domain**:
  - Để trống mục **Route 53 managed domain**

![cloudfront-get-started](/images/5-Workshop/5.3-Implementation/cdn-name.png)

#### Cấu hình Origin & Cache Settings

- Tại mục **Origin type**: Chọn **API Gateway**.
- Tại mục **API Gateway origin**:
  - Chọn endpoint API Gateway đã tạo ở bước trước (ví dụ: c6kbjaktj9.execute-api.ap-southeast-1.amazonaws.com).
- Tại mục **Origin path - optional**: Để trống nếu muốn trỏ về root API hoặc điền đường dẫn cụ thể nếu cần.
- Tại mục **Settings**:
  - **Origin settings**: Chọn **Use recommended origin settings**.
  - **Cache settings**: Chọn **Use recommended cache settings tailored to serving API Gateway content**.

![cloudfront-origin-settings](/images/5-Workshop/5.3-Implementation/origin.png)

#### Cấu hình Bảo mật AWS WAF (Enable security)

- Chọn **Enable security protections** để tích hợp AWS WAF bảo vệ ứng dụng.
- Trong danh sách **Included security protections**:
  - Tích chọn **Rate limiting** (Khuyến nghị): Giới hạn số lượng request từ một IP để chống tấn công HTTP flood / DoS.
  - Nhập thông số **When rate exceeds...**: `100` requests per IP address per 5-minute period.
- Nhấn **Next** và kiểm tra lại toàn bộ cấu hình trước khi nhấn **Create distribution**.

![cloudfront-security-waf](/images/5-Workshop/5.3-Implementation/waf.png)