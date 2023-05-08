---
template: overrides/blog.html
icon: material/plus-circle
title: SQL injection - String
description: >
  
search:
  exclude: true
hide:
  - feedback

tags:
    - SQL Injection 
---

# __SQL injection - String__

<span>
:octicons-calendar-24: April 28, 2023

</span>

---


## __Tài nguyên và link challenge__

Tài nguyên của challenge này tại [https://www.root-me.org/en/Challenges/Web-Server/SQL-injection-String](https://www.root-me.org/en/Challenges/Web-Server/SQL-injection-String)

Link challenge này tại [:octicons-arrow-right-24: http://challenge01.root-me.org/web-serveur/ch19/][http://challenge01.root-me.org/web-serveur/ch19/]

  [http://challenge01.root-me.org/web-serveur/ch19/]: http://challenge01.root-me.org/web-serveur/ch19/

## __Tổng quan__

Trong challenge này, mục tiêu của ta là lấy mật khẩu admin. Cụ thể là tiêm câu truy vấn phù hợp để truy xuất được thông tin về account có trong cơ sở dữ liệu.

Một vài keyword có thể hữu dụng cho việc truy vấn dữ liệu trong table như order by, group by,...

## __Kịch bản tấn công__
### Bước 1: Kiểm tra website

Với những thăm dò đầu tiên, ta nhận thấy rằng lỗi SQL có thể được khai thác ở url http://challenge01.root-me.org/web-serveur/ch19/?action=recherche.

Như thường lệ, ta thử tiêm 1 truy vấn đơn giản `' or 1=1` ngay lập tức ta nhận được kết quả như hình bên dưới

<figure markdown>
  ![Image title](/rootme/web-server/sql-injection/images/sql-injection-string-01.png#zoom)
  <figcaption>Hình ảnh mô tả kết quả tiêm payload</figcaption>
</figure>

Ta thử test với `' or1=1` để kiểm tra thông báo lỗi như nào

<figure markdown>
  ![Image title](/rootme/web-server/sql-injection/images/sql-injection-string-02.png#zoom)
  <figcaption>Hình ảnh mô tả kết quả tiêm payload</figcaption>
</figure>

Như hình trên, ta thấy thông báo cú pháp "or1" bị lỗi (vì không có dấu cách ::) ), nhưng quan trọng hơn ta thấy được database của website này dùng SQLite3.

### Bước 2: Thăm dò lỗi và tìm flag

Rồi, đây có vẻ là loại UNION. Ta tiến hành kiểm tra số cột bị lỗi. Khi ta dùng query `' order by 3--` ngay lập tức bị lỗi và ta xác định được bảng này có 2 cột

<figure markdown>
  ![Image title](/rootme/web-server/sql-injection/images/sql-injection-string-03.png#zoom)
  <figcaption>Hình ảnh mô tả kết quả tiêm payload</figcaption>
</figure>

Tiếp theo, ta kiểm tra cột hiển thị bằng `' union select 1, 2--` và ta nhận được cả 2 cột đều có thể khai thác.


<figure markdown>
  ![Image title](/rootme/web-server/sql-injection/images/sql-injection-string-04.png#zoom)
  <figcaption>Hình ảnh mô tả kết quả tiêm payload</figcaption>
</figure>

Hơn nữa, vì đây là SQLite3 nên ta sẽ dùng `' union select 1,sql from sqlite_master--` để tìm bảng

<figure markdown>
  ![Image title](/rootme/web-server/sql-injection/images/sql-injection-string-05.png#zoom)
  <figcaption>Hình ảnh mô tả kết quả tiêm payload</figcaption>
</figure>

Well, ta thấy được bảng users. Có bảng user rồi thì ta làm gì?

::) Thì ta đọc nó thôi: `' union select username,password FROM users--`

<figure markdown>
  ![Image title](/rootme/web-server/sql-injection/images/sql-injection-string-06.png#zoom)
  <figcaption>Hình ảnh mô tả kết quả tiêm payload</figcaption>
</figure>

Well, ta có được danh sách account như hình trên rồi kìa. Lụm flag thôi nào

!!! success "__Flag__"
    c4K04dtIaJsuWdi