---
layout: post
title: "VSFTP 虚拟用户设置"
date: 2021-12-13 00:54:40
---

Step 1) Create the virtual users database.

创建账号源文件 "logins.txt"，格式：
```
账号
密码
账号
密码
```
生成密码数据库文件：
```
sudo db4.8_load -T -t hash -f .ftpuser /etc/vsftpd_login.db
```
db4，根据实际情况命令不同。

更改数据库文件权限：
```
chmod 600 /etc/vsftpd_login.db
```

Step 2) Create a PAM file which uses your new database.

修改pam配置文件，大体位置在 /etc/pam.d/vsftpd
注释掉所有内容，添加一下两行
```
auth required /lib/i386-linux-gnu/security/pam_userdb.so db=/etc/vsftpd_login
account required /lib/i386-linux-gnu/security/pam_userdb.so db=/etc/vsftpd_login
```
路径根据实际情况不同。搜索 pam_userdb.so


Step 3) Set up the location of the files for the virtual users.
```
sudo useradd -d /home/vsftpd virtual
ls -ld /home/vsftpd
(which should give):
drwx------    3 virtual  virtual      4096 Jul 30 00:39 /home/vsftpd
```

Step 4) Create your vsftpd.conf config file.

See the example in this directory. Let's go through it line by line:
```
anonymous_enable=NO
local_enable=YES
```
This disables anonymous FTP for security, and enables non-anonymous FTP (which
is what virtual users use).
```
write_enable=NO
anon_upload_enable=NO
anon_mkdir_write_enable=NO
anon_other_write_enable=NO


chroot_local_user=YES

guest_enable=YES
guest_username=virtual

listen=YES
listen_port=10021


pasv_min_port=30000
pasv_max_port=30999
```

Step 5) Start up vsftpd.

Go to the directory with the vsftpd binary in it, and:
```
./vsftpd
```
If all is well, the command will sit there. If all is not well, you will
likely see some error message.


Step 6) Test.

Launch another shell session (or background vsftpd with CTRL-Z and then "bg").
Here is an example of an FTP session:
```
ftp localhost 10021
Connected to localhost (127.0.0.1).
220 ready, dude (vsFTPd 1.1.0: beat me, break me)
Name (localhost:chris): tom
331 Please specify the password.
Password:
230 Login successful. Have fun.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> pwd
257 "/"
ftp> ls
227 Entering Passive Mode (127,0,0,1,117,135)
150 Here comes the directory listing.
226 Transfer done (but failed to open directory).
ftp> size hosts
213 147
ftp>
```
Comments:
The password we gave was "foo".
Do not be alarmed by the "failed to open directory". That is because the
directory /home/vsftpd is not world readable (we could change this
behaviour if we wanted using anon_world_readable_only=NO but maybe we want
it this way for security.
We can see that we have access to the "hosts" file we copied into the virtual
FTP area, via the size command.


vsftpd.conf
```
anonymous_enable=NO
local_enable=YES
write_enable=NO
anon_upload_enable=NO
anon_mkdir_write_enable=NO
anon_other_write_enable=NO
chroot_local_user=YES
guest_enable=YES
guest_username=virtual
listen=YES
listen_port=10021
pasv_min_port=30000
pasv_max_port=30999
```

vsftpd.pam
```
auth required /lib/security/pam_userdb.so db=/etc/vsftpd_login
account required /lib/security/pam_userdb.so db=/etc/vsftpd_login
```


Step 1) Activate per-user configurability.

创建存放虚拟用户配置文件的文件夹：
```
user_config_dir=/etc/vsftpd_user_conf

sudo mkdir /etc/vsftpd_user_conf
```

Step 2) Give tom the ability to read all files / directories.

以用户名命名配置文件，
如，文件名：panswork

内容：
```
local_root=/home/vsftpd/uselys
virtual_use_local_privs=YES
write_enable=YES
local_umask=022
```