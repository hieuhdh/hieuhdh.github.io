---
template: overrides/blog.html
icon: material/plus-circle
title: SQL injection - Insert
description: >
  
search:
  exclude: true
hide:
  - feedback

tags:
    - SQL Injection 
---

# __SQL injection - Insert__

<span>
:octicons-calendar-24: May 06, 2023

</span>

---


## __Tài nguyên và link challenge__

Tài nguyên của challenge này tại [https://www.root-me.org/en/Challenges/Web-Server/SQL-injection-Insert](https://www.root-me.org/en/Challenges/Web-Server/SQL-injection-Insert)

Link challenge này tại [:octicons-arrow-right-24: http://challenge01.root-me.org/web-serveur/ch33/][http://challenge01.root-me.org/web-serveur/ch33/]

  [http://challenge01.root-me.org/web-serveur/ch33/]: http://challenge01.root-me.org/web-serveur/ch33/

## __Tổng quan__

Trong challenge này, mục tiêu của ta là lấy flag

## __Kịch bản tấn công__
### Bước 1: Kiểm tra website

Challenge này cung cấp cho ta 2 website: 1 cho việc login tài khoản , 1 cho việc đăng kí tài khoản (trang register)

Ta thử đăng kí tài khoản với user=abc, password=abc, email=abc@gmail.com thì nhận được kết quả là đăng kí thành công.

<figure markdown>
  ![Image title](/rootme/web-server/sql-injection/images/sql-injection-insert-00.png#zoom)
  <figcaption>Hình ảnh mô tả kết quả đăng kí tài khoản</figcaption>
</figure>

Ta thử đăng kí tài khoản với user=admin, password=admin, email=admin@gmail.com thì nhận được kết quả là tài khoản đã tồn tại.

<figure markdown>
  ![Image title](/rootme/web-server/sql-injection/images/sql-injection-insert-01.png#zoom)
  <figcaption>Hình ảnh mô tả kết quả đăng kí tài khoản</figcaption>
</figure>

Tuy nhiên, sau một vài phép thử ta biết được trường email không được filter đầu vào cụ thể nếu ta nhập user=a, password=a, email=a thì vẫn được chấp nhận.

### Bước 2: Tạo cơ sở dữ liệu và testing payload

Roài, ta thử tạo 1 cơ sở dữ liệu gồm bảng member để test một vài câu lệnh insert xem tình hình như thế nào...

Tổng quan để thêm thông tin user=a, password=a, email=a vào bảng `member` gồm 3 trường (username, password, email) ta có cú pháp câu lệnh như sau:

<div class="result" markdown>

``` sql linenums="1"
insert into member (username, password, email) values ('a', 'a', 'a');
```

    </div> hoặc

<div class="result" markdown>

``` sql linenums="1"
insert into member values (username, password, email), ('a', 'a', 'a'); 
```

</div>

<figure markdown>
  ![Image title](/rootme/web-server/sql-injection/images/sql-injection-insert-02.png#zoom)
  <figcaption>Hình ảnh mô tả kết quả thêm thông tin vào bảng trong cơ sở dữ liệu</figcaption>
</figure>

Uầy uầy, nhìn quen quen nhể ::))

!!! question "Câu hỏi"
    Chuyện gì xảy ra nếu ta tiêm email = abc') ?
!!! info "Trả lời câu hỏi"
    Ngay lập tức câu lệnh `insert into member values (username, password, email), ('a', 'a', 'a');` thành 
    <div class="result" markdown>

    ``` sql linenums="1"
    insert into member values (username, password, email), ('a', 'a', 'abc')');
    ```

    </div>


Uầy uầy ::), đến đây ta có thể tiêm thêm vài câu lệnh như email = abc'), ('aa', 'aa', (select version())) để thêm `ngầm` 1 tài khoản có email chính là phiên bản sql hiện tại

<figure markdown>
  ![Image title](/rootme/web-server/sql-injection/images/sql-injection-insert-03.png#zoom)
  <figcaption>Hình ảnh mô tả kết quả thêm thông tin vào bảng trong cơ sở dữ liệu</figcaption>
</figure>

Tiếp theo, ta vào challenge và tiêm payload nhằm mục đích show table INFO bằng `username=kk, password=kk, email=a'),('something','something',(select group_concat(INFO) from information_schema.processlist limit 1));#`

<figure markdown>
  ![Image title](/rootme/web-server/sql-injection/images/sql-injection-insert-04.png#zoom)
  <figcaption>Hình ảnh mô tả kết quả thêm thông tin vào bảng trong cơ sở dữ liệu</figcaption>
</figure>
 
Ta thấy được bảng membres. Roài ta sẽ tiến hành xem username trong bảng membres bằng `zzzz'),('something1','something1',(select username from membres limit 0, 1));#` thì nhận được kết quả `request fail`. Lúc này ta chưa hiểu lý do tại sao luôn, ngồi ngẫm nghĩ mãi và thử case này trên db của mình, thì mới thấy được thông báo

> ERROR 1093 (HY000): Table 'member' is specified twice, both as a target for 'INSERT' and as a separate source for data

<figure markdown>
  ![Image title](/rootme/web-server/sql-injection/images/sql-injection-insert-05.png#zoom)
  <figcaption>Hình ảnh mô tả kết quả thêm thông tin vào bảng trong cơ sở dữ liệu</figcaption>
</figure>

Và đây chính là vấn đề, này là tính năng của SQL rùi ::). Nên ta phải làm gì đó khác thôi....

Sau một hồi lang thang trên mạng, tôi biết rằng ta cần tìm kiếm một bảng nào đó đáng nghi ngờ trong `information_schema.tables`. Và yeah, sau quá trình Brute force thì ta nhận được trong `information_schema.tables` có bảng flag. 

> Brute force theo `select table_name from information_schema.tables limit {0},1`

Tiến hành tìm flag thôi nào, dùng 1 case `username=abcd, password=abcd, email=a'),('2222','2222',(select * from flag limit 0, 1));#` ta sẽ có flag

<figure markdown>
  ![Image title](/rootme/web-server/sql-injection/images/sql-injection-insert-06.png#zoom)
  <figcaption>Hình ảnh mô tả kết quả thêm thông tin vào bảng trong cơ sở dữ liệu</figcaption>
</figure>

!!! success "__Flag__"
    moaZ63rVXUhlQ8tVS7Hw

 [^1] Xem hướng dẫn cài đặt sqlmap tại https://sqlmap.org/
