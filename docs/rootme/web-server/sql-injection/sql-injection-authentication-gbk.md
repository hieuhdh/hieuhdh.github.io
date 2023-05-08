---
template: overrides/blog.html
icon: material/plus-circle
title: SQL injection - Authentication GBK
description: >
  
search:
  exclude: true
hide:
  - feedback

tags:
    - SQL Injection 
---

# __SQL injection - Authentication GBK__

<span>
:octicons-calendar-24: April 28, 2023

</span>

---


## __Tài nguyên và link challenge__

Tài nguyên của challenge này tại [https://www.root-me.org/en/Challenges/Web-Server/SQL-injection-authentication-GBK](https://www.root-me.org/en/Challenges/Web-Server/SQL-injection-authentication-GBK)

Link challenge này tại [:octicons-arrow-right-24: http://challenge01.root-me.org/web-serveur/ch42/][http://challenge01.root-me.org/web-serveur/ch42/]

  [http://challenge01.root-me.org/web-serveur/ch42/]: http://challenge01.root-me.org/web-serveur/ch42/

## __Tổng quan__

Trong challenge này, mục tiêu của ta là lấy được quyền admin (tất nhiên không dùng account admin).

Vấn đề gian nan của challenge này là injection theo GBK (GBK Character Encoding).

Với challenge [SQL Injection Authentication](/rootme/web-server/sql-injection/sql-injection-authentication) ta đã tìm được cách bypass password đơn giản. Nhưng điều đó không khả thi với GBK.

Thông thường người ta dùng hàm `addslashes()`[^1] để bảo vệ SQL Injection.

Hàm này hoạt động như kiểu filter đầu vào như hình bên dưới

<figure markdown>
  ![Image title](/rootme/web-server/sql-injection/images/sql-injection-gbk-01.png#zoom)
  <figcaption>Hình ảnh mô tả các filter có trong hàm addslashes</figcaption>
</figure>

Nghĩa là nếu đầu vào ta nhập `admin'` thì sẽ biến đổi thành `admin\'`. Vậy nên ta có thể dùng GBK mã hóa các kí tự để có thể bypass được password.

Well, phân tích xong rồi, chúng ta đến với kịch bản tấn công nào.

## __Kịch bản tấn công__
### Bước 1: Kiểm tra website

Đầu tiên ta xem xét website và bật Burp Suite quan sát gói tin

<figure markdown>
  ![Image title](/rootme/web-server/sql-injection/images/sql-injection-gbk-02.png#zoom)
  <figcaption>Hình ảnh challenge</figcaption>
</figure>

Quay trở lại đoạn code giả tưởng của ta ở challenge [SQL Injection Authentication](/rootme/web-server/sql-injection/sql-injection-authentication)

<div class="result" markdown>

``` sql linenums="1"
SELECT * FROM Users WHERE username ='admin' AND password ='admin'
```

</div>

Mục tiêu hiện tại của ta là tiêm payload `admin'OR 1=1--` vào username để có thể bypass password và đoạn code trên sẽ thành 

<div class="result" markdown>

``` sql linenums="1"
SELECT * FROM Users WHERE username ='admin' OR 1 = 1-- AND password ='admin'
```

</div>

Ngay lập tức ta bị filter và bị lỗi nhận dạng như hình bên dưới

<figure markdown>
  ![Image title](/rootme/web-server/sql-injection/images/sql-injection-gbk-03.png#zoom)
  <figcaption>Hình ảnh mô tả gói tin</figcaption>
</figure>

Ta phân tích gói tin, rõ ràng ta truyền vào `admin'OR 1=1--` nhưng khi bật Burp Suite ta thấy thông tin này được mã hóa (HTML URL Encoding) thành `admin%27OR+1%3D1--` và vẫn nhận được lỗi không nhận dạng được username, tức bị filter rùi ::). Ta sẽ tìm cách mã hóa input này để bypass filter.

???+ question "Chậm lại và suy nghĩ 1"

    Làm thế nào để biết phía BE họ dùng hàm `addslashes()` để filter đầu vào trong việc truy vấn vào hệ quản trị cơ sở dữ liệu?

??? success "Giải quyết chậm lại suy nghĩ 1"

    Để trả lời câu hỏi này, ta có thể test thử truyền một mảng username như hình bên dưới

    <figure markdown>
        ![Image title](/rootme/web-server/sql-injection/images/sql-injection-04.png)
        <figcaption>Hình ảnh kết quả việc tiêm payload trên</figcaption>
    </figure>

    Hoặc còn nhiều cách khác, bạn đọc tìm hiểu thử xem!

### Bước 2: Tạo payload

Như đã phân tích ở trên, ta nhìn lại 1 vài HTML URL Encoding bên dưới

| Character | From Windows-1252   | From UTF-8 |
| --------- | ------------------- | ---------- |
| '         |       %27           |   %27      |
| =         |      	%3D           |   %3D      |
| \         |      	%5C           |   %5C      |

Rồi, payload của ta là `admin%27OR+1%3D1--` thì %27 là ngay dấu `'` thì ngay lập tức sẽ bị filter và biến đổi thành `%5C%27` và dẫn đến việc khiến chúng ta không thực hiện được attack. Nhiệm vụ của ta là phải tìm payload để khi chèn vào và khi payload tự thêm `%5C` thì sẽ được kết hợp với thứ mình chèn, làm tách chuỗi `%5C%27` thành `...%5C` và `%27`.

Well, áp dụng GBK mà triển thôi, chọn đại 2 bytes đầu là %DF thì sẽ kết hợp với %5c thành %DF%5C và là 1 kí tự Trung Quốc.

Và payload hoàn chỉnh của ta có thể là `1%df%27OR+1%3D1--%20`

<figure markdown>
  ![Image title](/rootme/web-server/sql-injection/images/sql-injection-gbk-04.png#zoom)
  <figcaption>Hình ảnh mô tả việc tiêm thành công payload</figcaption>
</figure>

Và khi tiêm thành công payload, ta sẽ thấy được `Follow redirection` và ta click vào chuyển hướng, ta được flag.

<figure markdown>
  ![Image title](/rootme/web-server/sql-injection/images/sql-injection-gbk-05.png#zoom)
  <figcaption>Hình ảnh mô tả flag</figcaption>
</figure>

!!! success "__Flag__"
    iMDaFlag1337!

???+ question "Chậm lại và suy nghĩ 2"

    Tại sao payload ở phía trên phía sau cùng là %20, và tại sao số đầu là 1 chứ không phải `admin` như input mẫu ban đầu?

??? success "Giải quyết chậm lại suy nghĩ 2"

    Contact me :>

Qua bài SQL Injection Authentication trên, ta để ý rằng trong payload ta tiêm tồn tại điều kiện "OR" và bản chất ở đây là tiêm payload với hoặc username đúng hoặc mệnh đề sau đúng (hiển nhiên mệnh đề sau luôn đúng, vì ta tiêm 1=1 mà :>). Tóm lại đây là loại tấn công Tautologies.

[^1]: Xem document của hàm tại: https://www.php.net/manual/en/function.addslashes.php