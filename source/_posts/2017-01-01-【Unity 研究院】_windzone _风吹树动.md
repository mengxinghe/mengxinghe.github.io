---
title: 【Unity 研究院】--WindZone
abbrlink: ebe7573d
tags:
  - Unity
categories:
  - Unity
date: 2017-01-01 00:00:00
comments:
---
# windzone _风吹树动  
## 引言  
 用unity很久了，一直有留意到创建菜单`3D Object`下有个`Wind Zone`，看意思大概是风洞之类的效果，但一直没用过。今天有空研究研究

## 正题

 研究发现，原来这个东西是作用在地形上种的树的。
 效果如下  
 ![mark](http://p3goxj4ar.bkt.clouddn.com/blog/180617/EJ2J9BFgbi.gif)

 ## 注意点  

如果要是自己用3dmax制作的树，要注意两点：
1. 植物模型的材质类型必须使用植物专用的Nature中的类别
2. 地形编辑添加树木素材时需要把Bend Factor 数值开启  

![mark](http://p3goxj4ar.bkt.clouddn.com/blog/180617/KKm2jAjhaA.png?imageslim)