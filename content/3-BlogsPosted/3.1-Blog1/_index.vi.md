---
title: "Blog 1"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# [Góc Kỹ Thuật] Xây dựng REST API Multi-Region với Aurora DSQL: Cách xử lý đụng độ dữ liệu và quản lý credential

Chào anh chị và các bạn, gần đây khi làm hệ thống, mình có đọc một bài khá hay trên **AWS Database Blog** về việc kết hợp **Spring Boot** và **Amazon Aurora DSQL**. Bài toán đặt ra là: *Làm sao để chạy API ở nhiều khu vực (Multi-Region Active-Active) mà không gặp lỗi đồng bộ database?*

Các bạn làm Backend chắc đều biết vấn đề khi 2 request từ 2 Region khác nhau cùng update một dòng dữ liệu (ví dụ: trừ số lượng tồn kho). Nếu dùng Lock (khóa) truyền thống thì dễ sinh ra nghẽn connection, thậm chí Deadlock. Chưa kể việc đồng bộ mật khẩu DB giữa các node cũng phức tạp.

Việc kết hợp **Aurora DSQL** và **Spring Boot** có thể giải quyết bài toán này qua 3 điểm chính:

1. **Không cần Password tĩnh**: Bài viết dùng `DSQLConnector`. Công cụ này tự động xác thực bằng IAM Role, tự refresh token và mã hóa TLS. Dev không cần lưu hardcode password hay quản lý secret rắc rối nữa.
2. **Xử lý đụng độ bằng Optimistic Concurrency Control (OCC)**: DSQL không dùng Lock. Khi có conflict xảy ra lúc commit, 1 request sẽ thành công, request còn lại nhận lỗi `40001` SQL state.
3. **Kết hợp Spring Boot và HikariCP**: Thay vì báo lỗi `500` cho người dùng, chúng ta có thể xử lý ngầm như sau:
   * Cấu hình `DsqlExceptionOverride` báo cho **HikariCP** giữ lại connection khi gặp lỗi `40001` vì nó vẫn đang hoạt động tốt.
   * Dùng `@Retryable` của Spring bắt lỗi và tự động thử lại (retry) với độ trễ tăng dần (**Exponential backoff**). Kết quả là client vẫn nhận về HTTP `200 OK`.

---

### Sơ đồ kiến trúc Multi-Region

![Sơ đồ kiến trúc Aurora DSQL Multi-Region](/images/3-BlogsPosted/3.1-Blog1/aurora-dsql-architecture.png)
*Hình 1: Triển khai Spring Boot hoạt động song song đa vùng (active-active) với Aurora DSQL.*

---

### Tóm lại:

Kiến trúc này giúp các bạn tập trung vào Business Logic thay vì tốn thời gian xử lý hạ tầng DB. Nếu một Region gặp sự cố, Route 53 đẩy traffic sang Region kia, ứng dụng vẫn hoạt động bình thường mà không cần sửa code.

Các bạn làm hệ thống tải cao có ai đã áp dụng thực tế cơ chế OCC kết hợp Spring Retry này chưa? Hiệu năng connection pool thực tế có hoạt động ổn định không? Cùng chia sẻ góc nhìn nhé! 👇

---

* **Link bài gốc tham khảo**: [AWS Database Blog](https://aws.amazon.com/blogs/database/build-a-spring-boot-rest-api-with-amazon-aurora-dsql/)
* **Link bài đăng Facebook**: [AWS Study Group Facebook Post](https://www.facebook.com/photo?fbid=2093845508234493&set=gm.2199940367437590&idorvanity=660548818043427)