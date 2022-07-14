---
template: overrides/project.html
description: >
  Đây là đồ án môn học Data structure and Algorithms mà tôi đã học tại trường.
search:
  exclude: true
hide:
  - feedback
---

# Robotic vacuum cleaner

<span>
:octicons-calendar-24: June 23, 2021 ·
:octicons-clock-24: ~5 minutes

</span>

__Đây là đồ án mà tôi đã làm để kết thúc môn Cấu trúc dữ liệu và giải thuật.__

---
### **Đề bài**
1. Hãy thiết kế robot hút bụi với các tính năng sau:
    * Tên thương hiệu
    * Mã số sản phẩm
    * Hướng di chuyển (hiện hành): Bắc, Nam, Đông, Tây 
    * Tiến lên n bước
    * Lùi lại n bước
    * Xoay trái 90 độ
    * Xoay phải 90 độ
    * Xoay 180 độ
    * Tự động di chuyển về phía trước, nếu có chướng ngại vật thì rẽ sang bên phải, nếu bên phải có chướng ngại vật thì rẽ sang bên trái, nếu cả hai phía phải và trái đều có chướng ngại vật thì lùi lại một bước và tìm hướng có thể di chuyển theo nguyên tắc trên. Không ràng buộc lộ trình di chuyển của robot hút bụi.
    * Hút bụi trên lộ trình di chuyển.
 
2. Robot hút bụi có chế độ tự kiểm tra nguồn điện còn bao nhiêu %. Robot sẽ di chuyển cho đến khi gần hết nguồn điện thì phát tín hiệu thông báo (khi nguồn điện còn 15%, 10%, 5%).
3. Sau mỗi bước, in ra sơ đồ phòng và vị trí hiện hành của robot (không bắt buộc thiết kế giao diên đồ họa).  ↑ : Robot (hướng Bắc)

<div align="left">
    <a href="https://github.com/hieuhdh/DOAN_CTDLGT" class="btn">Source code</a> 
</div>

## **Đặc trưng trong project**

Trong chương trình này, Robot sẽ được đặc tả bộ não - gồm 4 chế độ di chuyển: Normal, Medium, Hard, User

* **Chế độ normal:** Robot sẽ tránh các va chạm và sẽ tự động di chuyển tùy ý.
* **Chế độ medium:** Robot sẽ được khởi động thông minh hơn, biết chọn lọc những điểm đã đi qua và tránh đi lại để giảm thiểu thời gian cũng như nguồn điện cấp phát.
* **Chế độ hard:** Robot được trang bị 1 con AI cơ bản, nhằm giúp tối ưu việc di chuyển cũng như tránh lặp lại các bước đi. Tuy nhiên, do kiến thức hạn hẹp nên AI chỉ được build ở chế độ cơ bản trên nền BFS. 
* **Chế độ USER:** 
    * Ở chế độ này, người dùng sẽ tự do điều khiển robot nhưng robot cũng được trang bị 1 con AI nhằm quản lý cách di chuyển của người dùng, hòng tránh việc người dùng lỡ may di chuyển khiến robot va vào tuờng, robot sẽ tự động điều hướng.
    * Tức là mặc dù người dùng điều khiển robot, nhưng robot sẽ tránh né và tự điều hướng nếu như người dùng cố tình điều khiển robot vào vật cản gây ra va chạm.
    * Chế độ này nên tự trải nghiệm chạy code, thì mới biết cách thức Robot hoạt động thế nào.

Hiển nhiên, đối với bộ não của Robot như trên, hoàn toàn có thể đáp ứng đủ và hơn những thứ đề bài mong đợi.

Chương trình được hiện thực toàn bộ bằng console và độ dài cũng như độ rộng khung hình lấy từ dữ liệu động ứng với các màn hình thiết bị, nên không bị lỗi sai tỉ lệ khung hình hay bất kì lỗi vặt nào linh tinh.

## **Demo 4 chế độ**

1. Chế độ Normal

<figure class="mdx-video" markdown>
  <div class="mdx-video__inner">
    <iframe src="https://user-images.githubusercontent.com/86739367/147782704-c68f9abf-c8ad-4c0b-8136-9779615b2b73.mp4" allowfullscreen></iframe>
  </div>
  <figcaption markdown>

Robot di chuyển ngẫu nhiên các vị trí.

  </figcaption>
</figure>

2. Chế độ Medium

<figure class="mdx-video" markdown>
  <div class="mdx-video__inner">
    <iframe src="https://user-images.githubusercontent.com/86739367/147786595-676677c7-7407-461b-a10e-1f5d37890b9d.mp4" allowfullscreen></iframe>
  </div>
  <figcaption markdown>

Robot di chuyển ngẫu nhiên các vị trí nhưng thông minh hơn.

  </figcaption>
</figure>

3. Chế độ Hard

<figure class="mdx-video" markdown>
  <div class="mdx-video__inner">
    <iframe src="https://user-images.githubusercontent.com/86739367/147786549-4730f590-98d2-46e7-a245-22b44b9850d0.mp4" allowfullscreen></iframe>
  </div>
  <figcaption markdown>

Robot được đặc tả bộ não AI để di chuyển hút bụi.

  </figcaption>
</figure>

4. Chế độ User

<figure class="mdx-video" markdown>
  <div class="mdx-video__inner">
    <iframe src="https://user-images.githubusercontent.com/86739367/147785933-94f28dfb-7b1a-4f05-ab0b-b2160ef477bc.mp4" allowfullscreen></iframe>
  </div>
  <figcaption markdown>

Người có thể tự do điều khiển robot để hút bụi.

  </figcaption>
</figure>

## **Cài đặt**

### Git clone

>git clone https://github.com/hieuhdh/DOAN_CTDLGT.git
    
### Chạy chương trình

1. Dùng VS2019 để chạy file `robothutbui.sln`trong thư mục CODE.

## **Báo cáo sự cố**

Bạn có thể báo cáo sự cố bằng việc <a href="https://github.com/hieuhdh/DOAN_CTDLGT/issues" class = "link_for_hover"  >mở một issues</a> trên github, hoặc bình luận dưới bài viết này.

<table>
  <thead>
    <tr>
<td style = "font-weight: bold">Sau những gì mà tôi đã chia sẻ ở trên mong rằng sẽ giúp ích được phần nào đó cho bạn đọc. Mọi thắc mắc hoặc góp ý bạn đọc có thể liên hệ <a class = "link_for_hover" href="https://hieuhdh.github.io/deuteri/">tại đây</a>.</td>
    </tr>
  </thead>
</table>