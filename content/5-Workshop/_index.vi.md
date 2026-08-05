---
title: "Workshop"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Triển khai ứng dụng Spring Boot chuẩn Production trên AWS

#### Tổng quan

Trong bài thực hành này, bạn sẽ học cách triển khai một ứng dụng backend Spring Boot đã đóng gói container (**CenFra-MS**) trên một hạ tầng đám mây AWS bảo mật, có khả năng giám sát tập trung và có tính sẵn sàng cao.

Kiến trúc bao gồm chạy ứng dụng trên máy ảo EC2, sử dụng target group để định tuyến lưu lượng qua bộ cân bằng tải ALB, tích hợp tên miền tùy chỉnh trong Route 53 với chứng chỉ SSL được cấp phát bởi AWS Certificate Manager (ACM), phân phối nội dung qua CloudFront CDN và gom logs tập trung về Amazon CloudWatch.

#### Nội dung chính

1. [Tổng quan & Sơ đồ kiến trúc](5.1-workshop-overview/)
2. [Các bước chuẩn bị](5.2-prerequiste/)
3. [Cơ sở dữ liệu RDS](5.3-rds/)
4. [Triển khai EC2](5.4-ec2/)
5. [Target Group](5.5-targetgroup/)
6. [Application Load Balancer](5.6-load-balancer/)
7. [Tích hợp Route 53 & CloudFront CDN](5.7-route53-cloudfront/)
8. [Amazon CloudWatch Logs & Giám sát](5.8-cloudwatch/)
9. [Dọn dẹp tài nguyên CenFra-MS](5.9-cleanup/)
