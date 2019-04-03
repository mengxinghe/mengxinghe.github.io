---
title: 【Unity研究院】Shader.Find()
tags:
  - shader
  - Unity
categories:
  - Unity
abbrlink: 49ad13f6
date: 2019-04-03 15:30:27
comments:
---
Shader.Find的一个坑，unity打包的时候，不会打包那些没有被引用的shader,所以编辑器下正常，打包后就报错。
<!-- more -->
# 解决方案
为了双保险.

1. 可以首先放到`Resources`文件夹里，能保证shader被打到apk里。
2. 在`ProjectSettings->Graphics`里的`always included shaders`添加此shader,使shader预加载，避免在第一次执行Shader.Find的时候出现卡顿。