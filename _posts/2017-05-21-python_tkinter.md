---
layout: post
title: python之tkinter实例化学习
date: 2017-05-21
tags: python
---


转载自：[jihite](http://www.cnblogs.com/kaituorensheng/p/3287652.html#_label3)

### 阅读目录

[1. 产品介绍](#1)

[2. 设计规划](#2)

[3. 相关知识](#3)

[4. 源码附件](#4)

**tkinter**模块("Tk接口")是Python的标准Tk GUI工具包的接口， 是python的内置模块，直接`import tkinter`即可使用。

作为实践，用tkinter做了个**ascii码转化查询表**

<h3 id="1">1. 语法示例</h3>

**界面**

<br/>
   
![](/images/posts/tkinter/image1.png)

<br/>

**功能**

<ul>
<li>通过<strong>输入</strong>字符或数字查询对应的信息</li>
<li>通过<strong>选择</strong>列表中的信息查询对应的信息</li>
</ul>


<h3 id="2">2. 设计规划<h3>

**规划图**

<br/>
   
![](/images/posts/tkinter/image2.png)

<br/>

<h3 id="3">3. 相关知识<h3>

首先看怎么产生第一个窗口

    from tkinter import *   #引用Tk模块
    root = Tk()             #初始化Tk()
    root.mainloop()         #进入消息循环

几个常用属性

<ul>
<li>title: 设置窗口标题</li>
<li>geometry: 设置窗口大小</li>
<li>resizable(): 设置窗口是否可以变化长 宽</li>
</ul>

    # -*- coding: cp936 -*-
    from Tkinter import *
    root = Tk()
    root.title("hello world")
    root.geometry('200x100')                 #是x 不是*
    root.resizable(width=False, height=True) #宽不可变, 高可变,默认为True
    root.mainloop()
    
先写到这，下次在继续