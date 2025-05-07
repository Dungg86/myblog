# 🚀 CodeLog — Nhật Ký Hành Trình Làm Bài Tập Lớn Môn Ứng Dụng Phân Tán

Chào bạn! Chúng mình là **Dung** và **Huy**, tụi mình tạo ra blog này như một cách để ghi lại tiến trình làm bài tập lớn giữa kỳ môn **Ứng Dụng Phân Tán** (Distributed Applications) ở trường. Nếu bạn cũng đang học môn này, chắc hẳn bạn sẽ hiểu cảm giác vừa vui vừa "dở khóc dở cười" khi mà bài tập lớn này lại liên quan đến rất nhiều khái niệm phức tạp như mạng máy tính, phân tán dữ liệu, và thiết kế hệ thống.

Tụi mình đã quyết định tạo ra blog này để **lưu lại những bước đi** của tụi mình trong suốt quá trình làm bài tập. Mỗi tuần, mỗi ngày, tụi mình sẽ chia sẻ một chút về những gì tụi mình đã làm được, gặp phải vấn đề gì, và những giải pháp mà tụi mình nghĩ ra. Hy vọng rằng những bài viết này không chỉ giúp tụi mình sau này quay lại xem lại, mà còn giúp bạn đọc có thể học hỏi, lấy cảm hứng hoặc thậm chí tránh được những sai lầm mà tụi mình đã mắc phải trong quá trình làm việc.

---

## 🎯 Mục Đích Của Blog

Tụi mình tạo blog này với một vài mục đích chính sau:

1. **Lưu trữ tiến độ công việc**: Blog này là một cách tuyệt vời để tụi mình ghi lại tất cả những gì đã làm trong bài tập lớn, từ những ý tưởng ban đầu, các bài học học được, cho đến những quyết định quan trọng mà tụi mình đã đưa ra.
   
2. **Chia sẻ kinh nghiệm**: Chúng mình hy vọng rằng những gì tụi mình chia sẻ sẽ giúp ích cho những ai đang học môn này hoặc có cùng sở thích với tụi mình. Nếu bạn gặp phải những vấn đề tương tự, có thể những bài viết này sẽ giúp bạn giải quyết nhanh hơn.

3. **Ghi chép lại những khó khăn và giải pháp**: Làm bài tập lớn về ứng dụng phân tán không hề dễ dàng, và tụi mình gặp rất nhiều vấn đề. Những vấn đề này có thể là về **quản lý dữ liệu**, **giao tiếp giữa các máy chủ**, **tối ưu hóa hiệu suất**... Tụi mình sẽ ghi lại tất cả, kèm theo cách tụi mình giải quyết chúng để có thể tham khảo sau này.

4. **Tài liệu tham khảo cho những ai đang bắt đầu**: Các bạn có thể xem các bài viết của tụi mình như một **hướng dẫn thực tế**, cách tụi mình áp dụng lý thuyết vào thực tiễn để giải quyết các bài toán phân tán.

---

## 🛠️ Công Nghệ Và Công Cụ Tụi Mình Sử Dụng

Tụi mình sử dụng một số công nghệ và công cụ để triển khai blog này, bao gồm:

### **SvelteKit**
- **Tại sao chọn SvelteKit?**: SvelteKit là một framework cực kỳ mạnh mẽ cho việc xây dựng ứng dụng web, đặc biệt là những ứng dụng tĩnh như blog. Nó hỗ trợ việc biên dịch HTML tĩnh cực nhanh, giúp website hoạt động mượt mà và có hiệu suất cao. Tụi mình chọn SvelteKit để xây dựng blog vì nó rất dễ sử dụng và có khả năng mở rộng linh hoạt, từ frontend đến backend.
  
- **Tính năng đáng chú ý**: 
    - Tối ưu hóa SEO tự động.
    - Tính năng phân trang (pagination) để quản lý các bài viết dễ dàng.
    - Dễ dàng tích hợp các bài viết Markdown.
    - Tích hợp sẵn RSS feed, giúp bạn dễ dàng đăng ký theo dõi bài viết của tụi mình.

### **Markdown**
- Mỗi bài viết trong blog này đều được viết bằng **Markdown** vì đây là một cách đơn giản và nhanh chóng để biên tập nội dung mà không cần phải lo lắng về cấu trúc HTML. Markdown cũng giúp tụi mình dễ dàng viết tài liệu kỹ thuật mà không bị phân tâm về các chi tiết thừa.

### **Netlify**
- Tụi mình sử dụng **Netlify** để deploy blog này lên internet. Với Netlify, tụi mình chỉ cần push code lên GitHub và Netlify sẽ tự động xây dựng và deploy website. Điều này cực kỳ tiện lợi và nhanh chóng, giúp tụi mình tiết kiệm được rất nhiều thời gian.

---

## 📝 Nội Dung Blog

Blog này sẽ chủ yếu tập trung vào các nội dung sau:

1. **Tiến độ công việc**: Mỗi bài viết sẽ ghi lại những gì tụi mình làm được trong một tuần, một buổi làm việc, hoặc khi tụi mình gặp phải những vấn đề khó khăn nào.
  
2. **Các quyết định thiết kế**: Tụi mình sẽ giải thích vì sao tụi mình lại chọn lựa các giải pháp kỹ thuật như vậy, từ việc phân chia các module trong hệ thống đến việc chọn các giao thức giao tiếp giữa các máy chủ.

3. **Các lỗi và cách khắc phục**: Làm bài tập lớn kiểu này chắc chắn sẽ không thiếu lỗi. Những lỗi tụi mình gặp phải (như **bug phân tán**, **thiết kế không hiệu quả**...) sẽ được tụi mình ghi lại và chia sẻ cách xử lý.

4. **Tài liệu tham khảo**: Tụi mình sẽ chia sẻ các tài liệu, bài viết hoặc bài giảng tụi mình đã tham khảo trong quá trình làm bài. Đây sẽ là những tài liệu quý giá cho bạn nào đang tìm hiểu về ứng dụng phân tán.

---

## 💬 Những Điều Tụi Mình Học Được

Tụi mình đã học được rất nhiều điều trong suốt quá trình làm bài tập lớn này. Dưới đây là một vài bài học tụi mình rút ra:

- **Làm việc nhóm là một nghệ thuật**: Ban đầu, tụi mình gặp khá nhiều khó khăn trong việc phân công công việc. Tuy nhiên, dần dần tụi mình đã học được cách giao tiếp rõ ràng và hiệu quả hơn, biết cách trao đổi ý tưởng và phối hợp với nhau.
  
- **Chia sẻ tiến độ thường xuyên giúp giữ vững động lực**: Việc ghi lại tiến độ và chia sẻ với nhau mỗi ngày không chỉ giúp tụi mình nhớ lại được những gì đã làm mà còn tạo động lực để tiếp tục tiến lên. Những bài viết trong blog này chính là minh chứng cho sự kiên trì đó.

- **Chấp nhận thất bại là một phần trong quá trình học**: Không phải lúc nào tụi mình cũng thành công ngay từ lần đầu tiên. Nhưng tụi mình nhận ra rằng, mỗi thất bại đều có giá trị riêng và giúp tụi mình trưởng thành hơn. Và nếu không ghi lại nó, tụi mình sẽ dễ quên và mắc lại lỗi cũ.

- **Lý thuyết và thực tiễn luôn có sự khác biệt**: Khi học lý thuyết về phân tán, tụi mình nghĩ là dễ, nhưng khi phải thực hiện thực tế thì hoàn toàn khác. Các vấn đề như **tính nhất quán dữ liệu**, **vấn đề đồng bộ hóa** luôn là những thử thách lớn.

---

## 🚀 Hướng Dẫn Triển Khai Blog

Tụi mình đã chuẩn bị sẵn cho bạn một hệ thống blog đơn giản nhưng mạnh mẽ để bạn có thể triển khai và sử dụng ngay. Bạn chỉ cần làm theo các bước sau:

1. **Clone Repo**: Bạn có thể clone repository này về máy của mình bằng lệnh:
    ```bash
    npx degit https://github.com/Catherine1401/blog.git my-sveltekit-blog
    ```
2. **Cài Đặt Các Gói Phụ Thuộc**:
    ```bash
    cd my-sveltekit-blog
    npm install
    ```

3. **Chạy Dự Án**:
    ```bash
    npm run dev -- --open
    ```
    Sau khi chạy lệnh này, bạn có thể mở website của mình trên trình duyệt và bắt đầu chỉnh sửa blog!

---

## 📬 Liên Hệ

Cảm ơn bạn đã ghé qua blog của tụi mình. Nếu bạn có bất kỳ câu hỏi nào, góp ý hay chỉ đơn giản muốn chia sẻ kinh nghiệm của bạn, đừng ngần ngại liên hệ với tụi mình. Bạn có thể tạo issue trên GitHub hoặc gửi email cho tụi mình qua địa chỉ dưới đây:

👉 [GitHub Repository](https://github.com/Catherine1401/blog.git)

Chúc bạn làm việc hiệu quả và hy vọng blog này sẽ giúp ích cho bạn trong hành trình học tập và làm việc của mình!

--- 

_Hẹn gặp lại trong những bài viết tiếp theo, hãy luôn tiếp tục chia sẻ kiến thức và đam mê nhé! 💬_

