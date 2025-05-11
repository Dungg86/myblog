---
title: "Deliverable 1: Tìm hiểu thư viện và đề xuất đề tài dự án"
date: "2025-05-11"
updated: "2025-05-11"
categories:

coverImage: "/images/deliverable.jpg"
coverWidth: 16
coverHeight: 9
excerpt: Tổng quan về đề tài, đề xuất giải pháp và đường hướng triển khai dự án ...
---

## Content
- [Thư viện CNOSDB](#thư-viện-cnosdb)
    - [Mục đích của thư viện](#mục-đích-của-thư-viện)
    - [Vấn đề thư viện có thể giải quyết](#vấn-đề-thư-viện-có-thể-giải-quyết)
    - [Điểm mạnh của CNOSDB](#điểm-mạnh-của-cnosdb)
    - [Điểm yếu của CNOSDB](#điểm-yếu-của-cnosdb)
    - [So sánh với các thư viện tương tự](#so-sánh-với-các-thư-viện-tương-tự)
- [Đề xuất đề tài dự án](#đề-xuất-đề-tài-dự-án)
    - [Vấn đề cần giải quyết](#vấn-đề-cần-giải-quyết)
    - [Mục tiêu của dự án](#mục-tiêu-của-dự-án)
    - [Giải pháp đề xuất](#giải-pháp-đề-xuất)

## Thư viện CNOSDB
---
#### Mục đích của thư viện

CNOSDB là một hệ thống dùng để lưu trữ và xử lý dữ liệu theo thời gian, tức là các loại dữ liệu được ghi lại liên tục theo từng thời điểm – ví dụ như nhiệt độ, độ ẩm, ánh sáng,... thường thấy trong cảm biến hoặc hệ thống giám sát.

Mục đích của CNOSDB là giúp chúng ta quản lý dữ liệu từ cảm biến một cách nhanh chóng, hiệu quả và dễ mở rộng, đặc biệt là trong các dự án liên quan tới IoT (Internet of Things), như nông trại thông minh.

#### Vấn đề thư viện có thể giải quyết

- **Ghi và lưu dữ liệu liên tục từ cảm biến:** Ví dụ mỗi phút một cảm biến gửi dữ liệu, CNOSDB sẽ lưu lại theo thứ tự thời gian.

- **Truy vấn dữ liệu dễ dàng:** Có thể hỏi hệ thống "trong 2 ngày qua nhiệt độ trung bình là bao nhiêu?".

- **Phù hợp với dữ liệu lớn:** Khi có hàng trăm hoặc hàng ngàn cảm biến gửi dữ liệu liên tục, CNOSDB vẫn xử lý tốt.

- **Kết nối dễ dàng với các thiết bị IoT:** Nhờ hỗ trợ nhiều kiểu kết nối như HTTP, MQTT,...

- **Hỗ trợ tự động hóa:** Có thể dùng dữ liệu từ CNOSDB để ra quyết định – ví dụ: tưới nước khi đất khô.

#### Điểm mạnh của CNOSDB

- **Nhanh và hiệu quả:** Lưu trữ và xử lý hàng ngàn dữ liệu mỗi giây mà không bị chậm.

- **Tối ưu cho dữ liệu cảm biến:** Được thiết kế riêng để làm việc với dữ liệu thời gian.

- **Dễ kết nối với thiết bị IoT:** Có thể lấy dữ liệu từ cảm biến, gửi vào hệ thống dễ dàng.

- **Dễ mở rộng:** Nếu sau này muốn thêm nhiều cảm biến hoặc khu vực giám sát, vẫn có thể dùng chung hệ thống này.

#### Điểm yếu của CNOSDB

- **Mới và ít người dùng:** Vì còn khá mới nên chưa có nhiều tài liệu, ví dụ hay cộng đồng hỗ trợ.

- **Phải tự làm giao diện hiển thị:** CNOSDB không có giao diện trực quan sẵn, nên phải dùng thêm công cụ khác như Grafana.

- **Ít ví dụ thực tế:** Cần tự mày mò nhiều nếu gặp lỗi.

#### So sánh với các thư viện tương tự
---

| Tiêu chí           | CNOSDB       | InfluxDB             | TimescaleDB             |
| ------------------ | ------------ | -------------------- | ----------------------- |
| Dễ dùng            | Trung bình   | Dễ                   | Khó hơn một chút        |
| Hiệu suất          | Rất tốt      | Tốt                  | Trung bình              |
| Dữ liệu cảm biến   | Rất phù hợp  | Phù hợp              | Cần cấu hình thêm       |
| Giao diện hiển thị | Phải tự thêm | Có sẵn đơn giản      | Phải dùng công cụ ngoài |
| Được dùng phổ biến | Chưa nhiều   | Rất nhiều người dùng | Tương đối               |

## Đề xuất đề tài dự án
---
#### Đề tài: Xây dựng hệ thống giám sát và điều khiển trang trại thông minh sử dụng CNOSDB

#### Vấn đề cần giải quyết

Hiện nay, nhiều trang trại vẫn sử dụng cách làm thủ công hoặc hệ thống đơn giản, không có khả năng lưu trữ và phân tích dữ liệu dài hạn. Điều này gây lãng phí tài nguyên (nước, phân bón), hiệu quả không cao và khó kiểm soát từ xa.

Vấn đề chính là làm sao thu thập dữ liệu liên tục, lưu trữ có tổ chức, và sử dụng dữ liệu để tự động điều khiển hoạt động trong trang trại.

#### Mục tiêu của dự án

Mục tiêu của dự án là xây dựng một hệ thống thu thập và lưu trữ dữ liệu từ các cảm biến trong nông trại (ví dụ: nhiệt độ, độ ẩm không khí, độ ẩm đất, ánh sáng, v.v.) thông qua CNOSDB để:

- Giám sát điều kiện môi trường theo thời gian thực.

- Phân tích dữ liệu để đưa ra các hành động tự động như tưới nước khi đất khô.

- Hiển thị thông tin qua dashboard để người nông dân dễ theo dõi.

#### Giải pháp đề xuất

Hệ thống sẽ có các thành phần:

- Cảm biến IoT đặt trong trang trại: đo nhiệt độ, độ ẩm đất, ánh sáng,...

- Thiết bị thu thập dữ liệu: gửi dữ liệu lên CNOSDB.

- CNOSDB: lưu trữ dữ liệu theo thời gian thực.

- Dashboard hoặc ứng dụng web: hiển thị dữ liệu cho người dùng.

- Chức năng điều khiển tự động: ví dụ như tưới nước khi độ ẩm dưới mức cho phép.
