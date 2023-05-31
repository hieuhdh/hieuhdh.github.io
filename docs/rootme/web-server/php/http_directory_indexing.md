---
template: overrides/blog.html
icon: material/plus-circle
title: HTTP - Directory indexing
description: >
  
search:
  exclude: true
hide:
  - feedback

tags:
    - PHP
    - Path Traversal
---

# __HTTP - Directory indexing__

<span>
:octicons-calendar-24: May 13, 2023

</span>

---


## __Tài nguyên và link challenge__

Tài nguyên của challenge này tại [https://www.root-me.org/en/Challenges/Web-Server/HTTP-Directory-indexing](https://www.root-me.org/en/Challenges/Web-Server/HTTP-Directory-indexing)

Link challenge này tại [:octicons-arrow-right-24: view-source:http://challenge01.root-me.org/web-serveur/ch4/][view-source:http://challenge01.root-me.org/web-serveur/ch4/]

  [view-source:http://challenge01.root-me.org/web-serveur/ch4/]: view-source:http://challenge01.root-me.org/web-serveur/ch4/

## __Tổng quan__

Ctrl + U ::)

## __Kịch bản tấn công__

Theo gợi ý, ta ctrl + U để xem sourceweb

<figure markdown>
  ![Image title](/rootme/web-server/php/images/http_directory_indexing-01.png#zoom)
  <figcaption>Hình ảnh source code web</figcaption>
</figure>

Ta thấy được đoạn comment `<!-- include("admin/pass.html") -->`. Tiến hành vào link thì bị rick roll

<figure markdown>
  ![Image title](/rootme/web-server/php/images/http_directory_indexing-02.png#zoom)
  <figcaption>Hình ảnh website</figcaption>
</figure>

Sau một vài thao tác tìm kiếm trong admin thì ta nhận được password

<figure markdown>
  ![Image title](/rootme/web-server/php/images/http_directory_indexing-03.png#zoom)
  <figcaption>Hình ảnh website</figcaption>
</figure>

<figure markdown>
  ![Image title](/rootme/web-server/php/images/http_directory_indexing-04.png#zoom)
  <figcaption>Hình ảnh website</figcaption>
</figure>

!!! success "__Flag__"
    LINUX