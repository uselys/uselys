---
layout: post
title: "用Apache日志记录百度蜘蛛访问行为"
date: 2020-04-18 22:53:17
---

前面有一篇写nginx的，然后有MJJ说自己不用nginx。那么好，再写一个apache的。

先说一下Apache日志的基本规则：

日志规则是由LogFormat命令定义的，就在 apache[2].conf或httpd.conf 下面，不同版本对 .conf的命名不同。
```
    LogFormat "%v:%p %h %l %u %t "%r" %>s %O "%{Referer}i" "%{User-Agent}i"" vhost_combined
    LogFormat "%h %l %u %t "%r" %>s %O "%{Referer}i" "%{User-Agent}i"" combined
    LogFormat "%h %l %u %t "%r" %>s %O" common
    LogFormat "%{Referer}i -> %U" referer
    LogFormat "%{User-agent}i" agent
```

里面的一大堆%参数定义了记录的内容，比如时间、IP、主机名称等。我们重点关注的是 User-Agent 这个参数，因为只蜘蛛的特征以User-Agent表示的。

后面的 vhost_combined 、 combined 、common 等是对这条规则的定义。可以自己增删里面的内容，并加个新的名称。

还有一个是日志的级别：
```
    #
    # LogLevel: Control the severity of messages logged to the error_log.
    # Available values: trace8, ..., trace1, debug, info, notice, warn,
    # error, crit, alert, emerg.
    # It is also possible to configure the log level for particular modules, e.g.
    # "LogLevel info ssl:warn"
    #
    LogLevel warn
```

每个级别定义不同的事件等级，详细可以查看手册，或者自己百度吧。

为简化起见，直接用 warn 好了。

下面设置记录虚拟主机的参数：
```
    <VirtualHost *:80>
        ServerName www.uselys.cn
        DocumentRoot /var/www/uselys.cn
        LogLevel warn
        SetEnvIf User-Agent "BaiduSpider" uselys.cn
        CustomLog /var/www/uselys.cn/access.log combined env=uselys.cn
    </VirtualHost>
```

前面两条MJJ们大概经常改来改去，熟门熟路了。

重点是后面两条：
```
    SetEnvIf User-Agent "BaiduSpider" uselys.cn
```

SetEnvIf 是设置Apache的环境变量，这里所记录的是 User-Agent，后面的 uselys.cn 是变量名称，自己修改。

```
    CustomLog /var/www/uselys.cn/access.log combined env=uselys.cn
```

CustomLog 定义的是路径和条件。路径自己定义，combined就是前面提到的的日志规则名称， env就是环境条件。

一句话概括：

当 env 的条件是 uselys.cn 规则中包含 BaiduSpider 这个词的时候，就把combined格式的日志记录在 /var/www/uselys.cn/access.log 里面。

然后再注意一下 BaiduSpider 的大小写。
