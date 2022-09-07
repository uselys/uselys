---
layout: post
title: "Mysql字段导出和导入"
date: 2022-09-02 11:09:00
---

导出：
select tid,subject from pre_forum_thread into outfile 'tmp.sql' fields terminated by ',' lines terminated by '\n';

导入：
LOAD DATA LOCAL INFILE 'tmp.sql' REPLACE INTO TABLE mybb_threads FIELDS TERMINATED BY ',' LINES TERMINATED BY '\n' (tid , subject);


discuz 转 mybb:

post表：
```
select pid,fid,tid,author,authorid,subject,dateline,message,useip,smileyoff,replycredit from pre_forum_post into outfile './post.sql' fields terminated by ',' lines terminated by '\n';


LOAD DATA LOCAL INFILE '/var/lib/mysql/post.sql' REPLACE INTO TABLE mybb_posts FIELDS TERMINATED BY ',' LINES TERMINATED BY '\n' (pid,fid,tid,username,uid,subject,dateline,message,ipaddress,smilieoff,replyto);
```

thread表：
```
select tid,subject,fid,author,authorid,dateline,dateline,lastpost,lastposter,views,replies,attachment,stickreply from pre_forum_thread into outfile './thread.sql' fields terminated by ',' lines terminated by '\n';

LOAD DATA LOCAL INFILE '/var/lib/mysql/thread.sql' REPLACE INTO TABLE mybb_threads FIELDS TERMINATED BY ',' LINES TERMINATED BY '\n' (tid,subject,fid,username,uid,dateline,firstpost,lastpost,lastposter,views,replies,attachmentcount,sticky);
```

forum 表：
```
select fid,fid,fup,type,name,status,threads,posts,allowsmilies,allowhtml,allowbbcode,allowimgcode,allowmediacode,status,status,status,status,status,status,status,status,status,status,status from pre_forum_forum into outfile './forum.sql' fields terminated by ',' lines terminated by '\n';

LOAD DATA LOCAL INFILE '/var/lib/mysql/forum.sql' REPLACE INTO TABLE mybb_forums FIELDS TERMINATED BY ',' LINES TERMINATED BY '\n' (fid,parentlist,pid,type,name,open,threads,posts,allowsmilies,allowhtml,allowmycode,allowimgcode,allowvideocode,disporder,active,allowmycode,allowsmilies,allowimgcode,allowvideocode,allowpicons,allowtratings,usepostcounts,usethreadcounts,showinjump);
```
user 表：
```
select uid,email,username,password,regdate from pre_common_member into outfile './users.sql' fields terminated by ',' lines terminated by '\n';

LOAD DATA LOCAL INFILE '/var/lib/mysql/users.sql' REPLACE INTO TABLE mybb_users FIELDS TERMINATED BY ',' LINES TERMINATED BY '\n' (uid,email,username,password,regdate);
```