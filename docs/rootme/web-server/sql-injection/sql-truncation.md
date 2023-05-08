---
template: overrides/blog.html
icon: material/plus-circle
title: SQL Truncation
description: >
  
search:
  exclude: true
hide:
  - feedback

tags:
    - SQL Injection 
---

# __SQL Truncation__

<span>
:octicons-calendar-24: May 02, 2023

</span>

---


## __Tài nguyên và link challenge__

Tài nguyên của challenge này tại [https://www.root-me.org/en/Challenges/Web-Server/SQL-Truncation](https://www.root-me.org/en/Challenges/Web-Server/SQL-Truncation)

Link challenge này tại [:octicons-arrow-right-24: http://challenge01.root-me.org/web-serveur/ch49/][http://challenge01.root-me.org/web-serveur/ch49/]

  [http://challenge01.root-me.org/web-serveur/ch49/]: http://challenge01.root-me.org/web-serveur/ch49/

## __Tổng quan__

Trong challenge này, mục tiêu của ta là cần truy xuất quyền truy cập vào khu vực admin.
Bài này nói về việc cắt bớt input đầu vào nếu input vượt quá quy định ở phía BE.

## __Kịch bản tấn công__
### Bước 1: Kiểm tra website và lấy flag ::)

Website cho ta một trang đăng kí tài khoản và 1 trang đăng nhập tài khoản. Công việc của ta là đăng kí 1 tài khoản nào đó nhưng 

<figure markdown>
  ![Image title](/rootme/web-server/sql-injection/images/sql-truncation-01.png#zoom)
  <figcaption>Hình ảnh mô tả trang đăng kí</figcaption>
</figure>

Tiếp theo, ta vào trang đăng kí xem có lượm nhặt được gì không.

<figure markdown>
  ![Image title](/rootme/web-server/sql-injection/images/sql-truncation-03.png#zoom)
  <figcaption>Hình ảnh mô tả trang đăng kí</figcaption>
</figure>

Source code trang này có vẻ là một gợi ý cho ta, ta lượm được 1 đoạn lệnh SQL về việc tạo bảng

<div class="result" markdown>

``` sql linenums="1"

CREATE TABLE IF NOT EXISTS user(   
	id INT NOT NULL AUTO_INCREMENT,
    login VARCHAR(12),
    password CHAR(32),
    PRIMARY KEY (id));

```

</div> 

Từ đoạn code trên, ta thấy được username chỉ tối đa 12 kí tự :octicons-arrow-right-24: ta sẽ tiêm payload trên 12 kí tự và để cho hệ thống nó ngắt các kí tự phía sau ví dụ `admin      123` và password là `admin123456`. Sau đó qua trang admin nhập password và lấy flag ::).

<figure markdown>
  ![Image title](/rootme/web-server/sql-injection/images/sql-truncation-02.png#zoom)
  <figcaption>Hình ảnh mô tả kết quả login account admin trên</figcaption>
</figure>

???+ question "Chậm lại và suy nghĩ 1"

    Tại sao khi tạo tài khoản với độ dài lớn hơn độ dài phía db đặt ra mà không nhận được thông báo lỗi?

??? success "Giải quyết chậm lại suy nghĩ"

    Contact me :>

???+ question "Chậm lại và suy nghĩ 2"

    Có cách nào để sửa lỗi này không và làm thế nào để biết lỗi này?

??? success "Giải quyết chậm lại suy nghĩ 2"

    Contact me :>

!!! success "__Flag__"
    J41m3Qu4nD54Tr0nc