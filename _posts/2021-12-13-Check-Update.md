---
layout: post
title: "检查文件更新.sh"
date: 2021-12-13 00:54:40
---

```
    #!/bin/sh
    # Created on 4月-28-20 15:01
    # test.sh
    # @author: xs0701
	# 配合nginx日志给 蜘蛛日志有更新的网站发送邮件。
    ########################################################
    MAIL_BOX=''
    dir_path=''
    ########################################################
    send_mail(){
        name=$1
        msg="
    ---------------------------------\n                          
    $1内容已更改\n
    ---------------------------------\n
        "
        echo -e $msg | mail -s "$1已更改" $MAIL_BOX
    }
    # 初次计算
    init(){
        for name in `ls`
        do
            [ -f $name ]  && old_dic[${name}]=`md5sum $name | cut -d ' ' -f1`
        done
    }
    #计算对比
    compare(){
        for name in `ls`
        do
            if [ -f $name ];then
                md5num=`md5sum $name | cut -d ' ' -f1`
                if [ ! -n old_dic[${word}] ] || [ ${old_dic[${name}]} != $md5num ];then
                    old_dic[${name}]=$md5num
                    echo "$name已更改"
                    send_mail $name
                fi
            fi
        done
    }
    cd $dir_path
    declare -A old_dic
    init
    while :
    do
        compare
        sleep 5s
    done
```