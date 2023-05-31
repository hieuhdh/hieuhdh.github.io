---
template: overrides/blog.html
icon: material/plus-circle
title: File upload - MIME type
description: >
  
search:
  exclude: true
hide:
  - feedback

tags:
    - PHP
    - Path Traversal
---

# __File upload - MIME type__

<span>
:octicons-calendar-24: May 09, 2023

</span>

---


## __Tài nguyên và link challenge__

Tài nguyên của challenge này tại [https://www.root-me.org/en/Challenges/Web-Server/File-upload-MIME-type](https://www.root-me.org/en/Challenges/Web-Server/File-upload-MIME-type)

Link challenge này tại [:octicons-arrow-right-24: http://challenge01.root-me.org/web-serveur/ch21/][http://challenge01.root-me.org/web-serveur/ch21/]

  [http://challenge01.root-me.org/web-serveur/ch21/]: http://challenge01.root-me.org/web-serveur/ch21/

## __Tổng quan__

Trong challenge này, mục tiêu của ta là hack thư viện ảnh bằng cách tải liên đoạn mã php. Lấy mật khẩu trong tệp .passwd

## __Kịch bản tấn công__

Trong có vẻ y hệt bài [File upload - Double extensions](/rootme/web-server/php/file-upload_double_extensions/) 

Ta thử làm thủ thuật như bài [File upload - Double extensions](/rootme/web-server/php/file-upload_double_extensions/)  thì well, nó không work ::)

<figure markdown>
  ![Image title](/rootme/web-server/php/images/file-upload_mine_type-01.png#zoom)
  <figcaption>Hình ảnh website</figcaption>
</figure>

Ta bật burp suite lên để xem

<figure markdown>
  ![Image title](/rootme/web-server/php/images/file-upload_mine_type-02.png#zoom)
  <figcaption>Hình ảnh website</figcaption>
</figure>

Well, phát hiện vấn đề rồi nha, challenge này nó không thực thi file ảnh (tức .jpg) nhưng nó khum hề filter cái file gửi lên, ta sẽ chuyển file a.php.jpg thành `a.php` và gửi

<figure markdown>
  ![Image title](/rootme/web-server/php/images/file-upload_mine_type-03.png#zoom)
  <figcaption>Hình ảnh website</figcaption>
</figure>

Lụm flag thui, hehe :3

<div class="result" markdown>

``` php linenums="1"
<?php
$output = shell_exec('cat ../../../.passwd');
echo "<pre>$output</pre>";
?>
```

</div>

???+ question "Chậm lại và suy nghĩ"

    Tại sao không upload file a.php từ đầu bằng website?

??? success "Giải quyết chậm lại suy nghĩ 2"

    Bạn thử làm nhé ::). Vấn đề này nó liên quan đến việc website khum hề check tên file, nó chỉ check đúng cái `Content-type

!!! success "__Flag__"
    a7n4nizpgQgnPERy89uanf6T4
