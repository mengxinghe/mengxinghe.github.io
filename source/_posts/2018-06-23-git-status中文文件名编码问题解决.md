---
title: git status中文文件名编码问题解决
tags:
  - git
categories:
  - 工具
abbrlink: 69af23b9
date: 2018-06-23 19:44:07
comments:
---
# git status中文文件名编码问题解决

在默认设置下，中文文件名在工作区状态输出，中文名不能正确显示，而是显示为八进制的字符编码。<!-- more -->

通过将git配置变量 `core.quotepath` 设置为`false`，就可以解决中文文件名称在这些Git命令输出中的显示问题,
```
：git config --global core.quotepath false
```

![mark](/../../Photos/180623/B275LHhLi8.png)