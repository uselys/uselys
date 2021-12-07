---
layout: post
title: "Mariadb 删除root密码"
date: 2020-09-17 17:43:16
---

```
[root@localhost ~]# mysql -uroot
MariaDB [(none)]> use mysql;
MariaDB [mysql]> SET password=PASSWORD('');
MariaDB [mysql]> FLUSH PRIVILEGES;
```
```
[root@localhost ~]# mysqladmin -u root password ''
#如果root已经设置过密码，采用如下方法
[root@localhost ~]# mysqladmin -u root -p 'oldpassword' password ''
```
```
[root@localhost ~]# mysql -uroot
MariaDB [(none)]> use mysql;
MariaDB [mysql]> UPDATE mysql.user SET password = PASSWORD('') WHERE user = 'root';
MariaDB [mysql]> FLUSH PRIVILEGES;
```