---
title: 【Unity研究院】DepthOnly是Don'tClear效果
tags:
  - Unity
categories:
  - Unity
abbrlink: '214e002'
date: 2019-04-03 14:55:17
comments:
---
曾经一度以为这是Unity的一个Bug,后来才搞明白是为什么
<!-- more -->
# 一、如何触发
1. 新建一个unity场景
2. 右键新建一个圆球
3. 直接把场景中默认`Main Camera`的`ClearFlags`改为`DepthOnly`
4. 运行场景，拖动圆球，会发生下图场景  
![mark](/../../Photos/190403/depthonly.png)

很明显，这是Don'tClear的效果

# 二、如何解决
1. 再新建一个`Camera`,这个新建的`Camera`的`ClearFlags`默认为`Skybox` 或者改为`SolidColor`都行
2. 在运行就会发现正常了

# 三、情况分析
演示的很明确了，`DepthOnly`模式的摄像机不能单独使用，必须有其他摄像机作为背景的情况下才正常。可以这样理解，在摄像机的混合显示中，深度摄像机的数据是直接叠加到其他摄像机画面之上的，在没有其他摄像机的情况下，其他区域还保持上一帧的数据，就这样一帧一帧叠加就造成了这个结果。