---
layout: post
title: ������һЩ����
date: 2017-10-20
tags: problem
---


### ����/usr/bin��/usr/sbin����PATH ���������У����޷��ҵ�������

`vim ~/.bashrc`

����ĩβ���`/usr/bin`��`/usr/sbin`:

`export PATH=$JAVA_HOME/bin:$JAVA_HOME/jre/bin:/usr/sbin:$PATH`


### ubuntu16.04 û��/var/log/messages

ͨ��tail -f /var/log/messages������Լ��ϵͳ��һ��һ����ʵ���ʱ��ȴ����û������ļ���

����취��

`vim /etc/rsyslog.d/50-default.conf`

ɾ���������е�ע�ͣ�

```
*.=info;*.=notice;*.=warn;\
    auth,authpriv.none;\
    cron,daemon.none;\
    mail,news.none      -/var/log/messages
```
����������������rsyslog����:

` sudo service rsyslog restart`


### 