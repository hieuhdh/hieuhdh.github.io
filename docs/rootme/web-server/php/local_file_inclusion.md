---
template: overrides/blog.html
icon: material/plus-circle
title: Local File Inclusion
description: >
  
search:
  exclude: true
hide:
  - feedback

tags:
    - PHP
    - Path Traversal
---

# __Local File Inclusion__[^1]

<span>
:octicons-calendar-24: May 09, 2023

</span>

---


## __Tài nguyên và link challenge__

Tài nguyên của challenge này tại [https://www.root-me.org/en/Challenges/Web-Server/Local-File-Inclusion](https://www.root-me.org/en/Challenges/Web-Server/Local-File-Inclusion)

Link challenge này tại [:octicons-arrow-right-24: http://challenge01.root-me.org/web-serveur/ch16/][http://challenge01.root-me.org/web-serveur/ch16/]

  [http://challenge01.root-me.org/web-serveur/ch16/]: http://challenge01.root-me.org/web-serveur/ch16/

## __Tổng quan__

Trong challenge này, mục tiêu của ta là lấy được password admin.

## __Kịch bản tấn công__
### Bước 1: Kiểm tra website

Đầu tiên ta xem xét website và thực hiện vài câu truy vấn, ta biết được Backend của challenge này được viết bởi php (nhờ dòng warming function file_get_contents()).

<figure markdown>
  ![Image title](/rootme/web-server/php/images/local_file_inclusion-01.png#zoom)
  <figcaption>Hình ảnh website challenge</figcaption>
</figure>

### Bước 2: Khai thác website

Thì như tiêu đề, ta thử dùng Path Traversal Attack thì phát hiện ra password

<figure markdown>
  ![Image title](/rootme/web-server/php/images/local_file_inclusion-02.png#zoom)
  <figcaption>Hình ảnh mô tả việc tiêm Path vào url</figcaption>
</figure>

!!! success "__Flag__"
    OpbNJ60xYpvAQU8


[^1]: Đọc thêm tại [Local File Inclusion](https://repository.root-me.org/Exploitation%20-%20Web/EN%20-%20Local%20File%20Inclusion.pdf?_gl=1*1t8353j*_ga*Nzc4MDU2MTY2LjE2ODI2OTQ4NDc.*_ga_SRYSKX09J7*MTY4MzU4NjE0Ni4zMi4xLjE2ODM1ODYxNDkuMC4wLjA.)