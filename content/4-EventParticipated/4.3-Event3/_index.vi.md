---
title: "Sự kiện 3"
date: 2026-07-11
weight: 3
chapter: false
pre: " <b> 4.3. </b> "
---

# BÁO CÁO THU HOẠCH SỰ KIỆN: AWS COMMUNITY DAY & TECH WORKSHOP

**Tên sự kiện:** AWS Community Day & Tech Workshop  
**Thời gian:** 11 Tháng 7, 2026
**Địa điểm:** Tầng 26, tòa nhà Bitexco, TP. Hồ Chí Minh  
**Chủ đề chính:** Bảo mật ứng dụng với AWS Security Agent, Ôn luyện chứng chỉ AWS CLF-C02 & Tư duy Giám sát SLA thực chiến  

---

### PHẦN I: NỘI DUNG CHI TIẾT CÁC CHỦ ĐỀ TRÌNH BÀY

#### 1. Chủ đề 1: Securing Your Web Apps With AWS Security Agent
* **Diễn giả:** Thinh Nguyen (DevOps/DevSecOps/Cloud Engineer – Styl Solutions)
* **Thách thức bảo mật thực tế (The Security Bottleneck):**
  * Các đợt kiểm thử xâm nhập thủ công (Pentest) thường kéo dài nhiều tuần, chi phí đắt đỏ ($5.000 – $20.000/đợt) và kết quả không đồng nhất do phụ thuộc lớn vào tâm lý và kỹ năng của người thử nghiệm.
* **Giải pháp AWS Security Agent (Frontier Agent):**
  * Tự động hóa suy luận dựa trên **Amazon Bedrock** để lập kế hoạch và thực thi công tác kiểm tra an toàn thông tin toàn diện.
  * **Bao phủ toàn bộ vòng đời ứng dụng (Full Lifecycle Coverage):**
    * **Design Security Review:** Đánh giá tài liệu kiến trúc (Markdown/Terraform) so với các bộ tiêu chuẩn PCI DSS, NIST CSF, AWS Well-Architected (Cung cấp gói Free Tier: 200 lượt/tháng).
    * **Code Security Review:** Tự động quét Pull Request (PR) trên GitHub/GitLab, nhận xét trực tiếp trên từng dòng mã nguồn và gợi ý sửa lỗi tự động (Auto-PR Fixes) (Free Tier: 1.000 PRs/tháng).
    * **Automated Pentesting:** Giả lập tấn công thực tế (IDOR => XSS) và xác thực lỗ hổng qua chuỗi khai thác thực sự.
* **Thực tế về Chi phí (Pricing Reality):**
  * Bản dùng thử miễn phí 2 tháng (400 task-hours/tháng). Mức giá trả theo dung lượng sử dụng là $50/task-hour.
  * Trong dự án thực tế, tổng chi phí kiểm thử khoảng **$1.500 – $2.500**, tiết kiệm hơn rất nhiều so với mức $10.000 khi thuê đội Pentest thủ công.
* **Hạn chế cần lưu ý:**
  * Bị chặn bởi các cơ chế xác thực nâng cao như MFA, Biometrics, mTLS.
  * Khó phát hiện các lỗi logic nghiệp vụ phức tạp nếu thiếu bối cảnh sâu.
  * Cần theo dõi sát thời gian chạy (Task-Hours) để tránh phát sinh chi phí vượt ngân sách.

---

#### 2. Chủ đề 2: Inside The Exam: AWS Cloud Practitioner (CLF-C02)
* **Diễn giả:** Ngo Le Tan Huy
* **Tổng quan kỳ thi:**
  * Cấp độ nhập môn (Foundational) dành cho người mới bắt đầu, tập trung vào tư duy tổng quan về Điện toán đám mây mà không yêu cầu lập trình hay cấu hình kỹ thuật chuyên sâu.
  * **Cấu trúc:** 65 câu hỏi trắc nghiệm, thời gian 90 phút (người không nói tiếng Anh bản ngữ được đăng ký cộng thêm 30 phút), điểm đạt là 700/1000, chứng chỉ có giá trị 3 năm.
* **Phân bổ 4 Domain nội dung:**
  * **Cloud Concepts (24%):** Tư duy chuyển đổi số, 6 lợi ích của Cloud, AWS WAF, AWS CAF.
  * **Security and Compliance (30%):** Mô hình chia sẻ trách nhiệm (Shared Responsibility Model), IAM, Security Group, NACL, AWS Shield, AWS WAF, AWS Artifact.
  * **Cloud Technology and Services (34%):** Hạ tầng toàn cầu (Region, AZ, Edge Location) và các dịch vụ cốt lõi về Compute (EC2, Lambda), Storage/DB (S3, EBS, RDS, DynamoDB), Networking (VPC, Route 53).
  * **Billing, Pricing, and Support (12%):** Các mô hình giá EC2, AWS Cost Explorer, AWS Budgets, các gói Support Plan.
* **Kinh nghiệm ôn thi & Mẹo làm bài:**
  * **Phương pháp:** Học theo từ khóa (*Keyword Thinking*), phân tích kỹ lý do đúng/sai khi luyện đề thử, kết hợp thực hành trải nghiệm thực tế trên AWS Free Tier.
  * **Mẹo trong phòng thi:** Áp dụng phương pháp loại trừ (thường có 2 đáp án không liên quan), chọn đáp án đơn giản và trực diện nhất, chú ý các từ khóa bẫy (`NOT`, `Least cost`), và dùng tính năng *Flag for review* để xem lại câu khó sau.

---

#### 3. Chủ đề 3: SLA and Monitoring: From SLA to Monitoring What Really Matters
* **Diễn giả:** Nguyễn Huỳnh Sơn (Ex-Infrastructure Reliability Engineer)
* **Khái niệm & Vai trò của SLA (Service Level Agreement):**
  * SLA giúp thiết lập kỳ vọng dịch vụ rõ ràng với khách hàng, quy trách nhiệm vận hành và hỗ trợ quản trị rủi ro hệ thống.
* **Sự thật về Giám sát (Monitoring Reality):**
  * **Hạ tầng khỏe chưa chắc Trải nghiệm người dùng đã tốt** (*Healthy infrastructure $\neq$ Happy users*).
  * *Ví dụ thực tế:* Cấu hình `/health` trả về 200 OK, CPU của EC2 chỉ chạy 18% rất mát, nhưng kết nối tới RDS Database bị lỗi khiến người dùng không thể đăng nhập (`/login` thất bại).
* **Mô hình Kim tự tháp Giám sát (Monitoring Pyramid):**
  * Cần giám sát theo thứ tự từ trên xuống dưới, tránh chỉ tập trung vào tầng dưới cùng:
    1. **Customer Experience:** Người dùng có đăng nhập/mua hàng thành công không?
    2. **Business Metrics:** Tỷ lệ đăng nhập thành công, số lượng đơn hàng, doanh thu.
    3. **Application:** Đột biến độ trễ (Latency), tỷ lệ lỗi ứng dụng.
    4. **Infrastructure:** CPU, Memory, Disk, Network.
    5. **Cloud Provider:** Trạng thái dịch vụ AWS (EC2, RDS, ALB).
* **Thông điệp cốt lõi & Triết lý vận hành:**
  * Cần theo dõi những gì người dùng thực sự thao tác (Login, Checkout, Payment), thay vì chỉ nhìn vào chỉ số máy chủ.
  * **Mô hình chia sẻ trách nhiệm SLA:** AWS đảm bảo sẵn sàng của hạ tầng Cloud, còn bạn phải chịu trách nhiệm hoàn toàn về Trải nghiệm của Khách hàng.
  * Triết lý từ Dr. Werner Vogels (CTO Amazon): *"Everything fails all the time, so plan for failure and nothing fails"*.

---

### PHẦN II: BÀI HỌC VÀ CẢM NHẬN CỦA BẢN THÂN KHI THAM DỰ BUỔI EVENT

Với vai trò là người tham dự lắng nghe các diễn giả trình bày, bản thân em đã rút ra nhiều bài học thực tiễn và góc nhìn vận hành quý báu:

#### 1. Bài học về Bảo mật & Tự động hóa DevSecOps
* Nhận thức rõ xu hướng ứng dụng Generative AI (AWS Security Agent) vào quy trình quét lỗi kiến trúc và code tự động, giúp giảm thiểu chi phí Pentest thủ công nhưng vẫn đảm bảo tiêu chuẩn bảo mật cho dự án.

#### 2. Định hướng Ôn luyện Chứng chỉ AWS
* Nắm vững cấu trúc bài thi CLF-C02, phương pháp tư duy theo từ khóa (Keyword Thinking) và kỹ năng làm bài trắc nghiệm thực chiến để chuẩn bị cho lộ trình lấy chứng chỉ AWS Cloud Practitioner.

#### 3. Thay đổi Tư duy Giám sát Hệ thống (Customer-Centric Monitoring)
* Thay đổi tư duy từ giám sát hạ tầng thuần túy sang **giám sát dựa trên trải nghiệm người dùng cuối** (User Experience). Hiểu rằng một hệ thống xanh chỉ thực sự có giá trị khi các luồng nghiệp vụ cốt lõi (Login, Payment) hoạt động trơn tru.

#### 4. Hình Ảnh Sự Kiện
![FCAJ Community Day](/images/event3.png)