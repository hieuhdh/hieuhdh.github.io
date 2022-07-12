---
template: overrides/blog.html
icon: material/table-edit
title: Yao's millionaire problem
description: >
  
search:
  exclude: true
hide:
  - feedback

tags:
  - Cryptography 
  - Mathematics
---

# Yao's millionaire problem

Năm 1982, Andrew Yao đã đề xuất bài toán triệu phú, trong đó thảo luận về cách hai triệu phú có thể xác định ai giàu hơn trong khi giữ bí mật tài sản thực tế của họ.

<aside class="mdx-author" markdown>
![@hieuhdh][@hieuhdh avatar]

<span>__Deuteri__ · @hieuhdh</span>
<span>
:octicons-calendar-24: May 11, 2020 ·
:octicons-clock-24: 5 min read ·

</span>
</aside>

  [@hieuhdh avatar]: https://user-images.githubusercontent.com/86739367/178121501-82770982-19ab-43e7-86a4-3f31989401df.png

---

## __Ngắn gọn vấn đề__

Bài toán Triệu phú của Yao là một bài toán tính toán đa bên an toàn được đưa ra vào năm 1982 bởi nhà khoa học máy tính và nhà lý thuyết tính toán Andrew Yao. Bài toán thảo luận về hai triệu phú, Alice và Bob, những người quan tâm đến việc biết ai trong số họ giàu hơn mà không tiết lộ tài sản thực tế của họ. 

!!! question "Vấn đề tổng quát cho bài toán"
    Cho 2 số $a$ và $b$, làm thế nào bạn có thể kết luận được $a\le b$ hay $a > b$ mà không cần biết giá trị của 2 số $a$ và $b$?

## __Các hướng giải quyết vấn đề__
### __0-encoding and 1-encoding__

Với tập $s = s_ns_{n-1}...s_0 \in \big\{0,1\big\}^n$. 

0-encoding của tập $S$ được kí hiệu là $S_s^0$ và được định nghĩa như sau: $S_s^0 = \big\{ s_ns_{n-1}...s_{i+1}1|s_i=0,1\le i \le n \big\}$ tức tách tập $S$ thành các tập con $S_i$ và tập $S_i$ được định nghĩa là cắt tập $S$ đầu vào tại vị trí trạng thái 0 đầu tiên đi từ trái qua và đổi trạng thái của nó thành 1, các trạng thái khác giữ nguyên. Ví dụ chuỗi $S=010$ ta sẽ có tập con $S_1 = 1$ và tập con $S_2 = 011$

1-encoding của tập $S$ được kí hiệu là $S_s^1$ và được định nghĩa như sau: $S_s^1 = \big\{ s_ns_{n-1}...s_i|s_i=1,1\le i \le n \big\}$ 

Giữa tập $S_s^0$ và tập $S_s^1$ có nhiều nhất $n$ phần tử.

=== "Giải quyết vấn đề"
    !!! tip "Giải quyết"
        Từ đây, vấn đề đưa ra được giải quyết như sau:

        - Nếu ta mã hóa a thành dạng 1-encoding $S_a^1$ và mã hóa b thành dạng 0-encoding $S_b^0$ chúng ta sẽ thấy rằng $a > b$ khi và chỉ khi $S_a^1$ và $S_b^0$ có phần tử chung tức $S_a^1 \cap S_b^0 \ne \emptyset$

=== "Ví dụ"
    ???+ info "Ví dụ"
        Alice có $a = 18$, Bob có $b = 12$. Giả sử ta không biết giá trị của 2 số mà Alice và Bob hiện có, ta thực hiện các phép tính sau: 

        - $S_a^1 = S_{18}^1 = \big\{1, 1001\big\}$
        - $S_b^0 = S_{12}^0 = \big\{1, 0111, 01101 \big\}$
        
        Vì $S_a^1 \cap S_b^0 = \big\{1\big\} \ne \emptyset$ nên $a>b$. Đặc biệt, vì $a$ và $b$ có vai trò như nhau nên việc tính $S_a^1$ hay $S_a^0$ đều không ảnh hưởng đến bản chất vấn đề, có điều ta phải thay đổi dấu bất đẳng thức.

### __Multiplicative homomorphism__

Phép đồng cấu (homomorphism) là một thuộc tính quan trọng và được cung cấp bởi các hệ thống mật mã, trong đó, phép toán trên bản rõ được ánh xạ thành một phép toán khác trên bản mã. 

- [x] :octicons-arrow-right-24: Tài liệu đang cập nhật
