---
title: "Deliaverable 4: Hệ Thống Giám Sát & Điều Khiển Nông Trại Thông Minh sử dụng CnosDB"
date: "2025-06-01"
updated: "2025-06-01"
categories:
  - "Ứng dụng phân tán"
coverImage: "/images/delivarable4.webp"
coverWidth: 16
coverHeight: 9
excerpt: "Hệ thống giám sát và điều khiển nông trại thông minh sử dụng CnosDB, mô phỏng cảm biến môi trường, xử lý dữ liệu thời gian thực và giao diện trực quan."
---

# LỜI NÓI ĐẦU

Đầu tiên, chúng em xin gửi lời cảm ơn chân thành đến thầy **Phạm Kim Thành**, người dìu dắt chúng em qua học phần *Ứng dụng phân tán*. Thầy đã khai mở tư duy cho chúng em, cho chúng em những góc nhìn mới và chúng em đã học được rất nhiều từ thầy.

Đây là **báo cáo bài tập lớn môn Ứng dụng phân tán**. Học phần này yêu cầu sinh viên thiết kế một ứng dụng có các đặc tính phân tán bao gồm:

- Tính trong suốt  
- Tính mở  
- Tính đồng thời  
- Tính không đồng bộ  
- Tính chịu lỗi  
- Tính mở rộng  

Tài nguyên chúng em được giao là **Cnosdb** (*Cloud native open source database*), yêu cầu là phải dựa vào cơ sở dữ liệu này để xây dựng nên một hệ thống phân tán.

Tuy nhiên, do thời gian cũng như năng lực có hạn nên sản phẩm của chúng em vẫn còn nhiều thiếu sót. Chúng em sẽ bổ sung thêm trong thời gian tới.

---

# 1. ĐẶT VẤN ĐỀ

## 1.1. Bối cảnh

Trong bối cảnh hiện nay, **nông nghiệp** đang từng bước chuyển mình để thích nghi với các xu hướng công nghệ mới, đặc biệt là mô hình **nông nghiệp thông minh**. Mô hình này hướng đến việc ứng dụng các công nghệ hiện đại như:

- Internet of Things (IoT)  
- Trí tuệ nhân tạo (AI)  
- Hệ thống cảm biến  
- Giải pháp lưu trữ – phân tích dữ liệu  
 Mục tiêu là nâng cao năng suất, tối ưu hóa nguồn lực và bảo vệ môi trường.

Tuy nhiên, tại nhiều trang trại – đặc biệt ở nông thôn – việc canh tác vẫn còn:

- Mang tính thủ công, rời rạc  
- Thiếu tính hệ thống  
- Dữ liệu môi trường (nhiệt độ, độ ẩm đất, ánh sáng,...) không được ghi lại hoặc ghi chép thủ công  

Dẫn đến các hệ quả:

- Không thể giám sát điều kiện môi trường theo thời gian thực  
- Không thể tự động hóa các hoạt động canh tác  
- Khó phân tích xu hướng  
- Không có lịch sử dữ liệu để đánh giá hiệu quả  

Ngoài ra, để triển khai hệ thống giám sát – điều khiển tự động, ta cần một **nền tảng lưu trữ dữ liệu**:

- Nhanh  
- Ổn định  
- Mở rộng được  
- Tối ưu cho dữ liệu thời gian thực  
 Điều mà các CSDL truyền thống (MySQL, PostgreSQL,...) **không đáp ứng tốt**.

### Bài toán đặt ra:

- Làm sao để thu thập dữ liệu môi trường liên tục và chính xác?
- Làm sao để lưu trữ và truy xuất dữ liệu hiệu quả theo thời gian?
- Làm sao để phân tích và tự động ra quyết định (VD: tưới nước khi đất khô)?
- Làm sao để hệ thống đơn giản, rẻ, dễ dùng với người nông dân?

---

## 1.2. Đề xuất giải pháp

Nhóm đề xuất xây dựng một hệ thống:

- Sử dụng cơ sở dữ liệu hiệu suất cao cho **time series data**
- Cung cấp **dashboard trực quan** để theo dõi theo thời gian thực
- Hiển thị cảnh báo khi có bất thường (VD: nhiệt độ cao, độ ẩm thấp)
- Hỗ trợ người dùng:
  - Kích hoạt hệ thống tưới tiêu tự động
  - Gửi thông báo đến người quản lý

---

# 2. THIẾT KẾ HỆ THỐNG

## 2.1. Mô tả hệ thống

Hệ thống xây dựng theo **kiến trúc phân tầng**, gồm các thành phần:

- **Thu thập dữ liệu** từ cảm biến (nhiệt độ, độ ẩm, ánh sáng,...)
- **Lưu trữ dữ liệu** vào Cnosdb dưới dạng **time series**
- **Hiển thị và điều khiển** qua giao diện người dùng
 **Cnosdb** là nền tảng lưu trữ chính nhờ:

- Xử lý hiệu suất cao  
- Hỗ trợ dữ liệu cảm biến theo thời gian  

Sau khi lưu trữ, dữ liệu sẽ được truy xuất để:

- Hiển thị trên **giao diện trực quan**
- Tạo **dashboard** theo dõi tình trạng từng khu vực
- Gửi **cảnh báo sớm**
- Cho phép người dùng **đưa ra hành động điều khiển**

> Hệ thống hướng tới: **dễ triển khai – dễ mở rộng – phù hợp với nông nghiệp thông minh**

## 2.2. Kiến trúc hệ thống

```st
               +---------------------------------------------+
               |               User Interface (UI)           |
               | (Monitoring, Alerts, Control Actions)       |
               +----------------------+----------------------+
                                      |
                             +--------v--------+
                             |     API Layer    |
                             +--------+--------+
                                      |
                   +-----------------v-----------------+
                   |  Multi-threaded Data Handler      |
                   |  - Write Threads                  |
                   |  - Read Threads                   |
                   |  - Alert/Decision Logic           |
                   +----------------+------------------+
                                      |
                         +------------v-------------+
                         |       CNOSDB Cluster     |
                         |  (Time-Series Database)  |
                         +------------+--------------+
                                      ^
             +------------------------+------------------------+
             |                        |                        |
   +---------v--------+    +---------v--------+     +---------v--------+
   |    Gateway A     |    |    Gateway B     | ... |    Gateway N     |
   +---------+--------+    +---------+--------+     +---------+--------+
             |                       |                         |
   +---------v-------+       +---------v-------+      +----------v---------+
   | Sensor A1...An  |       | Sensor B1...Bn  | ...  | Sensor N1...Nn     |
   +-----------------+       +-----------------+      +--------------------+

                         <== Load Balancer ==>
                     (Distributes load from all gateways)

```

### 2.2.1. Sensor Nodes

- Thu thập dữ liệu môi trường như: nhiệt độ, độ ẩm, ánh sáng,...
- Gửi dữ liệu về Gateway gần nhất theo chu kỳ định sẵn.

### 2.2.2. Gateway

- Tập trung dữ liệu từ các cảm biến nội vùng.
- Gửi dữ liệu theo định dạng chuẩn (JSON, Protocol Buffers,...) thông qua HTTP hoặc MQTT đến Load Balancer.

### 2.2.3. Load Balancer

- Phân phối lưu lượng ghi dữ liệu từ các Gateway đến các node trong cụm CNOSDB.
- Đảm bảo cân bằng tải giữa các node, tránh tình trạng nghẽn cổ chai.

### 2.2.4. CNOSDB Cluster

- Là cơ sở dữ liệu thời gian thực dùng để lưu trữ dữ liệu cảm biến.
- Hỗ trợ mở rộng ngang (horizontal scaling) và sharding theo vùng địa lý hoặc loại cảm biến.
- Tối ưu hóa cho các truy vấn dạng time-series (chuỗi thời gian).

### 2.2.5. Trình xử lý dữ liệu đa luồng (Multi-threaded Data Handler)

- **Luồng ghi (Write Threads):** tiếp nhận dữ liệu từ Load Balancer và ghi xuống CNOSDB.
- **Luồng đọc (Read Threads):** đọc dữ liệu từ CNOSDB để phục vụ hiển thị dashboard.
- **Luồng phân tích (Alert Threads):** kiểm tra dữ liệu bất thường như độ ẩm thấp, nhiệt độ cao.
- **Luồng hành động (Action Threads):** gửi lệnh điều khiển thiết bị nếu có cảnh báo, thông qua Gateway đến các cảm biến.

### 2.2.6. Giao diện người dùng (User Interface - UI)

- Giao diện dashboard hiển thị dữ liệu thời gian thực bằng biểu đồ.
- Hiển thị cảnh báo trực quan theo màu sắc và mức độ nghiêm trọng.
- Cung cấp các nút điều khiển như:
  - Tưới nước thủ công
  - Điều chỉnh ngưỡng cảnh báo
  - Bật/tắt hệ thống

## 2.3. Luồng hoạt động

```st
+--------------------------+
|  Sensor collects data    |
+--------------------------+
           |
           v
+--------------------------+
|  Data sent to Gateway    |
| (A, B, or C)             |
+--------------------------+
           |
           v
+--------------------------+
|  Gateway forwards data   |
|  to CNOSDB Cluster       |
+--------------------------+
           |
           v
+-------------------------------+
|  CNOSDB stores time-series    |
|  data                         |
+-------------------------------+
           |
           v
+-------------------------------+
|  Multi-threaded Data Handler  |
| - Reads/Writes                |
| - Alert/Decision Logic        |
+-------------------------------+
           |
           v
+-----------------------------+
|  API Layer prepares results |
+-----------------------------+
           |
           v
+-----------------------------+
|  User Interface (UI)        |
|  - Monitoring               |
|  - Alerts                   |
|  - Control Actions          |
+-----------------------------+

```

# 3. Tài nguyên sử dụng

## 3.1. Ngôn ngữ lập trình chính: Python

Hệ thống được xây dựng chủ yếu bằng ngôn ngữ Python, nhờ vào các ưu điểm sau:

- Dễ học, dễ đọc và bảo trì.
- Hỗ trợ mạnh mẽ cho xử lý dữ liệu, giao tiếp mạng, và lập trình song song.
- Có cộng đồng phát triển rộng lớn với hệ sinh thái thư viện đa dạng.

**Thư viện bên ngoài của Python được sử dụng:**

| Thư viện              | Vai trò |
|------------------------|--------|
| `streamlit`            | Tạo giao diện người dùng web đơn giản và hiệu quả, dùng để hiển thị dữ liệu cảm biến theo thời gian thực. |
| `plotly.express`       | Tạo các biểu đồ động, đẹp mắt và tương tác được (ví dụ: biểu đồ nhiệt độ, độ ẩm theo thời gian). |
| `pandas`               | Xử lý dữ liệu dạng bảng, chuyển đổi dữ liệu từ CNOSDB sang định dạng phân tích được. |
| `concurrent.futures`   | Tạo các luồng song song để thực hiện đọc và ghi dữ liệu cùng lúc mà không bị nghẽn. |
| `streamlit_autorefresh`| Làm mới giao diện tự động để đảm bảo người dùng luôn nhìn thấy dữ liệu mới nhất. |
| `urllib.request`       | Giao tiếp với các API hoặc tải dữ liệu từ xa. |
| `datetime`             | Gắn dấu thời gian, định dạng thời gian, hỗ trợ truy vấn dữ liệu theo thời gian. |
| `random`               | Sinh dữ liệu cảm biến ngẫu nhiên trong giai đoạn thử nghiệm. |
| `sys`, `os`            | Quản lý đường dẫn thư mục, hỗ trợ import mô-đun con trong dự án lớn nhiều tầng thư mục. |



## 3.2. Cơ sở dữ liệu: CNOSDB

CNOSDB là một hệ quản trị cơ sở dữ liệu mã nguồn mở, chuyên dụng cho dữ liệu chuỗi thời gian. Cơ sở dữ liệu này được thiết kế để phục vụ các ứng dụng đòi hỏi:

- Hiệu suất cao
- Khả năng nén mạnh
- Dễ sử dụng

Toàn bộ mã nguồn của CNOSDB đã được công khai trên GitHub, tạo điều kiện thuận lợi cho việc kiểm thử, triển khai và tùy biến theo yêu cầu thực tế.

Với đặc tính hiệu suất cao, khả năng mở rộng linh hoạt, hỗ trợ truy vấn thời gian mạnh mẽ và thiết kế phân tán cloud-native, CNOSDB là một giải pháp rất phù hợp cho các hệ thống cần lưu trữ và phân tích dữ liệu cảm biến thời gian thực ở quy mô lớn.

Đây là một nền tảng dữ liệu lý tưởng cho các ứng dụng IoT như **nông nghiệp thông minh**, giúp tối ưu hóa vận hành, tiết kiệm tài nguyên và tự động hóa quản lý trang trại.

# 4. Xây dựng hệ thống

## Cấu trúc thư mục của dự án:

```st

SMART_FARM/
├── config/            # Cấu hình hệ thống
├── db/                # Giao tiếp với CnosDB
├── distributed/       # Tài liệu triển khai phân tán
├── images/            # Lưu hình ảnh, biểu đồ
├── sensors/           # Các lớp cảm biến và mô phỏng
├── ui/                # Giao diện người dùng
├── main.py            # Điểm khởi động hệ thống
├── requirements.txt   # Danh sách thư viện cần thiết
├── README.md          # Tài liệu giới thiệu hệ thống

```


## 4.1. `config/`

Chứa file `cnosdb_config.py` định nghĩa thông tin kết nối đến CnosDB (địa chỉ, cổng, bucket, token,...).  
Việc tách riêng cấu hình giúp dễ thay đổi mà không ảnh hưởng đến logic hệ thống.

## 4.2. `db/`

Gồm các module xử lý giao tiếp với CnosDB:
- `writer.py`: Ghi dữ liệu cảm biến vào CnosDB.
- `query.py`: Truy vấn dữ liệu phục vụ dashboard và cảnh báo.

## 4.3. `sensors/`

Định nghĩa các lớp cảm biến:
- `base_sensor.py`: Lớp cơ sở (abstract class).
- `temperature_sensor.py`, `humidity_sensor.py`, `soil_sensor.py`: Các cảm biến cụ thể.
- `run_fake_sensor.py`: Sinh dữ liệu mô phỏng để test/demo.

## 4.4. `ui/`

Xây dựng giao diện người dùng bằng Streamlit:
- `dashboard.py`: Hiển thị dữ liệu thời gian thực, biểu đồ, trạng thái cảm biến, và điều khiển từ người dùng.

## 4.5. `distributed/`

Tài liệu `cnosdb_cluster.md` hướng dẫn triển khai cụm CnosDB dạng phân tán trong môi trường thực tế.

## 4.6. `main.py`

Tập tin chính để chạy hệ thống: khởi tạo cảm biến, đa luồng ghi dữ liệu, giao tiếp với CnosDB và hiển thị giao diện người dùng.

---

# 5. Sử dụng hệ thống

## 5.1. Yêu cầu tối thiểu

- Hệ điều hành: Linux (Debian-based).
- Python 3.0+
- Docker
- Git

## 5.2. Các bước cấu hình

**Bước 1: Clone dự án**

```bash
git clone https://github.com/Catherine1401/smart_farm.git
cd smart_farm
```

Bước 2: Chạy dự án

```bash
python3 main.py
```

Hệ thống sẽ:

- Tự động tải Docker image của CnosDB.
- Khởi chạy container.
- Ghi dữ liệu cảm biến.
- Mở giao diện dashboard tại localhost:8501.


# 6. Phương hướng phát triển

## 6.1. Hiện trạng

H thống hiện dùng CnosDB Community (single node) để:
- 
- Ghi dữ liệu từ 1000 trạm cảm biến mô phỏng (đa luồng).
- Hiển thị dữ liệu với Streamlit.
- Phản hồi và cảnh báo thời gian thực.

Hạn chế:

- Single-point-of-failure (1 node → dễ chết).
- Không scale-out / replication.
- Không phân vùng dữ liệu hay quản lý vnodes ở mức cluster.

## 6.2. Dự định trong tương lai

### 6.2.1. Phương án 1 (phiên bản tiết kiệm)

Dùng nhiều instance CnosDB Community, triển khai qua Docker.

Cân bằng tải bằng Nginx hoặc HAProxy:

- Ghi /write: round-robin.
- Đọc /query: theo tải hoặc sẵn có.
- Dùng các câu lệnh MOVE VNODE, COPY VNODE, DROP VNODE (hỗ trợ bản Community).

### 6.2.2. Phương án 2 (phiên bản đầy đủ - Enterprise)

Nâng cấp lên CnosDB Enterprise, hỗ trợ cluster:

- SHOW DATANODES: Theo dõi node.
- meta_service_addr: Cho các node tham gia 1 cụm.
- Replication: Tự động sao lưu giữa các node.
- REMOVENODE, DROP VNODE: Dễ bảo trì node.

Triển khai:

- 1 node Meta (port 8901).
- 2–3 node làm Storage + Query.
- Cấu hình meta_service_addr để node tự động tham gia cụm.
- Ghi/đọc phân phối tự động nhờ cluster controller.

# 7. Kết luận

Hệ thống giám sát và điều khiển nông trại thông minh sử dụng CnosDB đã được xây dựng thành công, đáp ứng các yêu cầu về:
- Giám sát môi trường theo thời gian thực.
- Lưu trữ dữ liệu cảm biến hiệu quả.
- Cung cấp giao diện trực quan và dễ sử dụng. 
