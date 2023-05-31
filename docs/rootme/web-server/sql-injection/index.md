---
template: overrides/blog.html
icon: material/plus-circle
title: SQL injection
description: >
  
search:
  exclude: true
hide:
  - feedback

tags:
  - SQL injection
---

# __Overview__

<span>
:octicons-calendar-24: April 28, 2023 ·
</span>

---

Nơi đây tổng hợp lỗ hỏng __SQL injection__ đến từ phía Server (server side) trên rootme.

## __Định nghĩa__

SQL injection là một kỹ thuật cho phép những kẻ tấn công lợi dụng lỗ hổng của việc kiểm tra dữ liệu đầu vào trong các ứng dụng web và các thông báo lỗi của hệ quản trị cơ sở dữ liệu trả về để inject và thi hành các câu lệnh SQL bất hợp pháp.

SQL injection là kĩ thuật dựa vào lỗ hỏng của ứng dụng web (web application) chứ không phải dựa vào lỗ hỏng của web server hay hệ quản trị cơ sở dữ liệu.

Có rất nhiều loại SQL Injection (SQLi) nhưng đơn giản hóa ta sẽ có ba loại chính dưới đây:

=== ":material-check-all: Tautologies"

    1. Định nghĩa
        - Tautologies là loại SQLi dựa trên điều kiện luôn đúng.
    2. Mục đích
        - Xác định các payload cần tiêm
        - Bỏ qua xác thực
        - Trích xuất dữ liệu
    3. Mô tả 
    
        - Chúng ta xem xét ví dụ về 1 câu lệnh truy vấn thông tin username và password như sau
            <div class="result" markdown>

            ``` sql linenums="1"
            SELECT * FROM Users WHERE username ="'" + Username + "'" AND password = "'" + Password + "'"
            ```

            </div> 

        - với Username và Password là 2 biến được lấy từ khung nhập input từ người dùng.

        - Chuyện gì xảy ra nếu ta nhập giá trị của Username là `admin' OR 1=1--` và giá trị của Password là `123`?

        - Nếu ta nhập như trên, câu truy vấn sẽ thành
            <div class="result" markdown>

            ``` sql linenums="1"
            SELECT * FROM Users WHERE username ='admin' OR 1=1-- AND password = '123'
            ```

            </div>
        - Và đây, password đã không còn quan trọng trong câu truy vấn này nữa. Đúng không nào?
        - Ngoài ra còn nhiều ngữ cảnh tiêm khác nữa, bạn đọc có thể tham khảo thêm từ các challenge  

=== ":material-jquery: PiggyBacked Query"

    1. Định nghĩa
        -  PiggyBacked Query là loại tấn công dựa trên Batched SQL Statements[^1]
    2. Mục đích
        - Trích xuất dữ liệu
        - Sửa đổi tập dữ liệu
        - Thực hiện các lệnh từ xa
        - Tạo cuộc tấn công từ chối dịch vụ
    3. Mô tả 

        - Chúng ta xem xét ví dụ về 1 câu lệnh truy vấn thông tin username và password như sau
            <div class="result" markdown>

            ``` sql linenums="1"
            SELECT * FROM Users WHERE username ="'" + Username + "'" AND password = "'" + Password + "'"
            ```

            </div> 

        - với Username và Password là 2 biến được lấy từ khung nhập input từ người dùng.

        - Giả sử ta có thông tin user:
            - Username: admin
            - Password: admin
        - Giả sử trong cơ sở dữ liệu có bảng deuteri chứa dữ liệu quan trọng.

        - Chuyện gì xảy ra nếu ta nhập giá trị của Username là `admin` và giá trị của Password là `admin; DROP TABLE DEUTERI;`?

        - Nếu ta nhập như trên, câu truy vấn sẽ thành
            <div class="result" markdown>

            ``` sql linenums="1"
            SELECT * FROM Users WHERE username ='admin' AND password = 'admin'; DROP TABLE DEUTERI;
            ```

            </div>
        - Với việc tiêm thêm câu truy vấn, ta đã thành công xóa bảng DEUTERI.

=== ":material-vector-union: UNION SQL Injection"

    1. Định nghĩa
        -  PiggyBacked Query là loại tấn công dựa trên Batched SQL Statements[^1]
    2. Mục đích
        - Bỏ qua xác thực
        - Trích xuất dữ liệu
    3. Mô tả 
        - Well, đến với phần này, ta có vài bước để thực hiện truy vấn:
            - __Bước 1__: Xác định lỗ hỏng
                - Thêm dấu ' vào cuối tham số (nhập vào khung input hoặc thêm vào tham số trên URL). Nếu có lỗi → có lỗ hổng. VD: ?id=20'
            - __Bước 2__: Xác định số cột của bảng hiện tại
                - Thêm ORDER BY [số n tùy chọn] và tăng dần số cho đến khi hiện lỗi → Biết được số lượng cột trong bảng đang được truy vấn VD: ?id=20 ORDER BY 11 → có lỗi xảy ra → có 10 cột
            - __Bước 3__: Xác định cột được hiển thị trên trình duyệt
                - Dùng UNION SELECT 1,2,3,…, n và quan sát kết quả để nhận biết giá trị của cột nào sẽ được hiển thị. VD: ?id=20 UNION SELECT 1,2,3,…n
                - Giả sử các bước khai thác trước xác định cột 4 được hiển thị. Ta đến với bước 4
            - __Bước 4__: Lấy thông tin chung của CSDL
                - Thay lần lượt các số ở vị trí cột được hiển thị bằng các hàm: 
                    • `user()`: trả về user name hiện tại của kết nối DB 
                    • `version()` hay `@@version`: trả về version của DB
            - __Bước 5__: Liệt kê danh sách table trong DB từ information_schema.tables bằng câu lệnh ?id=20 UNION SELECT 1, 2, 3, table_name, 5, 6, 7, 8, 9, 10 from information_schema.tables
            - __Bước 6__: Chọn table quan tâm và khai thác bằng câu lệnh ?id=20 UNION SELECT 1, 2, 3, column_name, 5, 6, 7, 8, 9, 10 from information_schema.columns where table_name=users
    4. Tóm gọn
        - Ta có thể tóm gọn các bước khai thác ở mục 3 thành bảng sau

        <figure markdown>
        ![Image title](/rootme/web-server/sql-injection/images/00.png#zoom)
        <figcaption>Hình ảnh minh họa các bước</figcaption>
        </figure>



## __Danh sách các challenge__

Các bạn có thể tham khảo danh sách challenge trong bảng dưới đây 

{{ read_excel('docs/rootme/web-server/db.xlsx', engine='openpyxl', sheet_name="_sql_injection") }}

[^1]: Xem tại https://learn.microsoft.com/en-us/sql/odbc/reference/develop-app/batches-of-sql-statements?view=sql-server-ver16