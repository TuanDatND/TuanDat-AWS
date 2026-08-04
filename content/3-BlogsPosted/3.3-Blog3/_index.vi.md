---
title: "Blog 3"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---

# AWS Security Blog Deep Dive | Bóc tách kiến trúc và dữ liệu DDoS với Flow Logs trong AWS Shield Advanced

Xin chào mọi người,

Trong lĩnh vực An toàn thông tin, việc bảo vệ các Critical Assets (tài sản trọng yếu) trước các cuộc tấn công DDoS không chỉ dừng lại ở việc thiết lập một "Firewall" vững chắc rồi phó mặc cho hệ thống tự động xử lý. Để thực sự làm chủ hệ thống, chúng ta cần dữ liệu chi tiết (visibility) ở mức độ gói tin (packet-level) để phân tích hành vi kẻ tấn công.

Bài viết *"Gain visibility into DDoS attacks with flow logs in AWS Shield Advanced"* của Ken Kitts cung cấp một cái nhìn rất sâu về cách AWS giải quyết bài toán này. Dưới đây là phần phân tích lại dưới góc độ kỹ thuật chi tiết hơn, tập trung vào kiến trúc hạ tầng và luồng dữ liệu.

---

### 1. Bản chất của Flow Logs trong quá trình Mitigation

Khi một cuộc tấn công DDoS ở tầng hạ tầng (infrastructure-layer) xảy ra, mục tiêu của hacker là làm bão hòa băng thông (saturate bandwidth) hoặc làm cạn kiệt bảng trạng thái kết nối (exhaust connection tables), dẫn đến tình trạng rớt gói tin (packet loss) và timeout.

Thay vì chỉ cung cấp CloudWatch metrics tổng hợp (vốn thiếu chi tiết) hay buộc kỹ sư phải chắp vá dữ liệu sau sự cố, AWS Shield Advanced sinh ra tính năng **attack flow logs**.

* **Cơ chế ghi**: Log được ghi nhận và xuất ra liên tục theo chu kỳ 5 phút/lần, hoạt động ngay cả khi cuộc tấn công đang diễn ra và sau khi nó kết thúc.
* **Giới hạn tệp**: Dung lượng tối đa cho mỗi file log là 75 MB. Nếu hệ thống bị flood data và đạt mức 75MB trong chưa đầy 5 phút, AWS sẽ tự động đóng file, publish nó và mở một file log mới.
* **Định dạng hỗ trợ**: Rất linh hoạt cho các pipeline xử lý data, bao gồm JSON, plain text, W3C, và Parquet.
* **Lưu ý**: Ở thời điểm hiện tại, Shield Advanced hỗ trợ flow logs cho các mục tiêu được bảo vệ bằng Elastic IP (EIP), các tài nguyên khác (CloudFront, ALB, Route 53) sẽ được mở rộng hỗ trợ sau.

---

### 2. Cấu trúc của một Record Log (Log Schema)

Dữ liệu metadata được AWS bóc tách cực kỳ kỹ lưỡng. Những dữ liệu dạng JSON này hoàn toàn có thể được kéo về thông qua API để render lên các custom dashboard do anh em tự build bằng HTML/CSS, giúp trực quan hóa bức tranh toàn cảnh của hệ thống mạng theo thời gian thực.

Một record sẽ chứa các trường (fields) trọng yếu sau:
* `protection_arn`: Mã định danh ARN của lớp bảo vệ Shield.
* `srcaddr` / `dstaddr` & `srcport` / `dstport`: IP và Port của cả nguồn tấn công lẫn đích đến.
* `protocol` & `tcp_flags`: Loại giao thức (TCP, UDP, ICMP) và các cờ TCP (như SYN, ACK) giúp xác định chính xác kỹ thuật flood đang được sử dụng (ví dụ: SYN Flood).
* `packets` / `bytes`: Khối lượng truy cập thực tế trong một aggregation window (khung thời gian gom nhóm).
* `action`: Trường quan trọng nhất để verify mitigation – cho biết Shield đã **Block** hay **Allow** luồng traffic này.
* `location` & `srccountry`: AWS edge location mà traffic đi vào và mã quốc gia 2 chữ cái của nguồn tấn công.

---

### 3. Sơ đồ kiến trúc phân phối Log

![Kiến trúc phân phối AWS Shield Advanced Flow Logs](/images/3-BlogsPosted/3.3-Blog3/shield-flowlogs-architecture.png)
*Hình 1: Sơ đồ kiến trúc tham chiếu cách bóc tách và phân phối dữ liệu luồng DDoS của AWS Shield Advanced.*

---

### 4. Mô hình Kiến trúc Phân phối Log (Delivery Architecture)

AWS thiết kế luồng xuất log theo mô hình 3 thực thể độc lập. Điều này giúp kiến trúc cực kỳ linh hoạt (decoupled), cho phép tái sử dụng (reuse) các điểm lưu trữ log cho nhiều tài nguyên khác nhau trên các Account hoặc Region khác nhau.

* **DeliverySource** (Nguồn phát): Đại diện cho ARN của lớp bảo vệ Shield Advanced (không phải ARN của bản thân EC2 hay ALB).
* **DeliveryDestination** (Đích đến): Đích lưu trữ cuối cùng, có thể là Amazon S3 (phục vụ truy vấn Athena), CloudWatch Logs log group (phục vụ CloudWatch Insights realtime), hoặc Amazon Data Firehose (stream trực tiếp ra SIEM thứ 3).
* **Delivery** (Kênh truyền): Sợi dây liên kết logic ghép Source và Destination lại với nhau.

---

### 5. Luồng triển khai kỹ thuật qua AWS CLI

Để thiết lập cấu trúc trên, kỹ sư DevOps cần có quyền IAM tương ứng (`logs:PutDeliverySource`, `logs:PutDeliveryDestination`, `logs:CreateDelivery`). Dưới đây là luồng CLI cụ thể:

#### Bước 1: Lấy ARN của Shield Protection (Nằm ở Region us-east-1 vì Shield là Global Service)
```bash
aws shield list-protections --region us-east-1
```

#### Bước 2: Khởi tạo Delivery Source
```bash
aws logs put-delivery-source \
--name my-shield-delivery-source \
--resource-arn <protection-arn> \
--log-type FLOW_LOGS \
--region us-east-1
```

#### Bước 3: Khởi tạo Delivery Destination (Cấu hình trỏ về ARN của S3, Log Group hoặc Firehose)
```bash
aws logs put-delivery-destination \
--name my-shield-delivery-destination \
--output-format plain \
--delivery-destination-configuration '{"destinationResourceArn":"<resource-arn>"}' \
--region us-east-1
```

#### Bước 4: Map Source và Destination (Create Delivery)
Sử dụng ID của đích đến được sinh ra ở Bước 3.
```bash
aws logs create-delivery \
--delivery-source-name my-shield-delivery-source \
--delivery-destination-arn <delivery-destination-arn> \
--region us-east-1
```

---

### Tổng kết

Điểm sáng giá nhất của AWS Shield Advanced Flow Logs là khả năng tương thích chéo (cross-account & cross-region centralization rules). Chúng ta có thể cấu hình gom toàn bộ log từ nhiều tài khoản AWS khác nhau về một S3 bucket trung tâm, sau đó dùng Athena + Amazon QuickSight (hoặc custom web portal) để phân tích tổng thể.

Việc không phải can thiệp vào tầng Application, không phải cài cắm Agent mà vẫn lấy được log chi tiết tới từng `tcp_flags` ở tốc độ near-realtime chính là sức mạnh của Cloud-native Security.

---

* **Nguồn tham khảo**: [AWS Security Blog](https://aws.amazon.com/vi/blogs/security/gain-visibility-into-ddos-attacks-with-flow-logs-in-aws-shield-advanced/)
* **Link bài đăng Facebook**: [AWS Study Group Facebook Post](https://www.facebook.com/groups/awsstudygroupfcj/posts/2211111629653797?locale=vi_VN)