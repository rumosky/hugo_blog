---
title: "PHP学习笔记：简易学生信息管理"
slug: "9"
date: 2018-11-30T10:15:00+08:00
lastmod: 2018-11-30T10:15:00+08:00
categories: ["技术"]
tags: ["linux", "宝塔面板", "Windows", "php", "mysql"]
draft: false
---

## 概述

只是一个小demo，实现基本学生的增加，删除，修改功能。 只涉及一张数据表，相应SQL语句请阅读后面内容。

源码：[student_information_management](https://github.com/rumosky/student_information_management)

### 参考文章

使用`HTML`+`CSS`+`JavaScript`+`PHP`+`MySQL`，环境搭建请参考：[bspost cid="10"]

#### 相关配置

```bash
# MySQL 5.7.23
# PHP 7.2.13
# Nginx 1.15.6
```

```txt
├──common
│   ├──header.php     //公共头部
│   └──footer.php     //公共底部
├──css
│   └──style.css     //全局样式
├──index.php         //主页，判断是否登录
├──login.php         //登录页面展示
├──login_action.php  //登录处理
├──logout.php        //注销
├──add.php           //增加学生
├──action.php        //数据库处理
├──show.php          //查询展示数据
├──menu.php          //菜单
├──edit.php          //编辑学生信息
├──dbconfig.php      //数据库配置信息
└──favicon.ico       //网页标签图标
```

```sql
create table student(
id int(8) not null auto_increment,
name varchar(30) not null,
sex char(2) not null check (sex in ("男","女")),
age numeric(2,0) not null check (age>0 and age<100),
classid int(4) not null,
primary key(id)
)
```

### 源代码（部分）

默认登录用户名：admin，密码：abc123456

```html
<html>
    <head>
        <meta charset="UTF-8">
        <title>欢迎进入人力资源管理系统</title>
        <link rel="stylesheet" href="./css/style.css" />
    </head>
    <?php include("common/header.php");?>
    <div class="body">
      <div style="height:150px; line-height:150px;">
    <?php 
    header('Content-type:text/html; charset=utf-8');
    // 开启Session
    session_start();

    // 首先判断Cookie是否有记住了用户信息
    if (isset($_COOKIE['username'])) {
        # 若记住了用户信息,则直接传给Session
        $_SESSION['username'] = $_COOKIE['username'];
        $_SESSION['islogin'] = 1;
    }
    if (isset($_SESSION['islogin'])) {
        // 若已经登录
        echo "你好! ".$_SESSION['username'].' ,欢迎来到个人中心!<br>';
        header('refresh:3; url=show.php');
        echo "<a href='logout.php'>注销</a>";
    } else {
        // 若没有登录
        echo "对不起，您还没有登录,请<a href='login.html'>登录</a>";
    }
 ?>
        </div>
    </div>
    <?php include("common/footer.php");?>
    <body>
    </body>
</html>
```

```html
<!doctype html>
<html>
<head>
    <title>学生信息管理</title>
    <link rel="stylesheet" href="./css/style.css" />
</head>
<body>
<?php include("common/header.php");?>
  <div class="body">
    <?php include_once "menu.php"; ?>
    <h3>增加学生信息</h3>

    <form id="addstu" name="addstu" method="post" action="action.php?action=add">
        <table>
            <tr>
                <td>姓名</td>
                <td><input id="name" name="name" type="text"/></td>

            </tr>
            <tr>
                <td>性别</td>
                <td><input type="radio" name="sex" value="男"/>&nbsp;男
                    <input type="radio" name="sex" value="女"/>&nbsp;女
                </td>
            </tr>
            <tr>
                <td>年龄</td>
                <td><input type="text" name="age" id="age"/></td>
            </tr>
            <tr>
                <td>班级</td>
                <td><input id="classid" name="classid" type="text"/></td>
            </tr>
            <tr>
                <td>&nbsp;</td>
                <td><input type="submit" value="增加"/>&nbsp;&nbsp;
                    <input type="reset" value="重置"/>
                </td>
            </tr>
        </table>

    </form>
    </div>
  <?php include("common/footer.php");?>
</body>
</html>
```

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>学生信息管理</title>
    <link rel="stylesheet" href="./css/style.css" />
    <script>
        function doDel(id) {
            if (confirm("确定要删除么？")) {
                window.location = 'action.php?action=del&id='+id;
            }
        }
    </script>
  <link rel="shortcut icon " type="images/x-icon" href="./favicon.ico">
</head>
<body>
  <?php include("common/header.php");?>
<div class="body">
    <?php
    include_once "menu.php";
    ?>
    <h3>浏览学生信息</h3>
    <table width="600" border="1">
        <tr>
            <th>ID</th>
            <th>姓名</th>
            <th>性别</th>
            <th>年龄</th>
            <th>班级</th>
            <th>操作</th>
        </tr>
        <?php include("dbconfig.php");?>
      <?php
        //执行sql语句，并实现解析和遍历
        $sql = "SELECT * FROM student ";
        foreach ($pdo->query($sql) as $row) {
            echo "<tr>";
            echo "<td>{$row['id']}</td>";
            echo "<td>{$row['name']}</td>";
            echo "<td>{$row['sex']}</td>";
            echo "<td>{$row['age']}</td>";
            echo "<td>{$row['classid']}</td>";
            echo "<td>
                    <a href='javascript:doDel({$row['id']})'>删除</a>
                    <a href='edit.php?id=({$row['id']})'>修改</a>
                  </td>";
            echo "</tr>";
        }

        ?>

    </table>
</div>
<?php include("common/footer.php");?>
</body>
</html>
```

```html
<!doctype html>
<html>
<head>
<meta charset="utf-8">
<title>简易信息管理</title>
</head>

<body>
<h2>系统菜单</h2>
<a href="show.php">浏览学生</a>
<a href="add.php">增加学生</a>
<hr>
</body>
</html>
```

```html
<!doctype html>
<html>
<head>
    <meta charset="UTF-8">
    <title>学生信息管理</title>
    <link rel="stylesheet" href="./css/style.css" />
</head>
<body>
  <?php include("common/header.php");?>
  <div class="body">
    <?php
    include_once"menu.php";
    include("dbconfig.php");
    //拼接sql语句，取出信息
    $sql = "SELECT * FROM student WHERE id =".$_GET['id'];
    $stmt = $pdo->query($sql);//返回预处理对象
    if($stmt->rowCount()>0){
        $stu = $stmt->fetch(PDO::FETCH_ASSOC);//按照关联数组进行解析
    }else{
        die("没有要修改的数据！");
    }
    ?>
    <form id="addstu" name="editstu" method="post" action="action.php?action=edit">
        <input type="hidden" name="id" id="id" value="<?php echo $stu['id'];?>"/>
        <table>
            <tr>
                <td>姓名</td>
                <td><input id="name" name="name" type="text" value="<?php echo $stu['name']?>"/></td>

            </tr>
            <tr>
                <td>性别</td>
                <td><input type="radio" name="sex" value="男" <?php echo ($stu['sex']=="m")? "checked" : ""?>/>&nbsp;男
                    <input type="radio" name="sex" value="女"  <?php echo ($stu['sex']=="w")? "checked" : ""?>/>&nbsp;女
                </td>
            </tr>
            <tr>
                <td>年龄</td>
                <td><input type="text" name="age" id="age" value="<?php echo $stu['age']?>"/ placeholder="请输入1-99之间的数字"></td>
            </tr>
            <tr>
                <td>班级</td>
                <td><input placeholder="例：1701，17级1班" id="classid" name="classid" type="text" value="<?php echo $stu['classid']?>"/></td>
            </tr>
            <tr>
                <td>&nbsp;</td>
                <td><input type="submit" value="修改"/>&nbsp;&nbsp;
                    <input type="reset" value="重置"/>
                </td>
            </tr>
        </table>
    </form>
    </div>
  <?php include("common/footer.php");?>
</body>
</html>
```

```php
<?php
        //连接数据库
        //test是数据库名，username是数据库用户名，password是密码。请对应填写。
        try {
            $pdo = new PDO("mysql:host=localhost;dbname=test;","username","password");
        } catch (PDOException $e) {
            die("数据库连接失败" . $e->getMessage());
        }
        //解决中文乱码问题
        $pdo->query("SET NAMES 'UTF8'");
?>
```

```html
<div class="header">
欢迎访问本系统&nbsp;V0.1&nbsp;Beta
</div>
```

```html
<hr/>
<div class="footer">
Copyright&nbsp;©&nbsp;<?php echo date("Y");?>&nbsp;<a href="https://cdn.rumosky.com" target="_blank">rumosky</a>&nbsp;All&nbsp;Rights&nbsp;Reserved<br/>
Powered&nbsp;by&nbsp;<a href="https://rumosky.xyz" target="_blank">rumosky!</a><br/>
<a href="https://cdn.rumosky.com" target="_self">关于本站</a>&nbsp;|&nbsp;<a href="mailto:rumosky@163.com" target="_blank">联系我们</a>&nbsp;|&nbsp;<a href="#">站点地图</a>
</div>
```

```css
.header {
    padding: 20px 0 20px 15px;
    font-family:"Microsoft YaHei","微软雅黑","黑体","宋体",sans-serif;
    font-weight: 600;
    text-align: center;
    background-color: #444444;
    color: #FFFFFF;
    text-align: center;
}
.body {
    font-family:"Microsoft YaHei","微软雅黑","黑体","宋体",sans-serif;
    background-color: #FAFBFC;
    color: #444444;
    text-align: center;
}
.footer {
    padding: 20px 0 20px 15px;
    line-height:100%;
    background-color: #FFFFFF;
    color: #717171;
    text-align: center;
}
```

### 使用说明

- 搭建好PHP环境（推荐使用宝塔面板安装）
- 复制全部源码到网站目录（请按照目录说明放置，否则请修改相应引入文件路径）
- 修改dbconfig.php中的数据库配置信息
- 如想修改样式，请修改style.css文件中的内容。
- demo完整的实现了一张数据表的增删查改，其余表与之相似，请自行添加。


### 补充

如果数据库是SQL server，那么需要修改`dbconfig.php`代码

```php
<?php
        //连接数据库
        //test是数据库名，username是数据库用户名，password是密码。请对应填写。
        try {
            $pdo = new PDO("sqlsrv:server=localhost;database=test;","username","password");
        } catch (PDOException $e) {
            die("数据库连接失败" . $e->getMessage());
        }
        //解决中文乱码问题
        $pdo->query("SET NAMES 'UTF8'");
?>
```

PHP连接SQL server需要安装驱动程序，地址：[sql server驱动](https://docs.microsoft.com/zh-cn/sql/connect/php/download-drivers-php-sql-server?view=sql-server-2017 "sql server驱动")