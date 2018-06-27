---
title: 【Unity研究所】--为什么Inspector视图中脚本前面的勾选框没了
abbrlink: '7e842929'
tags:
  - Unity
categories:
  - Unity
date: 2018-01-01 00:46:34
comments:
---
## 引言
如下图所示  
![mark](http://p3goxj4ar.bkt.clouddn.com/blog/180616/Ec4GHkebjf.png?imageslim)  
在Unity开发中，有时会发现Inspector视图中脚本前面的勾选框会消失。<!-- more -->好奇就测试了下：  
## 经过测试
发现消失的情况是把新建脚本后把 `Start() `与 `Update()` 方法删除导致的。  
经过一系列测试，发现只要加上 `Update() 、LateUpdate() 、FixedUpdate()、 OnGUI() `任一方法，都能让勾选框出来。。。但是`Awake()`不行。