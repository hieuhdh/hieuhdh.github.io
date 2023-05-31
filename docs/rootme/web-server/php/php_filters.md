---
template: overrides/blog.html
icon: material/plus-circle
title: PHP - Filters
description: >
  
search:
  exclude: true
hide:
  - feedback

tags:
    - PHP
    - Path Traversal
---

# __PHP - Filters__

<span>
:octicons-calendar-24: May 09, 2023

</span>

---


## __Tài nguyên và link challenge__

Tài nguyên của challenge này tại [https://www.root-me.org/en/Challenges/Web-Server/PHP-Filters](https://www.root-me.org/en/Challenges/Web-Server/PHP-Filters)

Link challenge này tại [:octicons-arrow-right-24: http://challenge01.root-me.org/web-serveur/ch12/][http://challenge01.root-me.org/web-serveur/ch12/]

  [http://challenge01.root-me.org/web-serveur/ch12/]: http://challenge01.root-me.org/web-serveur/ch12/

## __Tổng quan__

Trong challenge này, mục tiêu của ta là lấy được password admin.

## __Kịch bản tấn công__
### Bước 1: Kiểm tra và khai thác website

Điều tiên, website này cung cấp cho ta 2 trang con (1 trang login và 1 trang home). Ta sẽ thử tiêm payload vào url để xem chuyện gì xảy ra

<figure markdown>
  ![Image title](/rootme/web-server/php/images/php_filters-01.png#zoom)
  <figcaption>Hình ảnh website challenge</figcaption>
</figure>

Ta thấy rằng, website xuất hiện thông báo lỗi rằng không chấp nhận đường dẫn đến /etc/passwd. Ta sẽ thử dùng [wrapper](/rootme/web-server/php/local_file_inclusion_double_encoding/) để get sourcecode trong challenge

<figure markdown>
  ![Image title](/rootme/web-server/php/images/php_filters-02.png#zoom)
  <figcaption>Hình ảnh website challenge</figcaption>
</figure>

Ta được, challenge include cái `<?php include("ch12.php");?>` vào website. Tiếp theo, ta tiến hành xem sourcecode 

<figure markdown>
  ![Image title](/rootme/web-server/php/images/php_filters-03.png#zoom)
  <figcaption>Hình ảnh website challenge</figcaption>
</figure>

Ta được sourcecode như sau.

<div class="result" markdown>

``` php linenums="1"
<?php

$inc="accueil.php";
if (isset($_GET["inc"])) {
    $inc=$_GET['inc'];
    if (file_exists($inc)){
	$f=basename(realpath($inc));
	if ($f == "index.php" || $f == "ch12.php"){
	    $inc="accueil.php";
	}
    }
}

include("config.php");


echo '
  <html>
  <body>
    <h1>FileManager v 0.01</h1>
    <ul>
	<li><a href="?inc=accueil.php">home</a
```

</div>

Well, có sourcecode rồi, ta thấy rằng logic xử lí trong sourcecode cũng khá dễ hiểu, ta sẽ tiến hành xem code file config.php

<figure markdown>
  ![Image title](/rootme/web-server/php/images/php_filters-04.png#zoom)
  <figcaption>Hình ảnh website challenge</figcaption>
</figure>

Sau khi decode, ta được 

<div class="result" markdown>

``` php linenums="1"
<?php
$username="admin";
$password="DAPt9D2mky0APAF";
```

</div>


Yeahhh, flag cần tìm là `DAPt9D2mky0APAF`

!!! success "__Flag__"
    DAPt9D2mky0APAF


