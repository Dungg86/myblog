---
title: "Develop distributed application"
date: "2025-05-18"
updated: "2025-05-18"
categories:
  - "ứng dụng phân tán"
coverImage: "/images/udpt.jpeg"
coverWidth: 16
coverHeight: 9
excerpt: 
---
## Content

- [Các giao thức](#các-giao-thức)
    - [HTTP](#http)
    - [TCP/IP](#tcpip)
    - [UDP](#udp)
    - [REST (Representational State Transfer)](#rest-representational-state-transfer)
    - [GraphQL](#graphql)
    - [SOAP (Simple Object Access Protocol)](#soap-simple-object-access-protocol)
    - [AJAX (Asynchronous JavaScript and XML)](#ajax-asynchronous-javascript-and-xml)
    - [RPC (Remote Procedure Call)](#rpc-remote-procedure-call)
    - [gRPC](#grpc)
- [Tìm hiểu thư viện OpenMPI](#tìm-hiểu-thư-viện-openmpi)
    - [Tính năng chính của OpenMPI](#tính-năng-chính-của-openmpi)
    - [Một số hàm MPI thường dùng](#một-số-hàm-mpi-thường-dùng)
- [Ý tưởng giải bài toán: Tính 10,000,000 số nguyên tố đầu tiên bằng 32 core](#ý-tưởng-giải-bài-toán-tính-10000000-số-nguyên-tố-đầu-tiên-bằng-32-core)
    - [1. Chia công việc](#1-chia-công-việc)
    - [2. Cách chia](#2-cách-chia)
    - [3. Cách tính](#3-cách-tính)
    - [4. Gom kết quả](#4-gom-kết-quả)
    - [Mục tiêu](#mục-tiêu)
    - [Cách làm linh hoạt với số core](#cách-làm-linh-hoạt-với-số-core)

---

## Các giao thức
---
#### HTTP

Là giao thức truyền tải siêu văn bản, dùng để truyền dữ liệu giữa client và server trên web. Nó chạy trên giao thức TCP/IP. Gần như tất cả các trang web đều sử dụng HTTP hoặc HTTPS.

#### TCP/IP

Là bộ giao thức nền tảng của Internet, trong đó:

- TCP (Transmission Control Protocol) đảm bảo truyền dữ liệu đáng tin cậy, đúng thứ tự.
- IP (Internet Protocol) định tuyến dữ liệu qua Internet.

#### UDP

Là giao thức truyền dữ liệu nhanh nhưng không đảm bảo như TCP. Phù hợp cho các ứng dụng cần tốc độ như video call, game online, livestream.

#### REST (Representational State Transfer)

Là kiến trúc API sử dụng HTTP. Dữ liệu thường được gửi/nhận qua JSON. REST đơn giản, dễ dùng, phổ biến trong các hệ thống web hiện đại.

#### GraphQL

Là ngôn ngữ truy vấn API do Facebook phát triển. Thay vì nhiều endpoint như REST, chỉ cần một endpoint và client có thể truy vấn đúng dữ liệu cần. Hiệu quả hơn REST trong nhiều trường hợp.

#### SOAP (Simple Object Access Protocol)

Là giao thức nhắn tin dựa trên XML, thường dùng trong các hệ thống lớn, yêu cầu bảo mật cao. Cứng nhắc, nặng nề hơn REST. Chạy trên HTTP hoặc các tầng thấp khác.

#### AJAX (Asynchronous JavaScript and XML)

Không phải là giao thức mà là kỹ thuật trong web giúp gửi/nhận dữ liệu bất đồng bộ mà không cần tải lại trang. Dưới nền AJAX có thể dùng HTTP + REST hoặc GraphQL.

#### RPC (Remote Procedure Call)

Là phương pháp cho phép gọi hàm từ xa như thể gọi trong cùng chương trình. Không quan tâm đến việc truyền dữ liệu, mà tập trung vào việc gọi "hàm".

#### gRPC

Là phiên bản hiện đại của RPC do Google phát triển. Sử dụng HTTP/2, protobuf (thay JSON), nhanh hơn, tối ưu hơn REST. Phù hợp cho microservices, backend systems.

## Tìm hiểu thư viện OpenMPI
---
OpenMPI (Open Message Passing Interface) là một thư viện hỗ trợ lập trình song song phân tán theo mô hình message-passing. Nó được sử dụng rộng rãi trong các hệ thống HPC (High Performance Computing), cluster hoặc hệ thống nhiều máy.

#### Tính năng chính của OpenMPI:

- Giao tiếp giữa nhiều tiến trình chạy trên nhiều máy qua mạng (distributed memory).
- Tự động chia công việc (load balancing).
- Hỗ trợ đa nhân, đa máy, mở rộng dễ dàng.
- Có thể chạy trên cluster thật hoặc mô phỏng bằng nhiều tiến trình trên 1 máy.
- Dùng chuẩn MPI, có thể viết bằng C/C++/Fortran.

#### Một số hàm MPI thường dùng:

- `MPI_Init()`: Khởi tạo môi trường MPI.
- `MPI_Comm_rank()`: Lấy ID (rank) của tiến trình hiện tại.
- `MPI_Comm_size()`: Tổng số tiến trình đang chạy.
- `MPI_Send()` / `MPI_Recv()`: Gửi và nhận dữ liệu giữa các tiến trình.
- `MPI_Bcast()`: Phát dữ liệu từ một tiến trình cho tất cả tiến trình khác.
- `MPI_Reduce()` / `MPI_Gather()`: Gom kết quả từ nhiều tiến trình.
- `MPI_Finalize()`: Kết thúc chương trình MPI.

## Ý tưởng giải bài toán: Tính 10,000,000 số nguyên tố đầu tiên bằng 32 core
---
#### 1. Chia công việc:

- Mỗi core là 1 tiến trình.
- Tổng cộng 32 tiến trình sẽ chia đều khối lượng công việc.
- Mỗi tiến trình tính một phần dãy nguyên tố, rồi gửi kết quả về tiến trình master (rank 0).

#### 2. Cách chia:

- Dùng MPI để lấy `rank` của mỗi tiến trình.
- Mỗi tiến trình tính từ `start = rank * block_size` đến `end = (rank + 1) * block_size`.
- Số lượng cần chia = 10,000,000.
- Ví dụ nếu `total_process = 32`, mỗi tiến trình sẽ tính khoảng 312,500 số nguyên tố đầu tiên.

#### 3. Cách tính:

- Dùng Sieve of Eratosthenes (sàng Eratosthenes) hoặc trial division để tìm số nguyên tố.
- Mỗi tiến trình chạy thuật toán riêng không phụ thuộc nhau.

#### 4. Gom kết quả:

- Sau khi tính, các tiến trình gửi kết quả về tiến trình chính bằng `MPI_Gather()` hoặc `MPI_Send()` + `MPI_Recv()`.

---

### Mục tiêu:

- Tính đúng: đảm bảo các tiến trình không tính trùng và gom đúng kết quả.
- Tối ưu tốc độ: chia đều, tránh quá tải tiến trình nào.
- Tính linh hoạt: thay đổi số core tùy ý, chương trình vẫn chạy đúng.

### Cách làm linh hoạt với số core:

- Không hard-code số core. Thay vào đó, dùng:

```c
MPI_Comm_size(MPI_COMM_WORLD, &size); // tổng tiến trình
MPI_Comm_rank(MPI_COMM_WORLD, &rank); // ID của tiến trình
```

- Dựa vào size để chia đoạn dữ liệu.
- Như vậy dù cậu chạy với 8, 12, 16 hay 32 core, chương trình vẫn chia đều và chạy đúng.