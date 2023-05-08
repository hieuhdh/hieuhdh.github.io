---
template: overrides/blog.html
icon: material/plus-circle
title: SQL injection - Numeric
description: >
  
search:
  exclude: true
hide:
  - feedback

tags:
    - SQL Injection 
---

# __SQL injection - Numeric__

<span>
:octicons-calendar-24: May 02, 2023

</span>

---


## __Tài nguyên và link challenge__

Tài nguyên của challenge này tại [https://www.root-me.org/en/Challenges/Web-Server/SQL-injection-Numeric](https://www.root-me.org/en/Challenges/Web-Server/SQL-injection-Numeric)

Link challenge này tại [:octicons-arrow-right-24: http://challenge01.root-me.org/web-serveur/ch18/][http://challenge01.root-me.org/web-serveur/ch18/]

  [http://challenge01.root-me.org/web-serveur/ch18/]: http://challenge01.root-me.org/web-serveur/ch18/

## __Tổng quan__

Trong challenge này, mục tiêu của ta là lấy mật khẩu admin.

## __Kịch bản tấn công__
### Bước 1: Kiểm tra website

Ngó qua thử website thì ta không thấy chỗ nào để có thể tiêm payload được. Có tồn tại trang login và ta thử nhập vài lệnh cơ bản, ta thấy không khả thi mấy

<figure markdown>
  ![Image title](/rootme/web-server/sql-injection/images/sql-injection-numeric-01.png#zoom)
  <figcaption>Hình ảnh mô tả kết quả tiêm payload</figcaption>
</figure>

Rồi, bài này có thể là SQLi tại HTTP GET METHOD.
Quan sát URL, ta thấy với mỗi bài thì news_id chạy từ 1 tới 3

<figure markdown>
  ![Image title](/rootme/web-server/sql-injection/images/sql-injection-numeric-02.png#zoom)
  <figcaption>Hình ảnh mô tả trang có news_id = 3</figcaption>
</figure>

### Bước 2: Thăm dò lỗi và tìm flag

Well, lại một bài UNION nữa. Ta thử check database mà website sử dụng là loại gì (có nhiều cách check và hình bên dưới là 1 trong những loại)

<figure markdown>
  ![Image title](/rootme/web-server/sql-injection/images/sql-injection-numeric-03.png#zoom)
  <figcaption>Hình ảnh mô tả tiêm payload vào url</figcaption>
</figure>

SQLite3 được dùng ở thời điểm này. Tới đây thì bài này hoàn toàn giống cách khai thác của bài  [SQL injection - String](/rootme/web-server/sql-injection/sql-injection-string)

<figure markdown>
  ![Image title](/rootme/web-server/sql-injection/images/sql-injection-numeric-04.png#zoom)
  <figcaption>Hình ảnh mô tả tiêm payload vào url</figcaption>
</figure>

<figure markdown>
  ![Image title](/rootme/web-server/sql-injection/images/sql-injection-numeric-05.png#zoom)
  <figcaption>Hình ảnh mô tả tiêm payload vào url</figcaption>
</figure>

<figure markdown>
  ![Image title](/rootme/web-server/sql-injection/images/sql-injection-numeric-06.png#zoom)
  <figcaption>Hình ảnh mô tả tiêm payload vào url</figcaption>
</figure>

<figure markdown>
  ![Image title](/rootme/web-server/sql-injection/images/sql-injection-numeric-07.png#zoom)
  <figcaption>Hình ảnh mô tả tiêm payload vào url</figcaption>
</figure>

Well, sau step by step tiêm payload, ta được thông tin password của admin là aTlkJYLjcbLmue3

!!! success "__Flag__"
    aTlkJYLjcbLmue3