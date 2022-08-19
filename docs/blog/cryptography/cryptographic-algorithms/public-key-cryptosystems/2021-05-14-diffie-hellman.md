---
template: overrides/blog.html
icon: material/table-edit
title: Diffie-Hellman key exchange
description: >
  
search:
  exclude: true
hide:
  - feedback

tags:
  - Cryptography 
  - Asymmetric encryption 
  - Public-key 
---

# __Trao đổi khóa Diffie-Hellman__ 

<span>
:octicons-calendar-24: May 14, 2021 ·
:octicons-clock-24: ~5 minutes
<br>

??? info "Updates"
    === ":octicons-calendar-24: Timelines"
        :octicons-arrow-right-24: August 19, 2022:

        - Sửa lỗi hiển thị ở phần [__Man in the middle attack__](#man-in-the-middle-attack)
    === ":material-countertop: Updated statistics"
        - Sửa lỗi hiển thị ở phần [__Man in the middle attack__](#man-in-the-middle-attack)
</span>

---

## __Sự cần thiết của việc trao đổi khóa trong Public-key Cryptosystems__

!!! question "Câu hỏi"
    Với __Sysmetric Cryptosystems__:

    - Làm sao để có bảo mật thông tin liên lạc giữa 2 hay nhiều người dùng?
    - Với một thông điệp (thông điệp ở đây không mang nghĩa là những tài liệu trên giấy,...) đến từ một người nào đó, làm thế nào ta có thể xác định được tính toàn vẹn của thông điệp từ lúc họ vận chuyển thông điệp đến lúc thông điệp đến nơi của ta?

Vào năm 1976, Whitfield Diffie và Martin Hellman từ Đại học Stanford đã đạt được một bước đột phá bằng cách đưa ra một phương pháp giải quyết cả hai vấn đề và hoàn toàn khác với tất cả các cách tiếp cận trước đây đối với mật mã. Đó là phương pháp trao đổi khóa __Diffie-Hellman__[^5]
 [^5]: Xem tại https://en.wikipedia.org/wiki/Diffie%E2%80%93Hellman_key_exchange

## __Vấn đề về trao đổi khóa trong các ngữ cảnh nhất định__

### __Logarithm Based Cryptography__

#### __Mô tả phiên trao đổi khóa__

Với hai người Alice và Bob, vấn đề của __Public-key Cryptosystems__ là mỗi người đều có 1 private key riêng, làm cách nào để cả 2 người cùng sở hữu 1 shared secret key?

Đối với việc dùng __Diffie-Hellman__ trên nhóm __cycle__ và dùng __Logarithm Based Cryptography__ để trao đổi khóa, ta có các bước sau:

1. Alice có khóa bí mật là $a$, Bob có khóa bí mật là $b$. Số $p$ là công khai và là 1 số nguyên tố lớn, g là số được tạo ra từ $Z_p^* = \{1, 2, ..., p-1\}; \ \forall a \in Z_p^*  \ \exists i: a = g^i \ mod \ p$ 
2. Alice tính: $g^a \ mod \ p$ và gửi cho Bob.
3. Bob tính: $g^b \ mod \ p$ và gửi cho Alice.
4. Alice nhận được $g^b \ mod \ p$ và tính $k = {g^b}^a \ mod \ p = g^{ab} \ mod \ p$
5. Bob nhận được $g^a \ mod \ p$ và tính $k = {g^a}^b \ mod \ p = g^{ab} \ mod \ p$
6. Lúc này, cả Alice và Bob đều có chung 1 số k (chính là shared key của cả hai người).

#### __Man in the middle attack__

__Man in the middle attack[^1]__ được hiểu nôm na là có một người attacker ở giữa Alice và Bob nhằm lấy giá trị của $g^a$ và $g^b$ ở bước 2 và bước 3 bên trên và đồng thời tạo ra một giá trị ảo $m$ và thực hiện tính toán $g^m \ mod \ p$ để gửi cho Alice và Bob.
 [^1]: Xem thêm tại https://en.wikipedia.org/wiki/Man-in-the-middle_attack

Lúc này:

- Ở bước 4: Alice nhận được $g^m \ mod \ p$
- Ở bước 5: Bob nhận được $g^m \ mod \ p$
- Lúc này, Alice sẽ có shared key `chung` là $g^{am} \ mod \ p$ và Bob sẽ có shared key `chung` là $g^{bm} \ mod \ p$ và hai người đã khác `shared key` hay nói cách khác thì họ đã bị sai khóa bí mật chung và thông tin họ trao đổi không còn toàn vẹn nữa.

### __Elliptic Curve Cryptosystem__

#### __Mô tả phiên trao đổi khóa__

Đôi với việc dùng nhóm Curve, việc trao đổi khóa diễn ra như sau:

1. Thông tin public: Elliptic curve và điểm $G = (x, y)$ nằm trên curve.
2. Alice có khóa bí mật là $a$, Bob có khóa bí mật là $b$.
3. Alice tính: $Q_A = a.G$ và gửi cho Bob.
4. Bob tính: $Q_B = b.G$ và gửi cho Alice.
5. Alice nhận được $Q_B = b.G$ và tính $k = a.Q_B = abG$
6. Bob nhận được $Q_A = a.G$ và tính $k = b.Q_A = baG = abG$
7. Lúc này, cả Alice và Bob đều có chung 1 số k (chính là shared key của cả hai người).

#### __Man in the middle attack__