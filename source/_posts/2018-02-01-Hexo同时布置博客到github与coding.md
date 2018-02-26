---
title: Hexo同时布置博客到github与coding
abbrlink: bc32acff
date: 2018-02-01 00:20:54
tags:
  - Hexo
  - Next
  - coding
categories:
  - Hexo
  - Next
comments:
---
# 1. 分别根据平台建立响应仓库



# 2. 站点配置文件`_config.yml`的修改
```
deploy:
  type: git
  repo:
    #github: git@github.com:mengxinghe/mengxinghe.github.io.git,master
    coding: git@git.coding.net:mengxinghe/mengxinghe.coding.me.git,master
```