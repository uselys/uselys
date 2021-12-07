---
layout: post
title: "YDService是什么，如何卸载"
date: 2021-07-12 16:27:40
---


卸载方法：
```
    bash /usr/local/qcloud/YunJing/uninst.sh
```

或者
直接rm 所有 qcloud 相关的文件。 然后在 ```/usr/local```下面 touch一个空白的 qcloud 文件，让它无法自动生成目录。

官方给出的卸载方法：

```
https://console.cloud.tencent.com/cwp/asset/machine
```