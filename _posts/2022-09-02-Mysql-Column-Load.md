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

select tid,fid,author,authorid,subject,dateline,dateline,lastpost,lastposter,views,replies,attachment,stickreply from pre_forum_thread into outfile './tmp.sql' fields terminated by ',' lines terminated by '\n';


LOAD DATA LOCAL INFILE '/var/lib/mysql/tmp.sql' REPLACE INTO TABLE mybb_threads FIELDS TERMINATED BY ',' LINES TERMINATED BY '\n' (tid,fid,username,uid,subject,dateline,firstpost,lastpost,lastposter,views,replies,attachmentcount,sticky);


thread表：

select tid,subject,fid,author,authorid,dateline,dateline,lastpost,lastposter,views,replies,attachment,stickreply from pre_forum_thread into outfile 'tmp.sql' fields terminated by ',' lines terminated by '\n';

LOAD DATA LOCAL INFILE 'tmp.sql' REPLACE INTO TABLE mybb_threads FIELDS TERMINATED BY ',' LINES TERMINATED BY '\n' (tid,subject,fid , username,uid,dateline, firstpost,lastpost,lastposter,views,replies,attachmentcount,sticky);