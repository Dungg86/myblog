---
title: "Identifier"
date: "2025-05-18"
updated: "2025-05-18"
categories:
  - "ứng dụng phân tán"
coverImage: "/images/identifier.jpg"
coverWidth: 16
coverHeight: 9
excerpt: 
---
## Content
- [Quy trình duyệt trang web www.motvidu.com](#quy-trình-duyệt-trang-web-wwwmotviducom)
    - [Trình duyệt kiểm tra cache cục bộ (Local DNS Cache)](#trình-duyệt-kiểm-tra-cache-cục-bộ-local-dns-cache)
    - [Gửi yêu cầu tra cứu DNS (DNS Lookup)](#gửi-yêu-cầu-tra-cứu-dns-dns-lookup)
    - [Quy trình phân giải DNS (DNS Resolving)](#quy-trình-phân-giải-dns-dns-resolving)
    - [Caching](#caching)
    - [Kết nối tới máy chủ](#kết-nối-tới-máy-chủ)
- [Thuật toán Chord](#thuật-toán-chord)
    - [Mục tiêu](#mục-tiêu)
    - [Ví dụ cụ thể về Chord](#ví-dụ-cụ-thể-về-chord)
    - [Cách tính toán test case](#cách-tính-toán-test-case)
    - [Code thuật toán Chord (Python)](#code-thuật-toán-chord-python)
    - [Test case](#test-case)
    - [Kết quả thực nghiệm](#kết-quả-thực-nghiệm)

---

## Quy trình duyệt trang web www.motvidu.com
---
#### Trình duyệt kiểm tra cache cục bộ (Local DNS Cache)

Trình duyệt hoặc hệ điều hành sẽ kiểm tra xem trước đó có truy cập địa chỉ này chưa, nếu đã có thì lấy địa chỉ IP đã lưu trong bộ nhớ đệm (cache) và dùng luôn.

- Nếu tìm thấy IP → bỏ qua bước tra cứu DNS.
- Nếu không tìm thấy IP → tiếp tục bước sau.

#### Gửi yêu cầu tra cứu DNS (DNS Lookup)

Trình duyệt hỏi hệ điều hành: “IP của `www.motvidu.com` là gì?”

Hệ điều hành hỏi DNS Resolver (máy chủ phân giải DNS – thường là của nhà mạng).

#### Quy trình phân giải DNS (DNS Resolving)

Giả sử IP chưa được cache ở resolver, quá trình phân giải sẽ như sau:

- **Hỏi Root DNS Server**: Root server trả lời: “Tên miền `.com` thì hỏi máy chủ tên miền cấp cao `.com` (TLD Server)”
- **Hỏi TLD Server (.com)**: “Tên miền `motvidu.com` do server nào quản lý thì hỏi tại đó” (Authoritative DNS)
- **Hỏi Authoritative DNS Server**: Trả lời: “IP chính xác của `www.motvidu.com` là `123.45.67.89` (ví dụ)”

#### Caching

Tất cả các bước trên đều có thể được cache (lưu tạm) để giảm thời gian truy cập lần sau:

- Trình duyệt cache
- Hệ điều hành cache
- DNS resolver cache
- Trình ghi nhớ thời gian cache dựa trên TTL (Time To Live)

#### Kết nối tới máy chủ

Sau khi có IP, trình duyệt dùng giao thức TCP (hoặc HTTPS nếu bảo mật) để gửi yêu cầu (HTTP request) đến máy chủ chứa trang web tại địa chỉ IP đó.

Máy chủ trả về nội dung trang web (HTML, CSS, JS…)

## Thuật toán Chord
---
#### Mục tiêu

Hiểu và cài đặt thuật toán Chord – một thuật toán phân tán dạng vòng (ring-based DHT – Distributed Hash Table), giúp định vị nhanh chóng node chứa khóa trong hệ thống phân tán.

#### Ví dụ cụ thể về Chord

Giả sử hệ thống có:

- M = 3 (ID được mã hóa trong không gian 0 → 7).
- Các node đang có: `N1=1`, `N3=3`, `N6=6`.

a chèn một key = 5 → Chord tìm node gần nhất lớn hơn hoặc bằng 5, chính là 6.

Vậy key 5 được lưu tại node 6.

#### Cách tính toán test case

Khi tìm vị trí lưu key `k`, ta tìm node có `ID lớn hơn hoặc bằng k` gần nhất theo vòng. Nếu không có, vòng quay về node có ID nhỏ nhất.

VD: Với các node `[1, 3, 6]`

- key = 0 → lưu ở 1
- key = 2 → lưu ở 3
- key = 5 → lưu ở 6
- key = 7 → không ai lớn hơn → vòng về 1

#### Code thuật toán Chord (Python)

```python
class Node:
    def __init__(self, node_id):
        self.id = node_id

class ChordRing:
    def __init__(self, m_bits):
        self.m = m_bits
        self.size = 2 ** m_bits
        self.nodes = []

    def add_node(self, node_id):
        if node_id not in [node.id for node in self.nodes]:
            self.nodes.append(Node(node_id))
            self.nodes.sort(key=lambda node: node.id)

    def find_successor(self, key):
        for node in self.nodes:
            if key <= node.id:
                return node.id
        return self.nodes[0].id  # vòng lại đầu vòng

    def __str__(self):
        return 'Chord ring: [' + ', '.join(str(n.id) for n in self.nodes) + ']'
```

#### Test case

```python
def test_chord():
    ring = ChordRing(m_bits=3)
    ring.add_node(1)
    ring.add_node(3)
    ring.add_node(6)

    print(ring)
    test_keys = [0, 2, 5, 7]
    expected = [1, 3, 6, 1]

    for key, exp in zip(test_keys, expected):
        res = ring.find_successor(key)
        print(f"Key {key} -> Node {res} (Expected: {exp})")
        assert res == exp

test_chord()
```
#### Kết quả thực nghiệm

```bash
Chord ring: [1, 3, 6]
Key 0 -> Node 1 (Expected: 1)
Key 2 -> Node 3 (Expected: 3)
Key 5 -> Node 6 (Expected: 6)
Key 7 -> Node 1 (Expected: 1)
```