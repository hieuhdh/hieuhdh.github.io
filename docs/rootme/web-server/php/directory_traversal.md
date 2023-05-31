---
template: overrides/blog.html
icon: material/plus-circle
title: Directory traversal
description: >
  
search:
  exclude: true
hide:
  - feedback

tags:
    - PHP
    - Path Traversal
---

# __Directory traversal__

<span>
:octicons-calendar-24: May 09, 2023

</span>

---


## __Tài nguyên và link challenge__

Tài nguyên của challenge này tại [https://www.root-me.org/en/Challenges/Web-Server/Directory-traversal](https://www.root-me.org/en/Challenges/Web-Server/Directory-traversal)

Link challenge này tại [:octicons-arrow-right-24: http://challenge01.root-me.org/web-serveur/ch15/ch15.php][http://challenge01.root-me.org/web-serveur/ch15/ch15.php]

  [http://challenge01.root-me.org/web-serveur/ch15/ch15.php]: http://challenge01.root-me.org/web-serveur/ch15/ch15.php

## __Tổng quan__

Trong challenge này, mục tiêu của ta là lấy được password admin.

## __Kịch bản tấn công__

Như các challenge khác, sau vào bước tiêm payload, ta lấy được ảnh password như hình

<figure markdown>
  ![Image title](/rootme/web-server/php/images/directory-traversal-01.png#zoom)
  <figcaption>Hình ảnh website challenge</figcaption>
</figure>

Roài, ta xem ảnh thôi

<figure markdown>
  ![Image title](/rootme/web-server/php/images/directory-traversal-02.png#zoom)
  <figcaption>Hình ảnh website challenge</figcaption>
</figure>

!!! success "__Flag__"
    kcb$!Bx@v4Gs9Ez


