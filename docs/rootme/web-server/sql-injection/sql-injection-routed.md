---
template: overrides/blog.html
icon: material/plus-circle
title: SQL Injection - Routed
description: >
  
search:
  exclude: true
hide:
  - feedback

tags:
    - SQL Injection 
---

# __SQL Injection - Routed__

<span>
:octicons-calendar-24: May 02, 2023

</span>

---


## __Tài nguyên và link challenge__

Tài nguyên của challenge này tại [https://www.root-me.org/en/Challenges/Web-Server/SQL-Injection-Routed](https://www.root-me.org/en/Challenges/Web-Server/SQL-Injection-Routed)

Link challenge này tại [:octicons-arrow-right-24: http://challenge01.root-me.org/web-serveur/ch49/][http://challenge01.root-me.org/web-serveur/ch49/]

  [http://challenge01.root-me.org/web-serveur/ch49/]: http://challenge01.root-me.org/web-serveur/ch49/

## __Tổng quan__

Trong challenge này, mục tiêu của ta là tìm mật khẩu admin.

## __Kịch bản tấn công__
### Bước 1: Kiểm tra website

Ngó qua thử website thì ta thấy url http://challenge01.root-me.org/web-serveur/ch49/index.php?action=search là nơi ta có thể tiêm (tiem vào box). Tiếp theo ta thử tiêm `'or1=1` và xem chuyện gì xảy ra.

<figure markdown>
  ![Image title](/rootme/web-server/sql-injection/images/sql-injection-routed-01.png#zoom)
  <figcaption>Hình ảnh mô tả kết quả tiêm payload</figcaption>
</figure>

Dé, nó đã bị filter 1 :::). Nhưng khi ta tiêm `union select 1` thì không bị filter. Bài này chắc về HTTP POST METHOD. Bật Burp Suite lên thôi

### Bước 2: Nghiên cứu tiêm payload

Trên [hacktricks](https://book.hacktricks.xyz/pentesting-web/sql-injection#routed-sql-injection) cũng có bài viết về Routed SQL injection. Và nôm na là ta phải mã hóa payload sang dạng hexadecimal thì mới có thể tiêm và bypass được. Và rất may ta copy đoạn code mẫu và bypass được challenge này ::)

<figure markdown>
  ![Image title](/rootme/web-server/sql-injection/images/sql-injection-routed-02.png#zoom)
  <figcaption>Hình ảnh mô tả kết quả tiêm payload</figcaption>
</figure>


???+ question "Chậm lại và suy nghĩ"

    Tại sao chỉ dùng câu truy vấn mẫu như trên lại có thể bypass được challeng, thông tin tên bảng, tên các trường trong bảng ở đâu ra?

??? success "Giải quyết chậm lại suy nghĩ"

    Contact me :>. Bạn đọc có thể tìm cách kiểm tra hệ quản trị cơ sở dữ liệu của website này là gì. Sau đó xem xét bảng information_schema

!!! success "__Flag__"
    qs89QdAs9A