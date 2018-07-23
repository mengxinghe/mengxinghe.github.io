---
title: windows 新建.gitignore
tags:
  - null
  - null
categories:
  - null
  - null
abbrlink: 2ee23ef
date: 2018-06-28 10:50:26
comments:
---
我们都知道`GIT`使用`.gitignore`文件来保存忽略列表，但是在`window`下新建文件时必须输入文件名（`windows`认为`.gitignore`是一个后缀），否则就会报下面的错误
<!-- more -->
![mark](http://p3goxj4ar.bkt.clouddn.com/blog/180628/2CdchaAfIe.png?imageView2/0/q/75|watermark/2/text/bXJzb29uZy5jb20=/font/5qW35L2T/fontsize/600/fill/IzAwMDAwMA==/dissolve/100/gravity/SouthEast/dx/10/dy/10)

怎么办呢？
## 解决方案

就是输入时`.gitignore`后多加一个点`.gitignore.`，这样就没问题啦