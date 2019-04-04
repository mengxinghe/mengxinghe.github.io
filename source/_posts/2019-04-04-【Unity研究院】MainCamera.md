---
title: 【Unity研究院】MainCamera
tags:
  - Unity
  - MainCamera
categories:
  - Unity
abbrlink: 86c963d1
date: 2019-04-04 10:15:43
comments:
---

我们都知道，在Unity中，我们可以通过`Camera.main`这个接口获取场景中的主摄像机，但如果场景中有多个摄像机，获取的应该是哪个呢？
<!-- more -->
看下解释就知道了，如下图
![Camera](../../Photos/190404/mainCamera.jpg)

获取的是场景中第一个启用而且同时`Tag`是`MainCamera`的摄像机。
所以，想让哪个摄像机成为主摄像机，改下标签就好了。