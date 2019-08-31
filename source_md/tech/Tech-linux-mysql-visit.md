---
title: 'Mysql远程访问权限-授权'
date: 2019-08-31 22:37:27
categories:
- 编程吧
- SQL
tags:
- SQL
---




MySQL数据库刚刚建立的时候，数据库进行远程访问时可能会受限.
![](https://raw.githubusercontent.com/liruixue/muqiaosite/master/images/tech/Tech-linux-mysql-visit-home.jpg)
<center><font color=#c3c3c3>深圳的一家日式料理居酒屋墙壁的涂鸦漫画</font></center>
<!-- more -->
>本节主要告诉你如何对mysql进行远程访问授权


# 通过xshell等终端工具登录进服务器
```bash
[root@localhost ~]# mysql -uroot -p -P3306
```
输入密码后，即可进入mysql终端模式：
```bash
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 29
Server version: 5.5.62-log Source distribution

Copyright (c) 2000, 2018, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
+--------------------+
```
# 给root用户授予远程访问权限
```bash
mysql> use mysql;
Database changed
mysql> grant all privileges  on *.* to root@'%' identified by "password";
Query OK, 0 rows affected (0.00 sec)
mysql> flush privileges;
Query OK, 0 rows affected (0.00 sec)
```

# 当然也可以为已有用户授予远程访问的权限
```bash
mysql> grant all privileges  on testdb.* to testuser@'%' identified by "passwordtest";
Query OK, 0 rows affected (0.00 sec)

mysql> flush privileges;
Query OK, 0 rows affected (0.00 sec)

```
</br>
作者 [@碧海饮冰人]    
2019 年 8月 31日    
  



