#1. Brub1
- Source
```js
<?php

$servername = "127.0.0.1";
$username = "admin";
$dbname = "ctf";
$password = "admin";

// Create connection
$conn = new mysqli($servername, $username, $password,$dbname);

// Check connection
if ($conn->connect_error) {
    die("Connection failed: " . $conn->connect_error);
}

if(!empty($_GET['username']) && !empty($_GET['password']))
{
    $username=mysqli_real_escape_string($conn,$_GET['username']);
    $password=mysqli_real_escape_string($conn,$_GET['password']);
    if ($username=="admin" && $_SERVER['REMOTE_ADDR']!=="127.0.0.1")
    {
        die("Admins login are allowed locally only");
    }
    else
    {
        $res=$conn->query("select * from users where username='$username' and password='$password'"); # admin admin
        if($res->num_rows > 0)
        {
            $user=$res->fetch_assoc();
            echo ($user['username']==="admin")?"0xL4ugh{test_flag}":"sorry u r not admin";
        }
        else
        {
            echo "Error : Wrong Creds";
        }

    }
}
else
{
    echo "Please Fill All Fields";
}
?>
```
- Bài này tồn tại lỗ hổng ở câu truy vấn 
```js
"select * from users where username='$username' and password='$password'"
```
- Trong cấu trúc truy vấn SQL, các ký tự trong các giá trị chuỗi được xử lý không phân biệt chữ hoa chữ thường, vì vậy 'Admin' và 'admin' được coi là giống nhau. 
![Screenshot 2023-02-23 151915](https://i.imgur.com/9FJQm7x.png)
![Screenshot 2023-02-23 152007](https://i.imgur.com/OGelcux.png)

- Có thể dùng "admin " để bybass.

# 2. Bruh 2
- Source
```js
<?php

$servername = "127.0.0.1";
$username = "ctf";
$dbname = "login";
$password = "ctf123";

// Create connection
$conn = new mysqli($servername, $username, $password,$dbname);

// Check connection
if ($conn->connect_error) {
    die("Connection failed: " . $conn->connect_error);
}

if(!empty($_GET['username']) && !empty($_GET['password']))
{
    $username=mysqli_real_escape_string($conn,$_GET['username']);
    $password=mysqli_real_escape_string($conn,$_GET['password']);
    if (preg_match("/admin/i",$username) && $_SERVER['REMOTE_ADDR']!=="127.0.0.1")
    {
        die("Admins login are allowed locally only");
    }
    else
    {
        $res=$conn->query("select * from users where username='$username' and password='$password'"); # admin admin
        if($res->num_rows > 0)
        {
            $user=$res->fetch_assoc();
            echo ($user['username']==="admin")?"0xL4ugh{My_Broo_Ag@in_fREE_pALESTINE}":"sorry u r not admin";
        }
        else
        {
            echo "Error : Wrong Creds";
        }

    }
}
else
{
    echo "Please Fill All Fields";
}
?>
```

- Bài này vì tồn tại hàm preg_match("/admin/i",$username) và hàm này có chức năng là kiểm tra tất cả chữ cái hoa và thường nên sử dụng cách khác.
- Ở đây dùng unicode bypass
https://www.compart.com/en/unicode/U+0061
![Screenshot 2023-02-23 154652](https://i.imgur.com/eWTIegu.png)
- Truy vấn sẽ là
```js
username="àdmin"
password="admin"
```