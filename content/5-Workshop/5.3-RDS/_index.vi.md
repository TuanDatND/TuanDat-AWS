---
title: "Cơ sở dữ liệu RDS"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 5.3. </b> "
---

### Khởi tạo cơ sở dữ liệu Amazon RDS PostgreSQL

Trong mục này, bạn sẽ cấu hình và khởi tạo một cơ sở dữ liệu Amazon RDS PostgreSQL có tên là `aws-c8n`. Cơ sở dữ liệu này sẽ được sử dụng để lưu trữ dữ liệu quan hệ cho ứng dụng backend. Bạn sẽ thiết lập các quy tắc bảo mật mạng, kích hoạt xác thực kết hợp Password và IAM, và kiểm tra kết nối.

Các mục tiêu chính trong phần này bao gồm:
* Khởi tạo cơ sở dữ liệu PostgreSQL sử dụng gói miễn phí AWS Free Tier.
* Cấu hình thông tin đăng nhập và kích hoạt cả xác thực bằng password truyền thống lẫn xác thực bằng quyền IAM.
* Ánh xạ kết nối đến một nhóm bảo mật chuyên dụng cho phép truy cập an toàn.
* Tải xuống gói chứng chỉ bảo mật SSL/TLS toàn cầu của AWS và xác minh kết nối.

---

### Bước 1: Khởi tạo Cơ sở dữ liệu PostgreSQL

1. Truy cập vào **Amazon RDS console** và nhấn **Create database**.
2. Chọn phương pháp tạo cơ sở dữ liệu là **Standard create** (hoặc **Full configuration**).
3. Tại mục **Engine options**, chọn **PostgreSQL**.
4. Tại mục **Templates**, chọn **Free tier** để tránh phát sinh chi phí không mong muốn.
5. Tại mục **Availability and durability**, chọn **Single-AZ DB instance deployment** (cấu hình mặc định cho gói Free Tier).

![Khởi tạo PostgreSQL Database](/images/5-Workshop/5.3-RDS/01-create-db-postgres.png)

---

### Bước 2: Cấu hình Tên định danh và Thông tin đăng nhập

1. Đặt **DB instance identifier** thành `aws-c8n`.
2. Đặt **Master username** thành `postgres`.
3. Tại mục **Credentials management**, chọn **Self managed**.
4. Nhập mật khẩu quản trị **Master password** mạnh và xác nhận lại mật khẩu.
5. Tại mục **Database authentication options**, chọn **Password and IAM database authentication** (xác thực bằng password kết hợp IAM).
6. Tại mục **Instance configuration**, chọn các lớp cấu hình burstable (ví dụ: `db.t4g.micro` hoặc `db.t3.micro` tùy thuộc vào khu vực).

![Thông tin đăng nhập Database](/images/5-Workshop/5.3-RDS/02-db-settings.png)
![Lớp Instance và Xác thực](/images/5-Workshop/5.3-RDS/03-db-auth-instance.png)

---

### Bước 3: Cấu hình Kết nối & Quy tắc Nhóm bảo mật Security Group

Sau khi cơ sở dữ liệu khởi tạo thành công, truy cập vào trang chi tiết cơ sở dữ liệu để lấy endpoint kết nối và cấu hình quy tắc bảo mật:

1. Lấy thông tin **Endpoint**: `aws-c8n.czsenqug2xnh.us-west-2.rds.amazonaws.com` chạy trên cổng mặc định `5432`.
2. Tại tab **Connectivity & security**, xác minh tính năng **IAM Authentication** đã được bật hiển thị **Enabled**.
3. Tại mục **Security group rules**, cấu hình nhóm bảo mật được gán `rds-ec2-1` cho phép:
   * Lưu lượng truy cập PostgreSQL (cổng 5432) đi vào từ IP của quản trị viên (ví dụ: `14.169.24.57/32`).
   * Lưu lượng truy cập PostgreSQL (cổng 5432) đi vào từ nhóm bảo mật của EC2 (`sg-0c7a938ee4252d8e7` / `ec2-rds-1`) để ứng dụng Spring Boot trên EC2 có thể kết nối.

![Endpoint và Trạng thái Kết nối](/images/5-Workshop/5.3-RDS/04-db-connectivity.png)
![Quy tắc Nhóm bảo mật](/images/5-Workshop/5.3-RDS/05-db-security-groups.png)

---

### Bước 4: Kiểm tra kết nối Cơ sở dữ liệu bằng psql

Bạn có thể kiểm tra kết nối tới cơ sở dữ liệu PostgreSQL từ terminal thông qua các bước sau:

1. Tải xuống tệp chứng chỉ bảo mật SSL toàn cầu của AWS RDS:
   ```bash
   curl -o global-bundle.pem https://truststore.pki.rds.amazonaws.com/global/global-bundle.pem
   ```
2. Thiết lập biến môi trường cho host endpoint:
   ```bash
   export RDSHOST="aws-c8n.czsenqug2xnh.us-west-2.rds.amazonaws.com"
   ```
3. Tạo token xác thực tạm thời bằng quyền IAM và kết nối bằng `psql`:
   ```bash
   psql "host=$RDSHOST port=5432 dbname=postgres user=postgres sslmode=verify-full sslrootcert=./global-bundle.pem password=$(aws rds generate-db-auth-token --hostname $RDSHOST --port 5432 --username postgres --region us-west-2)"
   ```
