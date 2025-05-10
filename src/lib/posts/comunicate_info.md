---
title: "Communicate Infomation"
date: "2025-05-07"
updated: "2025-05-07"
categories:
  - "communicate"
coverImage: "/images/communicate_info.jpg"
coverWidth: 16
coverHeight: 9
excerpt: Các giao thức trao đổi thông tin phổ biến trong hệ phân tán.
---

## Content
- [**RabbitMQ**](#rabbitmq)
    - [**Tổng quan về RabbitMQ**](#tổng-quan-về-rabbitmq)
    - [**Các thành phần cơ bản**](#các-thành-phần-cơ-bản)
    - [**Chế độ hoạt động**](#chế-độ-hoạt-động)
- [**Thiết lập hệ thống đơn giản bằng RabbitMQ**](#thiết-lập-hệ-thống-đơn-giản-bằng-rabbitmq)
    - [**Mục tiêu chung**](#mục-tiêu-chung)
    - [**Các phụ thuộc**](#các-phụ-thuộc)
    - [**Các bước cấu hình (đối với linux dòng debian)**](#các-bước-cấu-hình-đối-với-linux-dòng-debian)
- [**Thiết lập giao thức RPC với JSON-RPC**](#thiết-lập-giao-thức-rpc-với-json-rpc)
    - [**Mô tả chung**](#mô-tả-chung)
    - [**Các bước thiết lập (đối với linux dòng debian)**](#các-bước-thiết-lập-đối-với-linux-dòng-debian)    
- [**Tài liệu tham khảo**](#tài-liệu-tham-khảo)

## RabbitMQ

Trong lập trình hiện tại, các hệ thống ngày càng phân tán để tận dụng sức mạnh của nhiều máy tính. Nhu cầu trao đổi thông tin hiệu quả trong hệ phân tán ngày càng cao.

RabbitMQ là một giải pháp cho bài toán trao đổi thông tin giữa các máy tính/chương trình.

#### Tổng quan về RabbitMQ

RabbitMQ là một mesage broker (người vận chuyển thông điệp) giữa các node trong hệ phân tán, được viết bằng Erlang năm 2007.

RabbitMQ là một trong những message broker đầu tiên triển khai giao thức AMQP (Advanced Message Queuing Protocol).

---

#### Các thành phần cơ bản

- **Message**: Đơn vị cơ bản để trao đổi thông tin, bao gồm:

    - **Body**: Mang thông tin cần trao đổi.
    - **Property**: Thông tin đi kèm giúp định dạng tin nhắn.
    
- **Producers**: Người gửi message.
- **Consumers**: Người nhận message.
- **Queues**: Lưu trữ message cho đến khi chúng được tiêu thụ bởi consumers.
- **Binding**: Thiết lập liên kết giữa exchange và queue.
- **Exchanges**: Định hướng message đến queue cụ thể. Đóng vài trò như bộ định tuyến/người dẫn đường cho message. Có 5 loại exchanges khác nhau. Mỗi exchanges đều có quy tắc định tuyến riêng.

---

##### Direct exchange

Hoạt động theo nguyên tắc **key - value** dựa trên **routing key**. Nếu **routing key** của message khớp với **routing key** của queue, message được phân phối đến queue đó. Ngược lại, nếu **routing key** của message không khớp bất kỳ queue nào, message bị loại bỏ

![direct exchange](/images/communicate_info1.png)

Ở hình trên, nếu một message có routing key là **pdf_create** sẽ được exchange **pdf_event** chuyển đến queue **create_pdf_queue** thông qua binding key **pdf_create** giữa exchange và queue.

##### Default exchange

Exchange mặc định cho phép message truyền thẳng tới queue. Queue mặc định sẽ có routing key giống routing key của message. Điều này cho phép tất cả message đều được lưu trữ vào queue nào đó.

##### Fanout exchange

Message được phân phối đến tất cả queue liên kết với exchange đó. Nói đơn giản, không cần biết routing key của message như nào, message chỉ cần đi qua exchange nhất định đều được phân phối đến tất cả queue liên kết với exchange đó.

![fanout exchange](/images/communicate_info2.png)

Bất kể message có routing key như nào, cứ đi qua exchange **sport news** đều được chuyển đến queue A, B và C.

##### Topic exchange

Phân chia message dựa trên topic.

Hình dung đơn giản, direct exchange định tuyến message tương tự style cho **id** trong CSS, topic exchange phân chia message tương tự **class** trong CSS. Bất kể id như nào, miễn thuộc về cùng một topic, đều được phân phối tới queue đó.

Sử dụng ký pháp **#** để đại diện cho **1** ký tự, ký pháp ***** để đại diện cho **0** hoặc **nhiều** ký tự.

![topic exchange](/images/communicate_info3.png)

Message có routing key agreements.eu.berlin được gửi tới exchange agreements. Message này sẽ được định tuyến tới queue A berlin_agreements vì routing pattern khớp với binding key agreements.eu.berlin.#.

Đồng thời, message này cũng được gửi tới queue B vì khớp với binding key agreements.#.

##### Header exchange

Tương tự direct exchange, chỉ khác ở chỗ thay vì định tuyến bằng **routing key**, header exchange định tuyến bằng **header field** của message.

![header exchange](/images/communicate_info3.png)

Ví dụ, ở hình trên, chúng ta có

- **Message 1**: header có dạng key: value là format: pdf, type: report
- **Message 2**: header có dạng key: value là format: pdf
- **Message 3**: header có dạng key: value là format: zip, type: log
- **Exchange**: aggreements
- **Queue A**
- **Queue B**
- **Queue C**

---

- **Binding 1 giữa exchange và Queue A**: có dạng key: value là format: pdf, type: report, x-match: all
- **Binding 2 giữa exchange và Queue B**: có dạng key: value là format: pdf, type: log, x-match: any
- **Binding 3 giữa exchange và Queue C**: có dạng key: value là format: zip, type: report, x-match: all

Tham số x-match trong binding xác định cách so sánh các cặp key/value của header trong message với header của binding. Có 2 giá trị cho tham số x-match là any và all.

- **x-match: any**: chỉ cần một thuộc tính trong header của message phải khớp với header của binding.
- **x-match: all**: tất cả các thuộc tính trong header của message phải khớp với header của binding.

Message 1 sẽ được định tuyến tới queue A vì tất cả cặp key/value trong header của message đều khớp với binding.

Message 1 sẽ được định tuyến tới queue B vì có cặp key/value format: pdf trùng và thuộc tính x-match: any yêu cầu chỉ cần một thuộc tính trong header khớp dù message 1 có type: report không khớp với type: log của binding 1.

Message 2 sẽ được định tuyến tới queue B vì khớp format: pdf.

Message 3 sẽ được định tuyến tới queue B vì type: log.

---

#### Chế độ hoạt động

Consumer tiêu thụ message thông qua 2 cơ chế chính:

- **push (default)**: Hệ thống push message từ queue tới consumer.
- **pull**: Consumer pull message từ queue.

##### Cơ chế push

Là chế độ hoạt động mặc định của RabbitMQ. RabbitMQ đẩy message tới consumer bất cứ lúc nào có message mới. Tuy nhiên, điều này không phải lúc nào cũng tốt vì mỗi consumer có mức độ tiêu thụ message khác nhau.

##### Cơ chế pull

Consumer chủ động gửi yêu cầu **Basic.Get** để poll message từ queue và nhận lại message khi queue có message.

Chế độ này phù hợp khi consumer muốn nhận message theo yêu cầu.

Để cái thiện hiệu năng, cần thiết lập tần suất poll phù hợp. Nếu poll quá nhiều trong khi queue không có tin nhắn sẽ dẫn đến tiêu hao chi phí không cần thiết. Nếu poll quá ít trong khi queue có quá nhiều message dẫn đến consumer rảnh dỗi trong khi queue quá tải message gây lãng phí bộ nhớ.

---

## Thiết lập hệ thống đơn giản bằng RabbitMQ

#### Mục tiêu chung

- Một file python đóng vai trò producer.
- Một file python đóng vai trò consumer.
- Giao tiếp thành công message "HW dep trai nhat he mat troi".

#### Các phụ thuộc

- Python với thư viện pika.
- RabbitMQ.

#### Các bước cấu hình (đối với linux dòng debian)

##### Cài đặt python3

```bash
sudo apt update
sudo apt install python3 python3-pip -y
sudo apt install python3.12-venv -y

```

##### Tạo môi trường ảo

```bash
python3 -m venv myenv
source myenv/bin/activate

```

##### Cài đặt thư viện pika

```bash
pip install pika

```

##### Cài đặt Erlang

```bash
sudo apt install erlang -y

```

##### Cài đặt RabbitMQ

```bash
sudo apt install rabbitmq-server -y

```

##### Tạo file send.py (producer - gửi message) với nội dung:

```python
import pika

# connect to rabbitMQ
connection = pika.BlockingConnection(pika.ConnectionParameters('localhost'))
channel = connection.channel()

# create queue
channel.queue_declare(queue='clm')

# send message
channel.basic_publish(exchange='', routing_key='clm', body='HW dep trai nhat he mat troi')

print("message sent!")

connection.close()
```

##### Tạo file receive.py với nội dung:

```python
import pika

# connect to rabbitMQ
connection = pika.BlockingConnection(pika.ConnectionParameters('localhost'))
channel = connection.channel()

channel.queue_declare(queue='clm')

# function handle message when receive
def callback(ch, method, properties, body):
    print(f"content of message: {body.decode()}")
    
# sign up callback function for queue "clm"
channel.basic_consume(queue='clm', on_message_callback=callback, auto_ack=True)

print("loading message...enter ctrl + c to exit!")
channel.start_consuming()

```

##### Chạy hệ thống

Mở 2 terminal

- Terminal 1 chạy file receive.py
- Terminal 2 chạy file send.py

**Lệnh chạy file .py:** 

```bash
python3 file_name.py

```

**Kết quả**: Terminal 1 sẽ in ra "HW dep trai nhat he mat troi"

---

## Thiết lập giao thức RPC với JSON-RPC

#### Mô tả chung

RPC (Remote Procedure Call) là một giao thức cho phép gọi hàm từ xa thông qua mạng. Client có thể gọi hàm từ server.

Hiểu đơn giản, trong hệ thống phân tán, giao thức này cho phép một node gọi một thủ tục từ node khác. Node sẽ gửi các tham số tương ứng với thủ tục cần gọi và nhận về ouput tương ứng. Ở đây, chúng ta sử dụng định dạng JSON (JavaScript Object Notation) để trao đổi dữ liệu.

Đơn giản hóa, khi lập trình C, ta khai báo và định nghĩa hàm `sum()` ở file `clm.h`. Ở file `main.h`, muốn sử dụng hàm `sum()`, ta không cần định nghĩa lại mà có thể gọi trực tiếp hàm `sum()` ở file `clm.h` thông qua khai báo `#include`. Đó cũng có thể coi là một RPC

Ở đây, chúng ta sẽ tạo 1 file `server.py` đóng vai trò như một **server** chứa hàm `square`, 1 file `client.py` đóng vai trò như một **client** để gọi `square()` từ **server**

#### Các bước thiết lập (đối với linux dòng debian)

##### Cài đặt thư viện `jsonrpclib`

```bash
source myenv/bin/active
pip install jsonrpclib

```

##### Tạo file `server.py`

```python
from jsonrpclib.SimpleJSONRPCServer import SimpleJSONRPCServer
import math

# init server RPC using JSON
server = SimpleJSONRPCServer(('localhost', 8080))
print("RPC-JSON is running in localhost: 8080...")

# define square function
def square(x):
    if (x < 0):
        return "error! cannt not square for positive number!"
    return math.sqrt(x);
    
# register
server.register_function(square, 'square')

server.serve_forever()

```

##### Tạo file `client.py`

```python
import jsonrpclib

# connect to server at localhost: 8080
server = jsonrpclib.Server('http://localhost:8080')

# call function remote
print(f"square of -1: {server.square(-1)}")
print(f"square of 9: {server.square(9)}")
print(f"square of 99: {server.square(99):.2f}")

```

#### Cách chạy

Mở 2 terminal

- Terminal 1 chạy file `server.py`
- Terminal 2 chạy file `client.py`

**Kết quả**:

- Dòng đầu tiên của terminal 2 thông báo lỗi: `error! cannt not square for positive number!`
- Dòng thứ hai của terminal 2 hiển thị: `3`
- Dòng thứ ba của terminal 2 hiển thị: `9.95`

---

### Tài liệu tham khảo

- [*https://200lab.io/blog/rabbitmq-la-gi*](https://200lab.io/blog/rabbitmq-la-gi)
- [*https://viblo.asia/p/tim-hieu-giao-thuc-tcp-va-udp-jvEla11xlkw*](https://viblo.asia/p/tim-hieu-giao-thuc-tcp-va-udp-jvEla11xlkw)
- [*https://viblo.asia/p/tim-hieu-ve-rabbitmq-OeVKB8bMlkW*](https://viblo.asia/p/tim-hieu-ve-rabbitmq-OeVKB8bMlkW)

