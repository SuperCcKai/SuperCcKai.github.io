---
layout: post
title: 遇到的一些问题
date: 2017-10-20
tags: problem
---


### 由于/usr/bin或/usr/sbin不在PATH 环境变量中，故无法找到该命令

`vim ~/.bashrc`

在最末尾添加`/usr/bin`或`/usr/sbin`:

`export PATH=$JAVA_HOME/bin:$JAVA_HOME/jre/bin:/usr/sbin:$PATH`


### ubuntu16.04 没有/var/log/messages

通过tail -f /var/log/messages命令可以监控系统的一举一动，实验的时候却发现没有这个文件。

解决办法：

`vim /etc/rsyslog.d/50-default.conf`

删掉以下四行的注释：

```
*.=info;*.=notice;*.=warn;\
    auth,authpriv.none;\
    cron,daemon.none;\
    mail,news.none      -/var/log/messages
```
运行下面命令重启rsyslog服务:

` sudo service rsyslog restart`


### 