---
title: 【Unity 研究院】_Awake函数的小坑
abbrlink: 541afe79
tags:
  - Unity
categories:
  - Unity
date: 2018-01-01 00:00:00
comments:
---
## 引言  
 曾经遇到过，记录下来。有次在`Update()`里写里一些东西，调试死活不执行，检查发现脚本自动关闭了。<!-- more -->还以为Unity出了bug。找了好久才发现是`Awake()`的坑
## 正题

随便建个脚本
```C#
 private void Awake()
    {
        throw new Exception("awakerr");
    }  
```
执行就会发现：  
![mark](/../../Photos/180617/3hJhgK7GmH.png)  
脚本自动关闭了。

测试发现，Start() 与 Update() 都不会这样。不晓得什么逻辑