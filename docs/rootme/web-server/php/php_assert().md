---
template: overrides/blog.html
icon: material/plus-circle
title: PHP - assert()
description: >
  
search:
  exclude: true
hide:
  - feedback

tags:
    - PHP
    - Path Traversal
---

# __PHP - assert()__

<span>
:octicons-calendar-24: May 09, 2023

</span>

---


## __Tài nguyên và link challenge__

Tài nguyên của challenge này tại [https://www.root-me.org/en/Challenges/Web-Server/PHP-assert](https://www.root-me.org/en/Challenges/Web-Server/PHP-assert)

Link challenge này tại [:octicons-arrow-right-24: http://challenge01.root-me.org/web-serveur/ch47/][http://challenge01.root-me.org/web-serveur/ch47/]

  [http://challenge01.root-me.org/web-serveur/ch47/]: http://challenge01.root-me.org/web-serveur/ch47/

## __Tổng quan__

Trong challenge này, mục tiêu của ta tìm và khai thác lỗ hổng để đọc file passwd.

## __Kịch bản tấn công__

Như thường lệ ta inject bằng LFI, RFI các kiểu thì thấy lỗi như hình dưới

<figure markdown>
  ![Image title](/rootme/web-server/php/images/php_assert()-01.png#zoom)
  <figcaption>Hình ảnh website challenge</figcaption>
</figure>

Roài, thì đây ta thấy nó không có filter gì đầu vào và ta có thể attack như kiểu sql injection thôi, ::)

Tức ta sẽ tiêm một logic vào và thay thế cái đoạn output `Detected hacking attempt!`. Tức là ta sẽ chỉnh lại die('Detected hacking attempt!') thành die(show_source('.passwd')) :3 để nó có thể xuất ra password cho mình

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


