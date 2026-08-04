---
title: "Event 3"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 4.3. </b> "
---

# Báo cáo thu hoạch: “FCAJ Agentic AI Build Week”

### Tổng quan về sự kiện

**FCAJ Agentic AI Build Week** là một tuần lễ hackathon và tăng tốc kỹ thuật chuyên sâu tập trung vào việc xây dựng các ứng dụng thực tế được hỗ trợ bởi công nghệ **Agentic AI** (AI tác tử). Trong suốt một tuần, các đội tham gia đã thiết kế, phát triển và tối ưu hóa các tác tử AI tự hoạt (autonomous AI agents) bằng cách sử dụng các framework tiên tiến nhất và triển khai chúng trên hạ tầng đám mây AWS.

Sự kiện là sự kết hợp giữa học tập kỹ thuật, tạo mẫu nhanh (rapid prototyping) và xây dựng mô hình kinh doanh, đỉnh điểm là buổi thuyết trình (pitching) nơi các đội trình bày giải pháp của mình trước hội đồng chuyên gia công nghệ và lãnh đạo doanh nghiệp.

---

### Những thách thức chính & Bài học kinh nghiệm

Việc phát triển các hệ thống AI tác tử tự hoạt trong một khoảng thời gian giới hạn đã mang lại nhiều thử thách phức tạp về thiết kế và kỹ thuật:

* **Điều phối Tác tử & Giám sát luồng hoạt động**: Việc xây dựng các tác tử có khả năng lập kế hoạch, tự sửa lỗi và sử dụng công cụ đệ quy đòi hỏi thiết kế rất cẩn thận. Chúng tôi đã học được cách xử lý phát hiện vòng lặp vô hạn và định hướng luồng thực thi của tác tử một cách an toàn.
* **Tích hợp các Framework AI nâng cao**: Có cơ hội trải nghiệm thực tế tích hợp các công cụ như **LangFuse** để theo dõi và quan sát các câu lệnh (prompts), **Apify** để cào dữ liệu web và các wrapper tác tử tùy chỉnh như **TinyFish**.
* **Kỹ nghệ câu lệnh (Prompt Engineering) & Quản lý chi phí**: Tối ưu hóa các câu lệnh hệ thống (system prompts) để đảm bảo đầu ra của tác tử nhất quán, đồng thời quản lý lượng tiêu thụ token LLM và chi phí là một nhiệm vụ tối ưu hóa quan trọng.
* **Tạo sản phẩm khả dụng tối thiểu (MVP) nhanh chóng**: Việc chuyển đổi một ý tưởng phức tạp thành một bản thử nghiệm hoạt động ổn định dưới áp lực thời gian đã dạy chúng tôi giá trị của việc lựa chọn đúng các dịch vụ serverless và container của AWS để triển khai nhanh chóng.

---

### Màn thuyết trình & Thiết kế mô hình kinh doanh

Tuần lễ Build Week khép lại bằng các bài thuyết trình (pitching), nơi mỗi đội phải giải thích giá trị sản phẩm, kiến trúc kỹ thuật và tính khả thi trong kinh doanh của mình:

* **Xác định thách thức thực tế**: Trong buổi thuyết trình, chúng tôi đã chỉ ra những điểm đau của người dùng và các thách thức cụ thể mà giải pháp Agentic AI của chúng tôi muốn giải quyết (ví dụ: tự động hóa các quy trình thủ công phức tạp).
* **Khung Canvas tạo và phân phối giá trị**: Chúng tôi đã phác thảo các đối tác chính của hệ thống (AWS, LangFuse, Apify, TinyFish), các tài nguyên cốt lõi, phân khúc khách hàng, kênh truyền thông và tuyên bố giá trị. Hoạt động này giúp chúng tôi tư duy vượt ra ngoài phạm vi lập trình để đánh giá tính khả thi kinh doanh thực tế của sản phẩm.

---

### Trải nghiệm cá nhân

Tham gia **FCAJ Agentic AI Build Week** là một trải nghiệm đầy thử thách nhưng vô cùng xứng đáng. Nó thúc đẩy chúng tôi học hỏi và triển khai nhanh chóng các khái niệm AI nâng cao, đồng thời hợp tác chặt chẽ cùng nhau dưới áp lực thời gian.

Cơ hội được thuyết trình giải pháp và nhận phản hồi trực tiếp từ các kỹ sư cấp cao và cố vấn doanh nghiệp là vô cùng quý giá. Nó giúp chúng tôi tinh chỉnh không chỉ kiến trúc kỹ thuật mà còn cả kỹ năng thuyết trình và giao tiếp, nhấn mạnh tầm quan trọng của việc thu hẹp khoảng cách giữa triển khai kỹ thuật chuyên sâu và giá trị kinh doanh thực tiễn.

---

### Một số hình ảnh tại sự kiện

![Khai mạc FCAJ Agentic AI Build Week](/images/4-EventParticipated/4.3-Event3/01-opening.jpg)
*Hình 1: Ban tổ chức trình bày lộ trình và mục tiêu của tuần lễ Agentic AI Build Week.*

![Màn thuyết trình - Đối mặt thử thách](/images/4-EventParticipated/4.3-Event3/02-pitching-challenge.png)
*Hình 2: Trình bày các thách thức thực tế của người dùng được giải quyết bởi tác tử AI tự hoạt của chúng tôi.*

![Màn thuyết trình - Canvas giá trị](/images/4-EventParticipated/4.3-Event3/03-pitching-canvas.png)
*Hình 3: Minh họa kiến trúc hệ thống và mô hình kinh doanh thông qua khung Canvas tạo và phân phối giá trị.*

---

> Nhìn chung, sự kiện không chỉ cung cấp các kiến thức kỹ thuật mà còn giúp tôi thay đổi cách tư duy về thiết kế ứng dụng, hiện đại hóa hệ thống và phối hợp hiệu quả hơn giữa các phòng ban.
