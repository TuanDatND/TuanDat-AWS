---
title: "Blog 2"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---

# Kiro Steering Files: Làm sao để AI luôn tuân thủ Security Standards trên AWS?

Gần đây mình đọc được một bài khá thú vị trên **AWS Security Blog** về cách sử dụng **Kiro** và **Amazon Q** để tăng cường bảo mật trên AWS. Mình đặc biệt chú ý đến:

* **Persistent Security Context thông qua Steering Files.**

Thoạt nghe có vẻ không quá đặc biệt. Nhưng càng đọc mình càng thấy đây có thể là một trong những cách thực tế nhất để giải quyết vấn đề mà hầu hết chúng ta đều gặp khi sử dụng AI Assistant mỗi ngày.

AI đang thay đổi cách chúng ta vận hành hệ thống Cloud. Từ việc tạo CloudFormation Template, viết IAM Policy, phân tích Security Hub Findings cho đến review Infrastructure as Code (IaC), các công cụ như Kiro hay Amazon Q đang giúp Cloud Engineer và Security Engineer tiết kiệm rất nhiều thời gian. Nhưng có một vấn đề lớn: **AI không hiểu tiêu chuẩn bảo mật của tổ chức bạn.**

---

### Một tình huống rất thực tế

Giả sử bạn yêu cầu AI: *“Create an S3 bucket for storing customer documents.”*

AI hoàn toàn có thể tạo được bucket. Nhưng liệu nó có biết rằng tổ chức của bạn yêu cầu:
* Bắt buộc bật Server-Side Encryption
* Chặn Public Access
* Bật Versioning
* Bật Access Logging
* Chỉ cho phép truy cập từ VPC Endpoint?

Có thể có, có thể không. Và đó chính là rủi ro.

---

### Vấn đề của AI trong môi trường Enterprise

Trong môi trường doanh nghiệp, việc triển khai tài nguyên AWS không chỉ cần hoạt động. Nó còn phải tuân thủ:
* **Security Standards** (Tiêu chuẩn bảo mật)
* **Compliance Requirements** (Yêu cầu tuân thủ)
* **Internal Policies** (Chính sách nội bộ)
* **Governance Rules** (Quy tắc quản trị)

Ví dụ: Một công ty có thể quy định:
* Không được tạo IAM User (Chỉ sử dụng IAM Role)
* Không được cấp quyền `AdministratorAccess`
* Mọi dữ liệu phải được mã hóa mặc định
* CloudTrail phải luôn được bật

Nếu AI không biết những quy tắc này, nó có thể tạo ra hạ tầng hoạt động tốt nhưng không đạt yêu cầu bảo mật.

---

### Giải pháp từ AWS: Kiro Steering Files

Trong bài viết gần đây, AWS giới thiệu việc sử dụng **Kiro Steering Files** để cung cấp ngữ cảnh bảo mật lâu dài cho AI. Ý tưởng khá đơn giản: thay vì mỗi lần đều phải nhắc lại các yêu cầu bảo mật trong từng câu lệnh (prompt), chúng ta định nghĩa chúng thành một tập quy tắc cố định (Steering File) để AI tham chiếu trong suốt quá trình làm việc.

#### Ví dụ: Cấu hình Steering File về Security Standards trên AWS

```markdown
# AWS Security Standards

## IAM
- Apply least privilege principle
- Never attach AdministratorAccess policy

## S3
- Enable encryption by default
- Block all public access
- Enable versioning

## Logging
- CloudTrail must be enabled
- Enable AWS Config

## Networking
- Restrict inbound traffic
- Avoid 0.0.0.0/0 unless justified

## Compliance
- Follow CIS AWS Foundations Benchmark
```

Khi có Steering File, nếu bạn yêu cầu: *“Create an S3 bucket for application logs.”*

Thay vì chỉ tạo một tài nguyên bucket cơ bản, AI sẽ tự động bổ sung cấu hình mã hóa (Encryption), phiên bản (Versioning), Bucket Policy và chặn Public Access ngay từ đầu. Điều này giúp giảm số lượng lỗi cấu hình (misconfiguration) — một trong những nguyên nhân phổ biến nhất dẫn đến sự cố bảo mật trên Cloud.

---

### Sơ đồ minh họa khái niệm

![Minh họa khái niệm Kiro Steering Files](/images/3-BlogsPosted/3.2-Blog2/kiro-steering-concept.png)
*Hình 1: AI Assistant tham chiếu Steering File để tạo cấu hình hạ tầng (IaC) bảo mật ngay từ đầu.*

---

### Đặc biệt hữu ích với môi trường Multi-Account AWS

Nhiều doanh nghiệp hiện nay vận hành nhiều tài khoản AWS riêng biệt cho Development, Staging, Production, Security và Logging. Steering Files giúp AI hiểu rõ:
* Cách tổ chức quản lý AWS Accounts
* Mô hình quản lý phân quyền IAM
* Các giới hạn kiểm soát bảo mật SCP (Service Control Policies)
* Các yêu cầu ghi nhật ký (Logging Requirements)

Từ đó, các đề xuất của AI sẽ sát thực tế và phù hợp hơn với cấu trúc của tổ chức.

---

### Kết luận

Steering Files không giải quyết toàn bộ bài toán bảo mật, nhưng nó giúp biến các nguyên tắc bảo mật của tổ chức thành một phần ngữ cảnh mà AI luôn ghi nhớ. Và trong môi trường Cloud, đôi khi một cấu hình đúng ngay từ đầu có giá trị hơn rất nhiều so với việc đi khắc phục và vá lỗi sau này.

---

* **Link bài gốc tham khảo**: [AWS Security Blog](https://aws.amazon.com/blogs/security/five-ways-to-use-kiro-and-amazon-q-to-strengthen-your-security-posture/)
* **Link bài đăng Facebook**: [AWS Study Group Facebook Post](https://www.facebook.com/photo/?fbid=2034832500720323&set=gm.2176154606482833&idorvanity=660548818043427)