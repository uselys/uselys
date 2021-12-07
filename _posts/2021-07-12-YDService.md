---
layout: post
title: "YDService是什么，如何卸载"
date: 2021-07-12 16:27:40
---

几乎所有国内大厂的服务器都会内建自家的服务，不管这个东西实际是用来做什么的，看着都很多余。

腾讯云默认的安全软件，对于新手而言，可能有些许用处。但它既吃内存，又吃CPU。

如果自己熟悉LInux，肯定并不需要它，安全策略很多种，这种内建软件的办法就好像装了后门一样让人不放心。

这玩意不论是重新安装还是自动升级，都会篡改 resolve.conf, 并使 letsencrypt 失效。


卸载方法：
```
    bash /usr/local/qcloud/YunJing/uninst.sh
```

复制代码

其实我自己也没这样删除过。 而是直接rm 所有 qcloud 相关的文件。 然后在 ```/usr/local```下面 touch一个空白的 qcloud 文件，让它无法自动生成目录。

官方给出的卸载方法：

```
https://console.cloud.tencent.com/cwp/asset/machine
```