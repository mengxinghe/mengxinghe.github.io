---
title: 【Unity研究院】UGUI_监听右键
tags:
  - Unity
categories:
  - Unity
abbrlink: 9abb676d
date: 2019-04-03 15:37:10
comments:
---
前一段时间用UGUI做一个小工具，需要支持右键点击
<!-- more -->
# 直接上代码

话不多说，直接上代码，连中键都行啊
```
using UnityEngine;
using UnityEngine.Events;
using UnityEngine.EventSystems;

public class RightClick : MonoBehaviour, IPointerClickHandler
{
    public UnityEvent leftClick;
    public UnityEvent middleClick;
    public UnityEvent rightClick;
    public void OnPointerClick(PointerEventData eventData)
    {
        if (eventData.button == PointerEventData.InputButton.Left)
            leftClick.Invoke();
        else if (eventData.button == PointerEventData.InputButton.Middle)
            middleClick.Invoke();
        else if (eventData.button == PointerEventData.InputButton.Right)
            rightClick.Invoke();
    }
}
```
