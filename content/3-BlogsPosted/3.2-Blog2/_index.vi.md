---
title: "Blog 2"
date: 2026-07-10
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---

# BẢO MẬT TRONG PHÁT TRIỂN PHẦN MỀM — KHÔNG CHỈ LÀ VIẾT CODE AN TOÀN

### 1. Lời mở đầu

Trong quá trình tiếp cận và triển khai giải pháp trên nền tảng AWS, một thực tế quan trọng được chỉ ra: ở giai đoạn phát triển ban đầu, các kỹ sư thường ưu tiên tối đa cho mục tiêu **"đảm bảo ứng dụng vận hành đúng chức năng"**. Tuy nhiên, khi hệ thống được phát hành lên môi trường Internet, bài toán cốt lõi lại dịch chuyển sang: **Làm thế nào để đảm bảo an toàn tuyệt đối cho hệ thống?**

Một ứng dụng dù đạt hiệu năng cao về mặt chức năng nhưng chỉ cần tồn tại một sai sót trong cấu hình hoặc một hổng bảo mật nhỏ cũng có thể dẫn đến nguy cơ rò rỉ dữ liệu hoặc tổn hại hạ tầng. Hệ sinh thái AWS không chỉ cung cấp hạ tầng tính toán mà còn trang bị chuỗi dịch vụ bảo mật chuyên sâu, hỗ trợ thiết lập kiến trúc an toàn ngay từ giai đoạn thiết kế ban đầu thay vì thụ động khắc phục sự cố.

---

### 2. 5 Bài học Bảo mật Cốt lõi trên Hạ tầng AWS

#### 2.1. Quản lý Tuyệt đối Thông tin Xác thực (Access Keys)

Việc cứng hóa (hardcode) thông tin xác thực trực tiếp trong mã nguồn là một trong những sai sót phổ biến nhất trong quản trị hạ tầng đám mây:
```
AWS_ACCESS_KEY="AKIAxxxxxxxx"
AWS_SECRET_KEY="xxxxxxxxxxxxxxxx"
```
Khi mã nguồn chứa khóa truy cập bị đẩy lên các kho lưu trữ công khai (Public Repository), các đối tượng tấn công có thể khai thác ngay lập tức cặp khóa này để chiếm quyền sử dụng tài nguyên AWS. Thực tế ghi nhận nhiều sự cố tài khoản bị lạm dụng để khai thác tiền mã hóa hoặc khởi tạo đồng loạt các EC2 Instance dung lượng lớn, gây thiệt hại chi phí nghiêm trọng chỉ trong thời gian ngắn.

Để loại bỏ triệt để rủi ro rò rỉ thông tin xác thực, AWS khuyến nghị áp dụng các cơ chế quản lý danh tính tiêu chuẩn:

- **IAM Role**
- **Environment Variables (Biến môi trường)**
- **AWS Secrets Manager**
- **AWS Systems Manager Parameter Store**

#### 2.2. Nguyên tắc Cấp quyền Tối thiểu (Least Privilege)

Trong mô hình quản trị truy cập IAM, nguyên tắc cốt lõi là **chỉ cấp phát vừa đủ quyền hạn cần thiết để thực thi tác vụ, không cấp thừa**.

*Ví dụ:* Một EC2 Instance chỉ có nhiệm vụ đọc dữ liệu từ Amazon S3. Thay vì gán chính sách **AmazonS3FullAccess**, cấu hình tối ưu chỉ nên cấp quyền **s3:GetObject** trên đúng bucket mục tiêu. Phương pháp này giới hạn tối đa vùng ảnh hưởng (blast radius) nếu tài khoản hoặc dịch vụ bị xâm nhập — đây cũng là tiêu chuẩn bắt buộc trong thiết kế hệ thống doanh nghiệp.

#### 2.3. Cấu hình Phân vùng Mạng (Subnet Segmentation)

Một sai lầm kiến trúc thường gặp là bố trí toàn bộ dịch vụ vào vùng mạng công cộng (Public Subnet) — thiết lập kết nối trực tiếp từ Internet tới ứng dụng Backend và Cơ sở dữ liệu. Nếu Cơ sở dữ liệu sở hữu IP Public cùng quy tắc Security Group lỏng lẻo, nguy cơ bị rà quét cổng và tấn công mạng sẽ tăng cao.

Mô hình kiến trúc an toàn chuẩn mực đòi hỏi sự phân tách rõ ràng giữa các tầng:

> **Internet → Load Balancer → Backend (Public Subnet) → Database (Private Subnet)**

Theo đó, Cơ sở dữ liệu được cô lập trong Private Subnet và chỉ chấp nhận truy cập nội bộ từ tầng Backend. Đây là mô hình kiến trúc chuẩn mực được khuyến nghị trong **AWS Well-Architected Framework**.

#### 2.4. Phân tầng Bảo vệ Cho Ứng dụng Web

Ngay cả khi mã nguồn được tối ưu, ứng dụng vẫn đối mặt với các hình thức tấn công lớp ứng dụng như SQL Injection, Cross-Site Scripting (XSS), tấn công tự động từ Bot, hoặc DDoS.

**AWS WAF (Web Application Firewall)** đóng vai trò là lớp màng lọc lưu lượng trước khi yêu cầu chạm tới ứng dụng. Dịch vụ cung cấp các khả năng:

- Chặn truy cập từ các địa chỉ IP độc hại
- Giới hạn tần suất yêu cầu (Rate Limiting) theo IP
- Nhận diện và ngăn chặn các dạng mẫu tấn công theo tiêu chuẩn OWASP
- Kiểm soát và loại bỏ các gói tin chứa tải độc (payload)

Nhờ đó, hệ thống vừa giảm tải xử lý cho Backend, vừa nâng cao năng lực tự vệ trước các mối đe dọa từ Internet.

#### 2.5. Giám sát An ninh và Giám sát Hành vi Liên tục

Bảo mật hạ tầng không dừng lại ở cấu hình ban đầu mà yêu cầu **quá trình giám sát liên tục**. AWS cung cấp hệ sinh thái dịch vụ giám sát chuyên sâu:

- **Amazon GuardDuty:** Phân tích dữ liệu nhật ký từ AWS CloudTrail, VPC Flow Logs và DNS Logs nhằm phát hiện các hành vi bất thường — như đăng nhập từ vị trí địa lý lạ, EC2 Instance phát tán lưu lượng bất thường, hay các truy cập mang dấu hiệu tự động (botnet). GuardDuty tự động tổng hợp và đưa ra cảnh báo thời gian thực giúp quản trị viên chủ động xử lý.

- **Amazon Inspector:** Tập trung phát hiện lỗ hổng nội tại trong ứng dụng và hạ tầng. Dịch vụ tự động quét EC2 Instance, Container Image và các thư viện phần mềm nhằm phát hiện các gói phụ thuộc đã hết vòng đời hỗ trợ, các lỗ hổng theo danh mục CVE, hoặc cấu hình an ninh không đạt chuẩn.

- **AWS Security Hub:** Đóng vai trò trung tâm quản lý an ninh tập trung. Dịch vụ hợp nhất kết quả đánh giá từ GuardDuty, Inspector, IAM Access Analyzer, AWS Config và Macie lên một giao diện điều khiển duy nhất, giúp đội ngũ vận hành có cái nhìn toàn diện về trạng thái an ninh của toàn bộ hệ thống.

---

### 3. Giải quyết Các Thách thức Vận hành Thực tế

#### 3.1. Tối ưu Hiệu năng Khi Lưu lượng Truy cập Tăng đột biến

Hiện tượng nghẽn mạng hoặc quá tải hệ thống thường xuất hiện khi lưu lượng người dùng vượt quá năng lực phục vụ của hạ tầng đơn lẻ, dẫn đến tăng độ trễ phản hồi hoặc tràn thời gian chờ (timeout).

Bằng cách kết hợp **Amazon EC2 Auto Scaling** và **Elastic Load Balancer (ELB)**, hệ thống có khả năng tự động khởi tạo thêm các instance tính toán và phân bổ đều tải truy cập. Điều này đảm bảo tính sẵn sàng và duy trì hiệu năng ổn định ngay cả trong các thời điểm lưu lượng tăng cao đột biến.

#### 3.2. Quản lý Trạng thái và Dữ liệu Độc lập (Stateless Architecture)

Lưu trữ dữ liệu tải lên (file uploads, media) trực tiếp trên bộ nhớ cục bộ của máy chủ (ví dụ thư mục **uploads/** trên EC2) tạo ra điểm nghẽn lớn khi mở rộng hoặc thay thế instance, dẫn đến nguy cơ mất an toàn dữ liệu.

Việc ứng dụng **Amazon S3** làm kho lưu trữ đối tượng chuyên biệt cho hình ảnh, tài liệu và các bản sao lưu giúp tách biệt hoàn toàn giữa tầng lưu trữ dữ liệu (Storage) và tầng xử lý logic (Compute). Mô hình này không chỉ đảm bảo an toàn dữ liệu mà còn tối ưu hóa khả năng mở rộng của hệ thống.

---

### 4. Kết luận

Thực tiễn triển khai hạ tầng đám mây khẳng định rằng: **Bảo mật không phải là một tính năng bổ sung sau cùng, mà phải là nguyên tắc cốt lõi trong thiết kế kiến trúc hệ thống ngay từ đầu**. Phần lớn các sự cố an toàn thông tin không xuất phát từ lỗi cú pháp mã nguồn, mà đến từ các sơ hở trong cấu hình và quy trình vận hành hạ tầng.

Các nguyên tắc quản trị cốt lõi cần tuân thủ:

- Tuyệt đối không lưu trữ thông tin xác thực trong mã nguồn
- Thực thi nghiêm ngặt nguyên tắc cấp quyền tối thiểu (Least Privilege)
- Phân tách triệt để không gian mạng Public và Private
- Thiết lập hệ thống giám sát an ninh và ghi log liên tục
- Định kỳ rà quét lỗ hổng và cập nhật các gói phụ thuộc
- Đảm bảo tính chịu lỗi và sẵn sàng năng lực mở rộng cho hạ tầng

Đối với các bài toán thực tế, việc thấu hiểu bản chất và bài toán kỹ thuật mà các dịch vụ bảo mật AWS giải quyết sẽ định hình tư duy thiết kế hệ thống chuẩn mực, an toàn và sẵn sàng cho môi trường vận hành thực tế.

---

**Nhóm tác giả:** Thành Nhân, Nguyễn Cảnh Nguyên, Nguyễn Trọng Nhân, Nam Phan, Nguyễn Bá Nam.

**Link Blog:** [Security in Software Development on AWS](https://www.facebook.com/groups/awsstudygroupfcj/?multi_permalinks=2228803837884576&notif_id=1785383944402087&notif_t=feedback_reaction_generic_tagged)

**Tài liệu tham khảo:**
- [IAM Best Practices (AWS Docs)](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html)
- [Well-Architected – Security – IAM](https://docs.aws.amazon.com/wellarchitected/latest/framework/sec-iam.html)
- [AWS WAF](https://docs.aws.amazon.com/waf/latest/developerguide/what-is-aws-waf.html)
- [Amazon GuardDuty](https://docs.aws.amazon.com/guardduty/latest/ug/what-is-guardduty.html)
- [Amazon Inspector](https://aws.amazon.com/inspector/)