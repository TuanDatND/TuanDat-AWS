---
title: "Target Group"
weight: 5
pre: " <b> 5.5. </b> "
---

# Cấu hình Target Group cho Application Load Balancer

Trong phần này, chúng ta sẽ tạo một **Target Group** (Nhóm mục tiêu) để định tuyến các yêu cầu từ Load Balancer đến các phiên bản EC2 đang chạy ứng dụng backend. Chúng ta cũng sẽ thiết lập cơ chế **Health Check** (Kiểm tra sức khỏe) để đảm bảo Load Balancer chỉ gửi các yêu cầu đến các máy chủ hoạt động bình thường.

### 1. Khởi tạo Target Group

* Đăng nhập vào AWS Management Console và truy cập dịch vụ **EC2**.
* Ở thanh điều hướng bên trái, cuộn xuống phần **Load Balancing** và chọn **Target Groups**.
* Nhấp vào nút **Create target group** để bắt đầu.

![Khởi tạo Target Group - Form trống](/images/5-Workshop/5.5-TargetGroup/Screenshot%202026-07-23%20173205.png)

Tại phần **Basic configuration**:
* **Choose a target type**: Chọn **Instances** (định tuyến lưu lượng truy cập trực tiếp tới các máy ảo EC2).
* **Target group name**: Đặt tên cho Target Group (ví dụ: `aws-c8n`).
* **Protocol**: Chọn `HTTP`.
* **Port**: Nhập `80` (Cổng mặc định mà Target Group sẽ dùng để nhận lưu lượng từ Load Balancer).
* **VPC**: Đảm bảo chọn đúng VPC nơi các phiên bản EC2 của bạn đang chạy (ví dụ: `vpc-044c12820d72fc502`).
* **Protocol version**: Chọn **HTTP1**.

![Khởi tạo Target Group - Cấu hình cơ bản](/images/5-Workshop/5.5-TargetGroup/Screenshot%202026-07-28%20101935.png)

### 2. Cấu hình Health Check

Cuộn xuống phần **Health checks**. Đây là một bước quan trọng để Load Balancer theo dõi trạng thái ứng dụng của bạn.
* **Health check protocol**: Chọn `HTTP`.
* **Health check path**: Nhập đường dẫn API dùng để kiểm tra trạng thái hoạt động của backend. Thay vì đường dẫn gốc mặc định `/`, hãy nhập đường dẫn tùy chỉnh của dự án: `/api/health`.

![Cấu hình Health Check](/images/5-Workshop/5.5-TargetGroup/Screenshot%202026-07-28%20102528.png)

Nhấn **Next** để chuyển sang bước đăng ký các mục tiêu.

### 3. Đăng ký Mục tiêu (Register Targets)

Tại bước **Register targets**, hệ thống sẽ liệt kê các phiên bản EC2 đang chạy trong VPC đã chọn.
* Tích chọn phiên bản EC2 bạn muốn thêm vào Target Group (ví dụ: Instance có tên `CenFra-MS`).
* Tại ô **Ports for the selected instances**, nhập cổng mà ứng dụng backend thực tế đang chạy trên EC2 (ví dụ: `8080`).
* Nhấn nút **Include as pending below**. Phiên bản sẽ xuất hiện ở bảng Review targets phía dưới với trạng thái *Pending*.

![Đăng ký Mục tiêu](/images/5-Workshop/5.5-TargetGroup/Screenshot%202026-07-28%20102707.png)

### 4. Kiểm tra lại và khởi tạo (Review and create)

Tại bước cuối cùng, hãy xem lại tất cả các thông số cấu hình:
* Target type: `Instance`
* Protocol:Port: `HTTP: 80`
* Health check path: `/api/health`
* Đảm bảo instance được gán đúng cổng dịch vụ `8080`.

Sau khi xác nhận các thông tin đã chính xác, nhấn nút **Create target group** ở góc dưới cùng.

![Kiểm tra và Khởi tạo Target Group](/images/5-Workshop/5.5-TargetGroup/Screenshot%202026-07-28%20102733.png)

---
*(Lưu ý: Hình ảnh cuối cùng `Screenshot 2026-07-28 102832.png` hiển thị màn hình Load Balancers trống, cho thấy bước tiếp theo của đội ngũ sẽ là tạo một Load Balancer và liên kết Target Group vừa tạo này vào đó).*
