---
title: "Blog 3"
date: 2026-07-10
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---

# QUẢN LÝ HẠ TẦNG VỚI TERRAFORM — KHÔNG CHỈ LÀ CLICK ON THE CONSOLE

### 1. Lời mở đầu

Trong quá trình học và làm dự án trên Cloud, một hành trình rất quen thuộc mà hầu như ai mới bắt đầu cũng từng trải qua: Ban đầu, để tạo một hệ thống gồm VPC, EC2, RDS, S3..., chúng ta thường đăng nhập vào AWS Management Console rồi click từng nút một. Cách này nhanh và trực quan khi mới làm quen.

Tuy nhiên, khi dự án lớn lên, câu hỏi bắt đầu xuất hiện: **Làm sao để nhân bản toàn bộ hệ thống này sang môi trường Staging hay Production mà không bấm nhầm?** Nếu ai đó vô tình sửa sai một Security Group trên Console thì làm sao khôi phục lại?

Đó là lúc nhóm tìm đến **Terraform** và tư duy **Infrastructure as Code (IaC)** — quản lý toàn bộ hạ tầng đám mây bằng mã nguồn thay vì thao tác thủ công bằng tay.

---

### 2. 5 Bài học Quản lý Hạ tầng Cốt lõi với Terraform

#### 2.1. Infrastructure as Code (IaC) – Biến hạ tầng thành file mã nguồn

Thay vì phải nhớ từng bước click chuột, Terraform cho phép định nghĩa toàn bộ tài nguyên (Server, Network, Database, Firewall) dưới dạng các file cấu hình bằng ngôn ngữ HCL (HashiCorp Configuration Language).

Ví dụ đơn giản để khởi tạo một EC2 Instance:
```hcl
resource "aws_instance" "web_server" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t3.micro"

  tags = {
    Name = "WebServer-Dev"
  }
}
```

Nhờ viết thành code, toàn bộ hạ tầng có thể được lưu trữ trên Git, kiểm tra lịch sử thay đổi (version control), review code trước khi triển khai và tái sử dụng dễ dàng ở bất kỳ đâu.

#### 2.2. Quy trình "Plan" và "Apply" – Tránh những bất ngờ trên Production

Một trong những điểm tối ưu nhất ở Terraform là cơ chế xem trước thay đổi trước khi thực thi thực sự. Quy trình chuẩn thường gồm 3 bước:

- **terraform init:** Khởi tạo môi trường, tải các provider (AWS, Azure, GCP...) cần thiết.
- **terraform plan:** So sánh code hiện tại với hạ tầng thực tế và hiển thị chính xác những gì sẽ được Tạo mới (+), Sửa đổi (~), hoặc Xóa bỏ (-).
- **terraform apply:** Chỉ khi người quản trị xác nhận yes, Terraform mới gửi lệnh gọi API lên AWS để tạo đúng những gì đã khai báo.

Tính năng plan giúp giảm thiểu tối đa rủi ro xóa nhầm Database hay cấu hình sai hạ tầng quan trọng — rủi ro rất dễ xảy ra khi thao tác bằng tay.

#### 2.3. Quản lý Trạng thái Hạ tầng An toàn với terraform.tfstate

Khi làm việc theo nhóm, bài toán lớn nhất là: **Làm sao Terraform biết tài nguyên nào đã tạo rồi, tài nguyên nào chưa?**

Terraform sử dụng một file có tên là terraform.tfstate để lưu trữ trạng thái hiện tại của hạ tầng. Khi mới tiếp cận, một sai lầm phổ biến là lưu file **.tfstate** này ngay trên máy local hoặc commit lên Git repo:

- **Xung đột dữ liệu:** Nếu 2 người cùng apply một lúc, dữ liệu state sẽ bị xung đột.
- **Rò rỉ bảo mật:** File state chứa các thông tin nhạy cảm (như mật khẩu DB, token), tiềm ẩn nguy cơ rò rỉ nếu push lên kho lưu trữ công khai.

**Giải pháp chuẩn:** Sử dụng **Remote Backend** (lưu file state trên Amazon S3) kết hợp với **DynamoDB Table** để khóa trạng thái (State Locking). Khi một thành viên đang triển khai, DynamoDB sẽ khóa file state lại để đảm bảo không ai khác can thiệp cùng lúc.

#### 2.4. Tái sử dụng Code với Terraform Modules

Khi dự án mở rộng, việc viết tất cả tài nguyên vào một file **main.tf** duy nhất sẽ khiến code trở nên đồ sộ và cực kỳ khó quản lý.

Bằng cách đóng gói các tài nguyên liên quan thành các **Module** (ví dụ: Module VPC, Module RDS, Module EKS), nhóm có thể dễ dàng gọi lại cấu hình đó cho nhiều môi trường khác nhau:
```hcl
module "vpc_dev" {
  source     = "./modules/vpc"
  cidr_block = "10.0.0.0/16"
  env        = "dev"
}

module "vpc_prod" {
  source     = "./modules/vpc"
  cidr_block = "10.1.0.0/16"
  env        = "prod"
}
```

Nhờ vậy, môi trường Production và Development đảm bảo tính đồng nhất 100% về mặt kiến trúc, chỉ khác nhau về quy mô tài nguyên cấu hình.

#### 2.5. Quản lý "Drift" – Khi hạ tầng bị sửa lén trên Console

Trong thực tế, thỉnh thoảng sẽ có nhân sự tự ý lên AWS Console để sửa một Security Group hoặc đổi dung lượng Instance mà không thông qua code. Hiện tượng này gọi là **Infrastructure Drift**.

Khi chạy lại **terraform plan**, Terraform sẽ quét toàn bộ hệ thống thực tế, phát hiện sự sai lệch giữa Code và Thực tế, sau đó tự động đề xuất phương án đưa hạ tầng về đúng trạng thái đã được định nghĩa trong code. Điều này giúp đảm bảo hạ tầng luôn được kiểm soát tập trung (**Single Source of Truth**).

---

### 3. Giải quyết Các Thách thức Vận hành Thực tế

#### 3.1. Quản lý Trạng thái Tập trung và Khóa Trạng thái (State Locking)

Trong môi trường làm việc nhóm, việc duy trì tính nhất quán của file state là yếu tố sống còn. Bằng cách thiết lập Amazon S3 làm Remote Backend kết hợp DynamoDB cho State Locking, hệ thống ngăn chặn hoàn toàn tình trạng ghi đè state song song và bảo vệ dữ liệu nhạy cảm thông qua cơ chế mã hóa S3.

#### 3.2. Chuẩn hóa Kiến trúc Đa Môi trường bằng Modules

Việc duy trì tính đồng nhất giữa môi trường Dev, Staging và Prod thường gặp khó khăn do thao tác thủ công dễ dẫn đến sai lệch cấu hình. Đóng gói kiến trúc hạ tầng thành các Terraform Module giúp tái sử dụng mã nguồn chuẩn hóa, rút ngắn thời gian triển khai môi trường mới từ nhiều giờ xuống chỉ vài phút.

---

### 4. Kết luận

Thực tiễn triển khai hạ tầng đám mây khẳng định rằng: **Hạ tầng không chỉ là phần cứng hay dịch vụ đám mây, hạ tầng cũng là Code.**

Các nguyên tắc quản trị cốt lõi cần tuân thủ:

- Tuyệt đối không thao tác thủ công trên Console sau khi đã quản lý bằng Terraform
- Quản lý State file cẩn thận: Luôn dùng Remote Backend (S3 + DynamoDB) và mã hóa file state
- Áp dụng CI/CD cho hạ tầng: Đưa bước **terraform plan** vào Pull Request để toàn đội cùng review trước khi merge và apply
- Viết code theo dạng Module để dễ bảo trì, tái sử dụng và mở rộng về sau

Chuyển từ tư duy "ClickOps" sang "Infrastructure as Code" đòi hỏi thời gian đầu để làm quen với cú pháp và công cụ. Tuy nhiên, khi hệ thống phát triển lớn hơn, đây là kỹ năng bắt buộc để xây dựng hạ tầng chuẩn hóa, tin cậy và chuyên nghiệp.

---

**Nhóm tác giả:** Thành Nhân, Nguyễn Cảnh Nguyên, Nguyễn Trọng Nhân, Nam Phan, Nguyễn Bá Nam.

**Link Blog:** [Manage Infrastructure with Terraform — Beyond Clicking on the Console](https://www.facebook.com/groups/awsstudygroupfcj/posts/2229938421104451?notif_id=1785478381072886&notif_t=tagged_with_story&ref=notif)

**Tài liệu tham khảo:**
- [Terraform AWS Provider Docs](https://registry.terraform.io/providers/hashicorp/aws/latest/docs)
- [Terraform Recommended Best Practices](https://developer.hashicorp.com/terraform/tutorials)
- [AWS Backend S3 & DynamoDB](https://developer.hashicorp.com/terraform/language/settings/backends/s3)
