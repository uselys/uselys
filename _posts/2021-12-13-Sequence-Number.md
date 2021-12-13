---
layout: post
title: "生成数字序列.sh"
date: 2021-12-13 00:54:40
---

```
#!/bin/sh
num=1009924
while [ $num -le 2351242 ]
do
rm bbs.bozhong.com/thread-$num-1-1.html
num=`expr $num + 1`
done

```