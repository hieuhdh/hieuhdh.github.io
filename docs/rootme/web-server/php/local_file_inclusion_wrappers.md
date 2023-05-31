---
template: overrides/blog.html
icon: material/plus-circle
title: Local File Inclusion - Wrappers
description: >
  
search:
  exclude: true
hide:
  - feedback

tags:
    - PHP
    - Path Traversal
---

# __Local File Inclusion - Wrappers__

<span>
:octicons-calendar-24: May 09, 2023

</span>

---


## __Tài nguyên và link challenge__

Tài nguyên của challenge này tại [https://www.root-me.org/en/Challenges/Web-Server/Local-File-Inclusion-Wrappers](https://www.root-me.org/en/Challenges/Web-Server/Local-File-Inclusion-Wrappers)

Link challenge này tại [:octicons-arrow-right-24: http://challenge01.root-me.org/web-serveur/ch45/][http://challenge01.root-me.org/web-serveur/ch45/]

  [http://challenge01.root-me.org/web-serveur/ch45/]: http://challenge01.root-me.org/web-serveur/ch45/

## __Tổng quan__

Trong challenge này, mục tiêu của ta là lấy được flag.

## __Kịch bản tấn công__
### Bước 1: Kiểm tra website

Challenge này cung cấp cho chúng ta một website có chức năng upload ảnh. 

Hơn nữa, Website còn filter việc chỉ cho phép chúng ta upload lên mỗi file dạng .jpg

Bản chất file ảnh cũng chỉ là 1 file nén được nén thành định dạng ảnh (png, jpg,...). Ta có thể lợi dụng việc này để tạo một file thực thi code php nhưng nén lại với file có định dạng ảnh, và upload lên server để thăm dò những việc tiếp theo.

### Bước 2: Tạo code attack

Ta thử tạo một đoạn code php để lấy ra các file có trong thư mục như sau

<div class="result" markdown>

``` php linenums="1"
<pre><?php $scan = scandir('.'); foreach($scan as $file){echo $file;} ?></pre>;
```

</div>

Sau đó upload lên website (file tên aa.jpg)

<figure markdown>
  ![Image title](/rootme/web-server/php/images/local_file_inclusion_wrappers-01.png#zoom)
  <figcaption>Hình ảnh nội dung file php</figcaption>
</figure>

Ta sẽ dùng `?page=zip://tmp/upload/<tên file .jpq>%23aa` dể chạy file php. Ở đây, tên file sau khi được upload đã được modified và mang tên `AQy0SFiS4.jpg`

Roài, ta tìm được file flag....php

<figure markdown>
  ![Image title](/rootme/web-server/php/images/local_file_inclusion_wrappers-02.png#zoom)
  <figcaption>Hình ảnh nội dung file php</figcaption>
</figure>

Tiến hành đọc file thôi nào!!!

Tương tự như trên, code php đọc file flag là 

<div class="result" markdown>

``` php linenums="1"
<pre><?php show_source('flag-mipkBswUppqwXlq9ZydO.php'); ?></pre>;
```

</div>

<figure markdown>
  ![Image title](/rootme/web-server/php/images/local_file_inclusion_wrappers-03.png#zoom)
  <figcaption>Hình ảnh nội dung file php</figcaption>
</figure>

!!! success "__Flag__"
    lf1-Wr4pp3r_Ph4R_pwn3d
