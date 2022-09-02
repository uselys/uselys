---
layout: post
title: "Mysql字段导出和导入"
date: 2022-09-02 11:09:00
---

导出：
load data local file 'tmp.sql' into table mybb_threads fields terminated by ',' lines terminated by '\n' tid,subject;

导入：
LOAD DATA LOCAL INFILE 'tmp.sql' REPLACE INTO TABLE mybb_threads FIELDS TERMINATED BY ',' LINES TERMINATED BY '\n' (tid , subject);
