---
title: "Sự kiện 4"
date: 2026-07-25
weight: 4
chapter: false
pre: " <b> 4.4. </b> "
---

# BÁO CÁO THU HOẠCH SỰ KIỆN: AGENTIC AI BUILD WEEK (AABW) HACKATHON

**Tên sự kiện:** Agentic AI Build Week (AABW) Hackathon  
**Thời gian:** 25 Tháng 7, 2026
**Địa điểm:** Tầng 26 và Tầng 36, tòa nhà Bitexco, TP. Hồ Chí Minh  
**Chủ đề chính:** Giới thiệu giải pháp AI/AWS thực tế và Chia sẻ kinh nghiệm thực chiến trong 24 giờ thi Hackathon  

---

### PHẦN I: GIỚI THIỆU GIẢI PHÁP & BÀI THI TỪ CÁC ĐỘI

#### 1. Đội Plan V – Dự án: Solution Architect Professional AI Native App
* **Thành viên:** Phạm Tiến Thuận, Phát Huỳnh Hoàng Long, Lê Minh Nghĩa, Trần Đại Vĩ, Nguyễn An.
* **Vấn đề đặt ra (Problem):**
  * Các Solution Architect (SA) mất nhiều thời gian thủ công phân tích yêu cầu từ tài liệu của khách hàng (BRD/PRD).
  * Quy trình thiết kế sơ đồ kiến trúc, viết code Infrastructure as Code (IaC - Terraform) và tính toán chi phí đám mây thường phải xây dựng lại từ đầu, phụ thuộc lớn vào kinh nghiệm cá nhân.
* **Giải pháp (Solution):**
  * Xây dựng ứng dụng AI-Native dành riêng cho SA:
    * Phân tích văn bản tự nhiên và trích xuất danh mục yêu cầu (Requirements Catalogue) trong vài phút.
    * Tự động tạo bản thảo kiến trúc đám mây (hỗ trợ Hybrid-Cloud) tuân thủ tiêu chuẩn doanh nghiệp.
    * Sinh sơ đồ kiến trúc có thể chỉnh sửa (Draw.io và biểu tượng chuẩn AWS).
    * Dự toán chi phí AWS theo từng vùng (vd: ap-southeast-1).
    * Tương tác và tinh chỉnh thông qua Chatbot Sidebar.
* **Kiến trúc kỹ thuật & Tech Stack:**
  * **AI & Retrieval:** Amazon Bedrock, Knowledge Base, Draw.io MCP, AWS Pricing MCP.
  * **Hạ tầng AWS:** ECS Fargate (Backend & Agent Services), Amazon EFS, S3, PostgreSQL, CloudFront, Application Load Balancer (ALB), AWS Cognito, CloudWatch, ECR, Terraform.

---

#### 2. Đội Dream AI Team – Dự án: Signal Scout
* **Thành viên:** Lê Tấn Lực, Đỗ Hoàng Hiếu, Triệu Quốc Hào, Nguyễn Văn Duy Khiêm, Nguyễn Công Minh, Nguyễn Trần Minh Quân.
* **Bài toán & Giá trị mang lại (Value Proposition):**
  * Giúp các đội ngũ chiến lược, quản trị rủi ro và phân tích đối thủ phát hiện sớm các tín hiệu thay đổi chiến lược doanh nghiệp (tái cấu trúc, biến động chỉ số tài chính/vận hành).
  * Thu thập dữ liệu rải rác và tổng hợp thành báo cáo minh bạch có trích dẫn bằng chứng cụ thể (cited evidence), hỗ trợ ban điều hành ra quyết định: *Maintain (Duy trì)*, *Adapt (Thích ứng)*, hoặc *Accelerate (Đẩy nhanh)*.
* **Công nghệ & Tối ưu chi phí:**
  * **Tech Stack:** Amazon Bedrock, AgentCore Runtime & Short-Term Memory, AWS Amplify, WAF, DynamoDB, AWS Lambda, API Gateway, Route53, S3 Intelligent-Tiering, cùng Langfuse, Apify, TinyFish.
  * **Tối ưu chi phí:** Đưa ra bảng ngân sách linh hoạt theo quy mô sử dụng (dao động từ ~$81/tháng ở mức cơ bản đến ~$359/tháng ở quy mô cao).

---

#### 3. Đội 3KA – Dự án: S.H.E.P.H.E.R.D.
*(Smart Human-flow Evaluation, Prediction, Hazard Detection, Response, and Dispatch)*
* **Thành viên:** Huỳnh An Khương, Nguyễn Quốc Huy, Ngô Quang Khôi, Hoàng Lê Thành Đức, Đặng Nguyễn Phước Lộc, Đặng Trường Hưng.
* **Bối cảnh & Bài toán:**
  * Xuất phát từ ý tưởng Đồ án tốt nghiệp (Capstone), đội đem dự án tới Hackathon để kiểm chứng bản MVP thực tế dưới áp lực thời gian.
  * Giám sát sự kiện và địa điểm đông người theo thời gian thực (real-time) để phát hiện ùn tắc sớm thay vì xử lý thụ động.
* **Tính năng cốt lõi & Kiến trúc:**
  * Phân tích video camera live stream: Đếm/theo dõi luồng người, đo mật độ đám đông, đánh giá tình trạng hàng chờ, phát hiện nguy cơ ùn tắc và gợi ý hành động điều phối.
  * **Computer Vision:** YOLO + ByteTrack (nhận diện & theo dõi đối tượng).
  * **AI & Platform:** Amazon SageMaker, Amazon Bedrock AgentCore + Strands Agent (Agentic AI Layer với *Autonomous Monitor* theo dõi tự động và *Operator Copilot* hỗ trợ hỏi đáp dữ liệu real-time), Dashboard React.

---

### PHẦN II: CHIA SẺ KINH NGHIỆM THI HACKATHON (HACKATHON JOURNEY)

Bên cạnh giải pháp kỹ thuật, đại diện các đội thi đã chia sẻ hành trình 24 giờ thi đấu với nhiều bài học thực chiến giá trị:

#### 1. Thách thức & Sự cố thực tế
* **Rào cản chuyên môn & Thời gian:** Nhiều thành viên lần đầu tiếp xúc với AI và dịch vụ AWS, phải hoàn thiện sản phẩm chạy được (MVP) chỉ trong đúng 24 giờ.
* **Sự cố kỹ thuật:** Xử lý video real-time bị giật lag, mất tracking giữa các khung hình, thức xuyên đêm debug tới 3 giờ sáng, hay sự cố vô tình push file chứa thông tin bảo mật **(.env)** lên GitHub.

#### 2. Trải nghiệm đáng nhớ
* Không khí 24 giờ thức trắng làm việc nhóm, cùng thảo luận phân công công việc, đi dạo nạp năng lượng đêm khuya và tinh thần đồng đội gắn kết.
* Mở rộng mạng lưới kết nối (networking) với các kỹ sư, chuyên gia và dàn Mentor giàu kinh nghiệm từ AWS.

#### 3. Lời khuyên xương máu cho người tham gia Hackathon
1. **Chuẩn bị trước cuộc thi (Preparation):** Xác định rõ tiêu chí hoàn thành (definition of done), chuẩn bị sẵn mẫu template/tài khoản và phân công vai trò rõ ràng (coding, UI/UX, pitching).
2. **Quản lý phạm vi dự án (Scope it tiny):** Tập trung hoàn thiện xuất sắc **1 tính năng cốt lõi**. Một sản phẩm nhỏ chạy mượt mà luôn đánh bại một ý tưởng hoành tráng nhưng dở dang hoặc nhiều lỗi.
3. **Tận dụng sự trợ giúp từ Mentor:** Chủ động hỏi đáp và lắng nghe góp ý từ các Mentor chuyên môn.
4. **Mạnh dạn dấn thân (Just sign up):** Đừng chờ tới khi "cảm thấy đủ giỏi", bước vào phòng thi đã là một bước tiến lớn.

---

### PHẦN III: TỔNG KẾT (KEY TAKEAWAYS)

* **Sự dấn thân là bước khởi đầu:** Dám đăng ký và tham gia cuộc thi đã giúp nâng cao vượt bậc kỹ năng thực chiến.
* **Tính thực tế và chỉn chu:** Sản phẩm hoạt động ổn định quan trọng hơn quy mô ý tưởng.
* **Giá trị kết nối:** Cuộc thi mang me cơ hội rèn luyện áp lực cao và tìm kiếm những đồng đội cùng chí hướng trong cộng đồng Cloud & AI.

---

### PHẦN IV: BÀI HỌC VÀ CẢM NHẬN CỦA BẢN THÂN KHI THAM DỰ BUỔI CHIA SẺ

Với vai trò là người tham dự lắng nghe các đội thi trình bày dự án và chia sẻ kinh nghiệm thực chiến từ cuộc thi Hackathon **Agentic AI Build Week (AABW)**, bản thân em đã gặt hái được nhiều góc nhìn mới mẻ cùng những bài học quý giá:

#### 1. Góc nhìn Kỹ thuật & Tư duy Kiến trúc Cloud / AI
* **Tư duy thiết kế ứng dụng Agentic AI thực tế:** Qua việc theo dõi các sản phẩm của đội Plan V, Dream AI và 3KA, em đã hình dung rõ ràng hơn cách tích hợp các mô hình Generative AI (Amazon Bedrock, AgentCore) với hạ tầng đám mây (ECS Fargate, Lambda, SageMaker, DynamoDB) để giải quyết các bài toán tự động hóa thực tế của doanh nghiệp.
* **Chiến lược tối ưu chi phí & Hạ tầng:** Học hỏi cách các đội thi phân tích ngân sách vận hành chi tiết, kết hợp linh hoạt giữa các dịch vụ Serverless, S3 Intelligent-Tiering và DynamoDB on-demand để tối ưu chi phí hạ tầng Cloud.
* **Tích hợp Computer Vision với Machine Learning Pipeline:** Hiểu thêm phương pháp kết hợp mô hình YOLO + ByteTrack với Amazon SageMaker và Bedrock Agent trong bài toán xử lý video thời gian thực và tự động đưa ra cảnh báo điều phối.

#### 2. Cảm nhận & Bài học Thực tiễn từ Kinh nghiệm của Các Đội thi
* **Bài học về quản lý phạm vi (Scope Management):** Lắng nghe chia sẻ của các anh/chị thi trước giúp em nhận ra tầm quan trọng của việc tập trung làm thật chỉn chu một **tính năng cốt lõi (MVP)** thay vì ôm đồm quá nhiều ý tưởng dở dang khi triển khai dự án công nghệ.
* **Tinh thần làm việc nhóm dưới áp lực cao:** Dù không trực tiếp tham gia thi đấu 24 giờ, những câu chuyện thực tế về cách các đội phối hợp ăn ý, cùng vượt qua sự cố kỹ thuật khẩn cấp xuyên đêm và tinh thần dấn thân đã truyền cho em nhiều cảm hứng và động lực học hỏi.
* **Giá trị từ sự kết nối & Giao lưu cộng đồng:** Buổi chia sẻ tạo cơ hội tuyệt vời để em mở rộng góc nhìn, lắng nghe những góp ý chuyên môn sâu sắc từ dàn Mentor AWS, đồng thời học hỏi được tư duy xử lý vấn đề của các kỹ sư đi trước.

#### 3. Hình Ảnh Sự Kiện
![FCAJ Community Day - AABW Showcase](/images/event4.png)
