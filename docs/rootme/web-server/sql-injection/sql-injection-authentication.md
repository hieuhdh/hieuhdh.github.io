---
template: overrides/blog.html
icon: material/plus-circle
title: SQL injection - Authentication
description: >
  
search:
  exclude: true
hide:
  - feedback

tags:
    - SQL Injection 
---

# __SQL injection - Authentication__

<span>
:octicons-calendar-24: April 28, 2023

</span>

---


## __Tài nguyên và link challenge__

Tài nguyên của challenge này tại [https://www.root-me.org/en/Challenges/Web-Server/SQL-injection-authentication](https://www.root-me.org/en/Challenges/Web-Server/SQL-injection-authentication)

Link challenge này tại [:octicons-arrow-right-24: http://challenge01.root-me.org/web-serveur/ch9/][http://challenge01.root-me.org/web-serveur/ch9/]

  [http://challenge01.root-me.org/web-serveur/ch9/]: http://challenge01.root-me.org/web-serveur/ch9/

## __Tổng quan__

Trong challenge này, mục tiêu của ta là lấy được tài khoản và mật khẩu của người quản trị viên (adnin)

## __Kịch bản tấn công__
### Bước 1: Kiểm tra website

Đầu tiên ta xem xét website

<figure markdown>
  ![Image title](/rootme/web-server/sql-injection/images/sql-injection-01.png#zoom)
  <figcaption>Hình ảnh website challenge</figcaption>
</figure>

Ta thấy website cung cấp cho ta hộp thoại login nhận tham số đầu vào là username và password. Tiếp theo, ta sẽ vào Burp Suite để kiểm tra gói tin request và response như thế nào với account admin/admin

<figure markdown>
  ![Image title](/rootme/web-server/sql-injection/images/sql-injection-02.png)
  <figcaption>Hình ảnh gói tin request và response</figcaption>
</figure>

Với hình ảnh trên, ta thấy rằng username và password không được mã hóa khi gửi đi, và đây có lẽ là kiểu sql injection đơn giản nhất.

Lúc này, có vẻ như bên phía server họ code logic theo dạng

<div class="result" markdown>

``` sql linenums="1"
SELECT * FROM Users WHERE username ='admin' AND password ='admin'
```

</div>

Thì với dạng trên, là một điểm chí mạng trong việc truy vấn dữ liệu trong cơ sở dữ liệu. Trên thực tế chả dev nào query dữ liệu raw như thế cả ::)

Rồi, ta phân tích code một xíu. Khung nhập username ta nhập chuỗi S thì ngay lập tức chuỗi S sẽ đưa vào username (trong câu truy vấn) thành `username = "S"`. Vậy để bypass được password ta có thể biến đổi truy vấn bằng nhiều cách và đây là một cách của tôi: nhập vào khung username là `admin'--` và password là `admin`.

Từ input của tôi khi đưa vào câu truy vấn sẽ thành `username = 'admin'-- AND password ='admin'` thì lúc này dấu `--` thể hiện cho việc command lệnh, nên sẽ không thực hiện query password, và ta có thể login thành công. Đấy là những điều mình nghĩ trong đầu ::)

### Bước 2: Tìm kiếm password

Ta tiến hành hiện thực bước 1 và thao tác trên Burp Suite như hình sau

<figure markdown>
  ![Image title](/rootme/web-server/sql-injection/images/sql-injection-03.png#zoom)
  <figcaption>Hình ảnh gói tin request và response</figcaption>
</figure>

Và ta đã nhận được password là `t0_W34k!$`, submit challenge thôi

!!! success "__Flag__"
    t0_W34k!$

???+ question "Chậm lại và suy nghĩ"

    Tại sao lại nghĩ được cách bypass password bằng việc tiêm payload như trên, liệu còn cách tiêm payload nào khác? 

??? success "Giải quyết câu hỏi"

    Ta xem xét đoạn code

    <div class="result" markdown>

    ``` sql linenums="1"
    SELECT * FROM Users WHERE username ='admin' AND password ='admin'
    ```

    </div> 

    Ta có thể tiêm payload kiểu `admin'or'1'='1'--` để đoạn code trên biến thành
    <div class="result" markdown>

    ``` sql linenums="1"
    SELECT * FROM Users WHERE username ='admin'or'1'='1'-- AND password ='admin'
    ```

    </div> 

    Để nhận logic hoặc là username = admin đúng hoặc là '1'='1' đúng (hiển nhiên vế này luôn đúng). Nhưng đáng tiếc thay website này filter chuyện đó, và ta xem kết quả khi tiêm payload vào website

    <figure markdown>
        ![Image title](/rootme/web-server/sql-injection/images/sql-injection-04.png)
        <figcaption>Hình ảnh kết quả việc tiêm payload trên</figcaption>
    </figure>

    Các bạn có thể thấy, nếu làm như thế thì mặc định website sinh ra user1 với password như trong hình (có thể test với các payload khác đều sinh ra cùng 1 user1). Challenge này không đơn giản đúng không?

    Challenge tiếp theo sẽ giúp chúng ta vận dụng việc tiêm payload như vầy nhưng ở một diễn biến hoàn toàn khác.