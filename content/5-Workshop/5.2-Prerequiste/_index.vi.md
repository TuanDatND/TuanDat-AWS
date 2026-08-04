---
title : "Các bước chuẩn bị"
date : 2024-01-01 
weight : 2 
chapter : false
pre : " <b> 5.2. </b> "
---

Trước khi bắt đầu bài thực hành triển khai ứng dụng, hãy đảm bảo bạn đã hoàn thành các bước chuẩn bị sau:

#### 1. Tài khoản AWS & Quyền hạn
* Một **tài khoản AWS** đang hoạt động.
* Một IAM user hoặc role có đủ quyền tạo máy ảo EC2, bộ cân bằng tải ALB, bản ghi DNS Route 53, phân phối CloudFront CDN và tạo CloudWatch Log Groups.

#### 2. Hạ tầng mạng (VPC)
* Một **VPC** được thiết lập sẵn:
  * Có tối thiểu hai **Public Subnet** nằm ở các Availability Zone khác nhau để phục vụ cho Application Load Balancer.
  * Đã đính kèm **Internet Gateway** (IGW) vào VPC và cấu hình bảng định tuyến (Route Table) trỏ ra ngoài Internet.

#### 3. Tên miền tùy chỉnh
* Một tên miền đang hoạt động và được quản lý trên **Amazon Route 53** (ví dụ: `tuandat.space`).
* Đầy đủ quyền quản trị để tạo và chỉnh sửa các bản ghi tên miền.

#### 4. Container Image ứng dụng
* Mã nguồn ứng dụng **CenFra-MS** đã được đóng gói thành Docker image và đẩy lên Docker Hub (ví dụ: `tuandat/cenframs-backend:latest`) hoặc AWS ECR.
* Chuẩn bị sẵn file `docker-compose.yml` để kéo (pull) image và chạy container ứng dụng.