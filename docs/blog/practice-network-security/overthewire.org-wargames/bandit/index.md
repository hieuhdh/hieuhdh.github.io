---
template: overrides/blog.html
icon: material/plus-circle
title: Bandit
description: >
  
search:
  exclude: true
hide:
  - feedback

tags:
  - Network security 
  - Linux syntax
---

# __BANDIT__

<span>
:octicons-calendar-24: October 18, 2022 ·
:octicons-clock-24: ~5 minutes

</span>

---

## __Lời mở đầu__

Ở website này, có tổng cộng 33 level tương ứng với 33 username (bandit-X) trong cùng 1 host ==bandit.labs.overthewire.org== và cùng ở port 2220. Vấn đề đặt ra là chúng ta đang thiếu ==password== để có thể connect đến 33 username, và trò chơi này mục đích chính là tìm ra password để connect đến user kế tiếp khi đang ở user hiện tại.
