---
template: overrides/blog.html
icon: material/plus-circle
title: PHP - register globals
description: >
  
search:
  exclude: true
hide:
  - feedback

tags:
    - PHP
    - Path Traversal
---

# __PHP - register globals__

<span>
:octicons-calendar-24: May 09, 2023

</span>

---


## __Tài nguyên và link challenge__

Tài nguyên của challenge này tại [https://www.root-me.org/en/Challenges/Web-Server/PHP-register-globals](https://www.root-me.org/en/Challenges/Web-Server/PHP-register-globals)

Link challenge này tại [:octicons-arrow-right-24: http://challenge01.root-me.org/web-serveur/ch17/][http://challenge01.root-me.org/web-serveur/ch17/]

  [http://challenge01.root-me.org/web-serveur/ch17/]: http://challenge01.root-me.org/web-serveur/ch17/

## __Tổng quan__

Trong challenge này, mục tiêu của ta là tìm được thông tin các tệp sao lưu. Đến với website challenge, ta đoán là cần tìm password.

## __Kịch bản tấn công__

Theo luồng challene, ta thử download file index.php backup thử, thì nó có thể download được và ta được sourcecode như sau

<div class="result" markdown>

``` php linenums="1"
<?php


function auth($password, $hidden_password){
    $res=0;
    if (isset($password) && $password!=""){
        if ( $password == $hidden_password ){
            $res=1;
        }
    }
    $_SESSION["logged"]=$res;
    return $res;
}

                                                                                

function display($res){
    $aff= '
	  <html>
	  <head>
	  </head>
	  <body>
	    <h1>Authentication v 0.05</h1>
	    <form action="" method="POST">
	      Password&nbsp;<br/>
	      <input type="password" name="password" /><br/><br/>
	      <br/><br/>
	      <input type="submit" value="connect" /><br/><br/>
	    </form>
	    <h3>'.htmlentities($res).'</h3>
	  </body>
	  </html>';
    return $aff;
}



session_start();
if ( ! isset($_SESSION["logged"]) )
    $_SESSION["logged"]=0;

$aff="";
include("config.inc.php");

if (isset($_POST["password"]))
    $password = $_POST["password"];

if (!ini_get('register_globals')) {
    $superglobals = array($_SERVER, $_ENV,$_FILES, $_COOKIE, $_POST, $_GET);
    if (isset($_SESSION)) {
        array_unshift($superglobals, $_SESSION);
    }
    foreach ($superglobals as $superglobal) {
        extract($superglobal, 0 );
    }
}

if (( isset ($password) && $password!="" && auth($password,$hidden_password)==1) || (is_array($_SESSION) && $_SESSION["logged"]==1 ) ){
    $aff=display("well done, you can validate with the password : $hidden_password");
} else {
    $aff=display("try again");
}

echo $aff;

?>
```

</div>

Từ sourcecode trên, ta thấy nếu $_SESSION["logged"]==1[^1] thì sẽ lấy được password. Ta tiến hành truy xuất password bằng `?_SESSION[logged]=1`

<figure markdown>
  ![Image title](/rootme/web-server/php/images/php_register_globals-01.png#zoom)
  <figcaption>Hình ảnh website challenge</figcaption>
</figure>

Yeahhh, flag cần tìm là `NoTQYipcRKkgrqG`

!!! success "__Flag__"
    NoTQYipcRKkgrqG


 [^1]: Xem thêm về biến $_SESSION tại https://www.php.net/manual/en/reserved.variables.session.php 


