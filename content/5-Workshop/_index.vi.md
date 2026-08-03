---
title: "Workshop"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5. </b> "
---
{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}}

# Kết nối bảo mật Hybrid tới S3 qua VPC Endpoints & Triển khai CenFra-MS

#### Tổng quan

**AWS PrivateLink** cung cấp kết nối riêng tư đến các dịch vụ AWS từ VPC và mạng on-premises của bạn mà không để lộ lưu lượng truy cập ra Public Internet.

Trong bài lab này, bạn sẽ học cách khởi tạo, cấu hình và kiểm thử các VPC Endpoint cho phép tải tài nguyên truy cập các dịch vụ AWS mà không đi qua mạng internet công cộng.

Thêm vào đó, bạn sẽ triển khai một ứng dụng backend Spring Boot đã container hóa (**CenFra-MS**) trên một hạ tầng AWS chuẩn production sử dụng EC2, Application Load Balancer (ALB), Route 53 DNS, CloudFront CDN và giám sát logs bằng CloudWatch.

#### Nội dung chính

1. [Tổng quan về workshop](5.1-workshop-overview)
2. [Các bước chuẩn bị](5.2-prerequiste/)
3. [Truy cập S3 từ VPC](5.3-s3-vpc/)
4. [Truy cập S3 từ On-premises](5.4-s3-onprem/)
5. [Chính sách bảo mật VPC Endpoint](5.5-policy/)
6. [Dọn dẹp tài nguyên (VPC Endpoint)](5.6-cleanup/)
7. [Triển khai EC2](5.7-ec2/)
8. [Application Load Balancer](5.8-load-balancer/)
9. [Tích hợp Route 53 & CloudFront CDN](5.9-route53-cloudfront/)
10. [Amazon CloudWatch Logs & Giám sát](5.10-cloudwatch/)
11. [Dọn dẹp tài nguyên CenFra-MS](5.11-cleanup/)
