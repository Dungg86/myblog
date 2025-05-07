---
title: "Process & Thread"
date: "2025-05-05"
updated: "2025-05-05"
categories:
  - "process & thread"
coverImage: "/images/process&thread.jpg"
coverWidth: 16
coverHeight: 9
excerpt: Cơ bản về tiến trình và luồng
---

### Hiệu năng máy tính

##### Thông số CPU

![Thông số CPU](/images/cpu.png)

Thông số cấu hình CPU của máy tính trên như sau:

- **Tổng số nhân:** 8
- **Số luồng trên mỗi nhân:** 2
- **Tổng số luồng:** 12

Rõ ràng ___tổng số luồng = tổng số nhân * số luồng trên mỗi nhân___ = 8 * 2 = 16 luồng. Tuy nhiên, máy chỉ có 12 luồng độc lập, có thể do có vài nhân là nhân tiết kiệm điện, không hỗ trợ đa luồng.

##### Thông số RAM

![Thông số RAM](/images/ram.png)

Thông số RAM của máy tính trên như sau:

- **Tổng dung lượng:** 15 Gi
- **Tổng dung lượng đã sử dụng:** 3.6 Gi
- **Tổng dung lượng trống:** 7.5 Gi
- **Dung lượng cache:** 5.2 Gi

Tổng dung lượng cộng lại = 5.2 + 7.5 + 3.6 = 16.3 Gi. Có thể hệ thống hiển thị tổng dung lượng chưa chính xác. Vì có hai thanh RAM 8 Gi, tổng dung lượng là 16 Gi.

##### Thông số GPU

![Thông số GPU](/images/gpu.png)

Máy trên có hai card đồ họa
- Một card onboard của Intel
- Một card rời của NVDIA: RTX 4050

---

### 12 bài toán IT sử dụng đa luồng

**1. Web Server (Máy chủ web đa luồng)**

**Mô tả:** Web server phải xử lý nhiều yêu cầu đồng thời từ người dùng, mỗi yêu cầu được xử lý bởi một luồng riêng biệt.

**Sử dụng đa luồng/multiprocessing:** Mỗi yêu cầu HTTP có thể được xử lý trong một luồng riêng biệt, giúp tăng hiệu suất xử lý.

**2. Download Manager (Trình quản lý tải xuống)**

**Mô tả:** Tải nhiều tệp từ nhiều nguồn khác nhau cùng một lúc.

**Sử dụng đa luồng/multiprocessing:** Mỗi luồng tải một tệp riêng biệt, tối ưu hóa tốc độ tải.

**3. Video Processing (Xử lý video)**

**Mô tả:** Các công việc xử lý video như chuyển đổi định dạng, cắt ghép video, hoặc áp dụng bộ lọc video.

**Sử dụng đa luồng/multiprocessing:** Các đoạn video có thể được xử lý đồng thời trong nhiều tiến trình hoặc luồng.

**4. Chạy thử nghiệm song song (Parallel testing)**

**Mô tả:** Khi thực hiện các bài kiểm tra phần mềm, cần kiểm tra trên nhiều môi trường hoặc phiên bản của phần mềm.

**Sử dụng đa luồng/multiprocessing:** Mỗi bài kiểm tra có thể chạy song song trên các luồng khác nhau, giảm thời gian chờ đợi.

**5. Render 3D (Render đồ họa 3D)**

**Mô tả:** Quá trình tạo hình ảnh 3D từ mô hình.

**Sử dụng đa luồng/multiprocessing:** Render các khung hình hoặc phần của cảnh 3D có thể được thực hiện song song, mỗi luồng xử lý một phần của công việc.

**6. Tìm kiếm thông tin (Information retrieval)**

**Mô tả:** Khi tìm kiếm thông tin từ các cơ sở dữ liệu lớn hoặc từ internet.

**Sử dụng đa luồng/multiprocessing:** Các truy vấn tìm kiếm có thể được xử lý song song, mỗi luồng sẽ tìm kiếm trong một phần của cơ sở dữ liệu.

**7. Các thuật toán học máy (Machine Learning Algorithms)**

**Mô tả:** Các mô hình học máy phức tạp như huấn luyện mạng nơ-ron hoặc phân tích dữ liệu lớn.

**Sử dụng đa luồng/multiprocessing:** Việc huấn luyện các mô hình có thể được phân chia giữa nhiều tiến trình hoặc luồng, mỗi tiến trình/luồng sẽ xử lý một phần của bộ dữ liệu.

**8. Tính toán khoa học (Scientific Computing)**

**Mô tả:** Các bài toán tính toán khoa học như mô phỏng động lực học hoặc xử lý tín hiệu.

**Sử dụng đa luồng/multiprocessing:** Các phép tính phức tạp có thể được chia nhỏ và thực hiện song song trên nhiều luồng hoặc tiến trình.

**9. Chạy ứng dụng đa nhiệm (Multitasking applications)**

**Mô tả:** Các ứng dụng như trình duyệt web, xử lý văn bản hoặc các ứng dụng desktop.

**Sử dụng đa luồng/multiprocessing:** Các tác vụ như tải trang, lưu dữ liệu, hoặc hiển thị giao diện người dùng có thể được xử lý song song trong các luồng khác nhau.

**10. Xử lý dữ liệu lớn (Big Data Processing)**

**Mô tả:** Phân tích và xử lý dữ liệu lớn từ các nguồn như mạng xã hội, cảm biến IoT, hoặc các cơ sở dữ liệu phân tán.

**Sử dụng đa luồng/multiprocessing:** Các phần của dữ liệu có thể được xử lý song song bởi nhiều tiến trình hoặc luồng, giúp tăng tốc quá trình phân tích.

**11. Chạy các game trực tuyến (Online gaming)**

**Mô tả:** Các game trực tuyến có thể có hàng nghìn người chơi đồng thời, mỗi người chơi yêu cầu tài nguyên tính toán và xử lý riêng.

**Sử dụng đa luồng/multiprocessing:** Mỗi kết nối của người chơi có thể được xử lý trong một luồng riêng biệt, trong khi các game server có thể sử dụng nhiều tiến trình để xử lý các phần khác nhau của trò chơi.

**12. Giải thuật sắp xếp song song (Parallel sorting algorithms)**

**Mô tả:** Các thuật toán sắp xếp dữ liệu nhanh chóng và hiệu quả.

**Sử dụng đa luồng/multiprocessing:** Các thuật toán sắp xếp có thể được chia thành các phần nhỏ và chạy song song trên nhiều tiến trình hoặc luồng để tăng tốc độ.

---

### Lựa chọn process hay thread?

![when use process or thread?](/images/when_use_process_or_thread.jpg)

---

### Tranging chatPGT

**1. Hạ tầng phần cứng: Siêu máy tính GPU quy mô lớn**

OpenAI sử dụng các siêu máy tính được xây dựng từ hàng nghìn GPU NVIDIA, như A100 hoặc H100, kết nối với nhau thông qua mạng tốc độ cao như InfiniBand.

Ví dụ, GPT-3 (175 tỷ tham số) được huấn luyện trên cụm GPU với hiệu suất đạt 502 petaFLOPS/s trên 3.072 GPU, sử dụng Megatron-LM để tối ưu hóa hiệu suất. 
arXiv

GPT-4 được huấn luyện tại trung tâm dữ liệu của Microsoft ở Iowa, nơi sử dụng lượng nước lớn để làm mát hệ thống. 
AP News

**2. Chiến lược huấn luyện phân tán: Kết hợp nhiều kỹ thuật song song**

Để huấn luyện mô hình lớn, OpenAI kết hợp các phương pháp sau:

- **Song song dữ liệu (Data Parallelism):** Mỗi GPU xử lý một phần dữ liệu khác nhau nhưng sử dụng cùng một mô hình.

- **Song song mô hình (Model Parallelism):** Chia nhỏ mô hình thành các phần và phân phối chúng trên nhiều GPU.

- **Song song pipeline (Pipeline Parallelism):** Chia quá trình huấn luyện thành các giai đoạn và xử lý chúng theo chuỗi trên các GPU.

Megatron-LM hỗ trợ kết hợp các kỹ thuật này để huấn luyện mô hình với hàng nghìn tỷ tham số. 
arXiv

**3. Phần mềm và công cụ hỗ trợ**

- **DeepSpeed:** Một thư viện tối ưu hóa huấn luyện từ Microsoft, giúp giảm chi phí và tăng tốc độ huấn luyện mô hình lớn.

- **Ray:** Khung phân tán hỗ trợ các tác vụ học máy, giúp quản lý tài nguyên và phân phối công việc hiệu quả.

- **DeepSpeed-Chat:** Cung cấp pipeline RLHF (Reinforcement Learning with Human Feedback) hiệu quả, giúp huấn luyện mô hình giống ChatGPT với chi phí thấp hơn. 
arXiv

**4. Quy trình huấn luyện ChatGPT**

- **Tiền huấn luyện (Pretraining):** Mô hình được huấn luyện trên tập dữ liệu văn bản lớn từ internet để học ngôn ngữ tự nhiên.

- **Huấn luyện có giám sát (Supervised Fine-tuning):** Mô hình được tinh chỉnh với dữ liệu có gán nhãn để thực hiện các nhiệm vụ cụ thể.

- **Học tăng cường từ phản hồi con người (RLHF):** Sử dụng phản hồi từ con người để cải thiện chất lượng và độ an toàn của mô hình.

###### Tài liệu tham khảo

- [*https://thenewstack.io/how-ray-a-distributed-ai-framework-helps-power-chatgpt/*](https://thenewstack.io/how-ray-a-distributed-ai-framework-helps-power-chatgpt/)
- [*https://apnews.com/article/chatgpt-gpt4-iowa-ai-water-consumption-microsoft-f551fde98083d17a7e8d904f8be822c4*](https://apnews.com/article/chatgpt-gpt4-iowa-ai-water-consumption-microsoft-f551fde98083d17a7e8d904f8be822c4)
