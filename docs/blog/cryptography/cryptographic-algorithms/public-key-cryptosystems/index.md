---
template: overrides/blog.html
icon: material/table-edit
title: Public-key Cryptosystems
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

# __Public-key Cryptosystems__ 

<span>
:octicons-calendar-24: June 11, 2021 ·
:octicons-clock-24: ~5 minutes

</span>

---

## __Các kiến thức về toán trừu tượng__

Đề hiểu tốt các thuật toán mã hóa cho mật mã khóa công khai, ta cần trang bị một vài kiến thức về `lý thuyết Nhóm (Group)`, `lý thuyết Vành (Ring)`, `lý thuyết Trường (Field)`.

=== ":material-math-integral: Lý thuyết Nhóm[^*]" 
    > :notepad_spiral: Hiểu nôm na, trong lý thuyết nhóm thì một nhóm là một tập hợp được trang bị 1 phép toán 2 ngôi bất kì để tạo ra 1 phần tử thứ ba thỏa mãn tiên đề nhóm. 
     [^*]: Xem tại https://en.wikipedia.org/wiki/Group_theory
=== ":material-math-log: Lý thuyết Vành[^{**}]"
    > :notepad_spiral: Hiểu nôm na, trong lý thuyết trường thì một vành là một tập hợp được trang bị 2 phép toán: phép toán cộng và phép toán nhân.
     [^{**}]: Xem tại  https://en.wikipedia.org/wiki/Ring_theory
=== ":material-chart-bell-curve-cumulative: Lý thuyết Trường[^{***}]"
    > :notepad_spiral: Hiểu nôm na, trong lý thuyết trường thì một trường là một tập hợp được trang bị 4 phép toán: phép toán cộng, phép toán trừ, phép toán nhân và phép toán chia.  
     [^{***}]: Xem tại https://en.wikipedia.org/wiki/Field_(mathematics)

## __Thông tin về Public-key Cryptosystems__

__Public-key Cryptosystems__ hay còn gọi là __Asymmetric Cryptosystems__ là hệ thống mật mã gồm 2 key (1 private key và 1 public key) khác với hệ thống mật mã đối xứng (chỉ gồm vỏn vẹn 1 private key).

__Private key__ và __public key__ phải được liên kết với nhau thông qua một thuật toán nào đó (tức phụ thuộc lẫn nhau) để giúp mã hóa, giải mã hoặc kí vào một tài liệu nào đó,...

!!! tip "Ưu điểm của mã hóa khóa công khai"
    
    1. Mã hóa khóa công khai an toàn hơn khỏi sự phân tích mật mã so với mã hóa đối xứng.

    2. Mã hóa khóa công khai là một kỹ thuật có mục đích chung đã làm cho mã hóa đối xứng trở nên lỗi thời.

    3. Có cảm giác rằng việc phân phối khóa là tầm thường khi sử dụng mã hóa khóa công khai, so với quá trình bắt tay rườm rà liên quan đến các trung tâm phân phối khóa để mã hóa đối xứng.

Một quy trình của __mã hóa khóa công khai__ gồm 6 thành phần:

> :fontawesome-solid-quote-right:
>
> 1. __Plaintext:__ Là văn bản thô, là một hình ảnh, là một video,... hay bất cứ gì khác có thể dùng làm đầu vào cho thuật toán mã hóa.
> 2. Encryption algorithm:__ Thực hiện các phép biến đổi trên plaintext.
> 3. __Public key:__ Được tạo trước private key và được sử dụng để mã hóa hoặc giải mã.
> 4. __Private key:__ Được tạo sau public key và được sử dụng để mã hóa hoặc giải mã.
> 5. __Ciphertext:__ Bản mã được tạo thông qua plaintext.
> 6. __Decryption algorithm:__ Thực hiện các phép biến đổi từ ciphertext và 1 khóa thích hợp để tạo ra được plaintext ban đầu.
>
>

## __Phân loại Public-key Cryptosystems__

__Hệ thống mã hóa khóa công khai__ được chia thành 3 loại:

> :fontawesome-solid-quote-right:
>
> - Mã hóa/giải mã:  Người gửi mã hóa tin nhắn bằng khóa công khai của người nhận.
> - Chữ ký số: Người gửi “ký” một tin nhắn bằng khóa riêng tư của họ.
> - Trao đổi chính: Hai bên hợp tác trao đổi khóa phiên.

???+ note "Lưu ý"
    Một số thuật toán phù hợp với cả ba ứng dụng, trong khi những thuật toán khác chỉ có thể được sử dụng cho một hoặc hai

## __Yêu cầu ràng buộc khi sử dụng Public key__

Đối với việc sử dung public key, ta có vài yêu cầu sau:

!!! note "Các yêu cầu ràng buộc"
    === "Yêu cầu về mặt tính toán"
        > 1. Về mặt tính toán, bên $B$ có thể dễ dàng tạo một cặp (public key $PU_b$, private key $PR_b$)
        > 2. Về mặt tính toán, bên người gửi $A$ biết khóa công khai $PU_b$ và thông điệp cần được mã hóa $M$ thì dễ dàng tính ra bản mã: $C = E(PU_b, M)$
        > 3. Về mặt tính toán, bên $B$ có thể dễ dàng giải mã ciphertext bằng cách sử dụng private key $PR_b$ để tìm lại plaintext (M): $M = D(PR_b, C) = D\big[PR_b, E(PU_b, M)\big]$
        > 4. Về mặt tính toán, attacker biết $PU_b$ thì việc tìm $PR_b$ về mặt tính toán là bất khả thi.
        > 5. Về mặt tính toán, attacker biết $PU_b$ và ciphertext (C) thì việc tìm plaintext (M) là điều bất khả thi.
        > 6. Hai key có thể được áp dụng theo hai thứ tự sau để tìm plaintext: $M = D\big[PU_b , E(PR_b, M)\big] = D\big[PR_b, E(PU_b, M)\big]$
    === "Yêu cầu về one-way function"
        1. __One-way function__ là hàm ánh xạ miền giá trị đầu vào thành miền giá trị đầu ra sao cho mọi giá trị của hàm có 1 nghịch ảnh duy nhất với điều kiện là việc tính toán trên hàm thì dễ dàng nhưng việc tìm nghịch ảnh là `khó`. Từ `khó` ở đây được định nghĩa dựa theo [computational complexity theory](https://en.wikipedia.org/wiki/Computational_complexity_theory "In computer science and mathematics, computational complexity theory focuses on classifying computational problems according to their resource usage, and relating these classes to each other. A computational problem is a task solved by a computer. A computation problem is solvable by...")
            - $Y = f(X)$ tính dễ 
            - $X = f^{-1}(Y)$ tính khó 
        2. __Trap-door one-way function__ là họ các hàm có thể tìm nghịch ảnh $f_k$ sao cho:
            - $Y = f_k(X)$ tính dễ nếu biết $k$ và $X$
            - $X = f_k^{-1}(Y)$ tính dễ nếu biết $k$ và $Y$
            - $X = f_k^{-1}(Y)$ tính khó nếu biết $Y$ mà không biết $k$

## __Đánh giá về Public-key Cryptosystems__

Dựa vào đặc trưng của __Public-key Cryptosystems__, ta có vài đánh giá như sau:

1. Kích thước khóa phải đủ nhỏ để tăng tốc độ của quá trình mã hóa và giải mã. Điều này đồng nghĩa với việc hệ mã hóa này dễ bị Brute Force nếu kích thước không đủ `lớn`.
2. Public-key encryption hiện chỉ giới hạn trong các ứng dụng quản lý khóa và chữ ký.
3. Một hình thức tấn công khác là tìm một số cách để tính toán private key với public key:
    - Cho đến nay, nó vẫn chưa được chứng minh về mặt toán học rằng hình thức tấn công này là không khả thi đối với một thuật toán khóa công khai cụ thể.
4. Tấn công Message:
    - Cuộc tấn công này có thể được ngăn chặn bằng cách thêm một số bit ngẫu nhiên vào các message đơn giản.
    > :fontawesome-solid-quote-right: Trích dẫn:
    >
    > Suppose, for example, that a message were to be sent that consisted solely of a 56-bit DES key. An adversary could encrypt all possible 56-bit DES keys using the public key and could discover the encrypted key by matching the transmitted ciphertext. Thus, no matter how large the key size of the public-key scheme, the attack is reduced to a brute-force attack on a 56-bit key. This attack can be thwarted by appending some random bits to such simple messages.


## __Những bài toán lớn tiền đề cho Public-key cryptography__

### __Factoring Based Cryptography__

Mật mã này bắt nguồn từ __Prime factorization problem__ (vấn đề được hiểu nôm na là phân tích một hợp số thành tích của nhiều số nguyên tố[^1])
 [^1]: Xem tại https://en.wikipedia.org/wiki/Integer_factorization

=== ":material-hexagon-multiple-outline:{ .example-nor } Ví dụ"
    !!! example "Ví dụ"
        Ta có thể phân tích số $n = 9$ thành $n = 3.3$ hoặc phân tích số $n = 42$ thành $n = 2.3.7$. 
        
=== ":question: Câu hỏi"
    !!! question "Câu hỏi"
        Làm thế nào để ta có thể phân tích một số $n \in \mathbb{Z^*}$ đủ lớn thành dạng $n  = p_1^{\alpha_1}.p_2^{\alpha_2}...p_n^{\alpha_n}$
=== ":material-reply:{ .reply-nor } Trả lời câu hỏi"
    !!! success "Trả lời"
        Với một số $n \in \mathbb{Z^*}$ đủ lớn như vậy nhưng ta cũng sẽ có một vài giải thuật như __Miller–Rabin primality test__ (1) để kiểm tra số nguyên tố với một xác suất chính xác nào đó hoặc dùng __Euclidean factorial algorithms__  (2) để xác định chính xác xem số đó có phải số nguyên tố hay không. Nhưng các cách trên sẽ bị `lâu` khi $n$ là 1 số đủ lớn để có thể làm `lâu` các cách đó. 

        Với trường hợp làm `lâu` như trên xảy ra, ta có một công cụ nữa đó chính là xây dựng 1 thư viện chứa tệp số nguyên tố đủ lớn cho trước và chỉ cần rà soát các số trong thư viện để xem xét đến kết quả.  

    1.  :man_raising_hand: Xem tại https://en.wikipedia.org/wiki/Miller%E2%80%93Rabin_primality_test
    2.  :woman_raising_hand: Xem tại https://en.wikipedia.org/wiki/Euclidean_division

=== ":material-comment-quote-outline:{ .comment-nor } Nhận xét và hướng giải quyết"
    !!! quote "Nhận xét"
        Câu hỏi cũng chính là một câu hỏi thách thức, một bài toán đố và cũng chính là phần cốt lõi để tạo ra các thuật toán mã hóa dựa vào vấn đề phân tích số nguyên tố. Một ứng cử viên điển hình cho thuật toán mã hóa này chính là __Rivest-Shamir-Adleman (RSA) Algorithm[^2]__.

        Trên thực tế: 
        
        - Nếu số $n$ có số bit đủ nhỏ thì khi dùng __quantumn computer__ ta có thể dễ dàng phá mã RSA trong tích tắc. 
        - Nếu số $n$ có số bit đủ lớn thì khi dùng __quantumn computer__ ta cũng có thể dễ dàng phá mã RSA trong thời gian `lâu` hơn. 
        
        Hay nói cách khác, về mặc bản chất thì RSA luôn luôn bị phá mã khác với việc dùng AES (là một mã hóa đối xứng mà __quantumn computer__ không thể phá được). Vấn đề là thời gian để phá mã. 
    !!! tip "Hướng giải quyết"
        Như những phân tích trên, chỉ cần tạo được một số $n$ với số bit đủ lớn thì sẽ khiến việc phá mã đủ lâu. Và theo tôi thì đó cũng là hướng giải quyết duy nhất đến thời điểm bài post này được đăng.

 [^2]: Xem tại https://hieuhdh.github.io/blog/cryptography/cryptographic-algorithms/public-key-cryptosystems/2020-12-17-rsa/

> :material-comment-quote-outline:{ .comment-nor } Độ an toàn của __Factoring Based Cryptography__ dựa vào bài toán __phân tích số__.

### __Logarithm Based Cryptography__

Mật mã này bắt nguồn từ __Discrete Logarithm problem__ (vấn đề được hiểu nôm na là xét độ khó để giải bài toán logarit rời rạc[^3])
 [^3]: Xem tại https://en.wikipedia.org/wiki/Discrete_logarithm

=== ":material-hexagon-multiple-outline:{ .example-nor } Ví dụ và đặt vấn đề"
    !!! example "Ví dụ"
        Với 1 nhóm đa thức $G$ được trang bị phép toán $.$ cho trước sao cho: $(G, .) = <g> = {g^n:n \in \mathbb{Z}}$ thì dễ dàng tạo 1 ánh xạ từ $g, n$ sang $y_0$ sao cho $y_0 = g^n \ mod \ p$ nhưng việc ta có được $g, y_0, p$ thì hoàn toàn khó tìm được ngược ảnh $n$ sao cho $g^n = y_0 \ mod \ p$
=== ":material-comment-quote-outline:{ .comment-nor } Nhận xét và hướng giải quyết"
    !!! quote "Nhận xét"
        Dựa vào độ khó của việc giải bài toán trên, một ứng cử viên điển hình cho thuật toán mã hóa này chính là __ElGamal cipher[^4]__.
         [^4]: Xem tại https://hieuhdh.github.io/blog/cryptography/cryptographic-algorithms/public-key-cryptosystems/2021-05-11-elgamal/

> :material-comment-quote-outline:{ .comment-nor } Độ an toàn của __Logarithm Based Cryptography__ dựa vào bài toán logarit rời rạc trên __trường hữu hạn__.

### __Elliptic Curve Cryptography__

Đây là loại mật mã dựa trên nhóm đường cong Elliptic[^a]
 [^a]: Xem tại https://hieuhdh.github.io/blog/cryptography/cryptographic-algorithms/public-key-cryptosystems/2021-05-13-elliptic-curve

Đối với __Factoring Based Cryptography__ và __Logarithm Based Cryptography__ thì độ phức tạp quá lớn cũng như để đảm bảo tính an toàn của mật mã trong kỉ nguyên lượng tử thì đòi hỏi kích cỡ khóa cũng khá lớn khiến cho nhiều hạ tầng như QR code,... không thể chứa được dữ liệu của khóa. Vì vậy, cần có một thuật toán tiên tiến hơn và làm giảm kích cỡ khóa lại nhưng phải có tính an toàn cao - nhất là trong thời đại của kỉ nguyên lượng tử hiện nay. Và đó cũng là điều kiện cho __Elliptic Curve Cryptography__ ra đời.

> :material-comment-quote-outline:{ .comment-nor } Độ an toàn của __Elliptic Curve Cryptography (ECC)__ dựa vào bài toán logarit rời rạc trên __nhóm các điểm của đường cong elliptic (ECDLP)__. 

Đến thời điểm đăng post này, vẫn chưa tìm được thuật toán dưới dạng hàm mũ để giải bài toán ECDLP.

### __Bài toán về vấn đề về trao đổi khóa giữa các bên__

!!! question "Câu hỏi"
    Với __Sysmetric Cryptosystems__:

    - Làm sao để có bảo mật thông tin liên lạc giữa 2 hay nhiều người dùng?
    - Với một thông điệp (thông điệp ở đây không mang nghĩa là những tài liệu trên giấy,...) đến từ một người nào đó, làm thế nào ta có thể xác định được tính toàn vẹn của thông điệp từ lúc họ vận chuyển thông điệp đến lúc thông điệp đến nơi của ta?

Vào năm 1976, Whitfield Diffie và Martin Hellman từ Đại học Stanford đã đạt được một bước đột phá bằng cách đưa ra một phương pháp giải quyết cả hai vấn đề và hoàn toàn khác với tất cả các cách tiếp cận trước đây đối với mật mã. Đó là phương pháp trao đổi khóa __Diffie-Hellman__[^5]
 [^5]: Xem thêm tại http://hieuhdh.github.io/blog/cryptography/cryptographic-algorithms/public-key-cryptosystems/2021-05-14-diffie-hellman/

## __Ưu và nhược điểm của Public-key Cryptosystems__

Ưu và nhược điểm của __Public-key Cryptosystems__ nằm trong bảng phân loại sau:

=== "Ưu điểm"
    !!! abstract ""
        1. Bảo mật mà không chia sẻ bí mật
        2. Xác thực mà không cần chia sẻ bí mật...
=== "Nhược điểm"
    !!! warning ""
        1. Tính toán chậm, lâu.
        2. Dùng những giả định của lý thuyết số chưa được chứng minh như: Factoring, RSA problem, discrete logarithm problem, decisional Diffie-Hellman problem…