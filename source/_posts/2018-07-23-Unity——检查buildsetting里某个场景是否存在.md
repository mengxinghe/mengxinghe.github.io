---
title: Unity——检查buildsetting里某个场景是否存在
tags:
  - Unity
categories:
  - Unity
abbrlink: e8edee68
date: 2018-07-23 14:06:05
comments:
---
偶然用到，发现Unity 里没有直接查询的API。<!-- more -->

代码：

``` C# 
    bool SceneExist(string SceneName)
    {
        for (int i = 0; i < SceneManager.sceneCountInBuildSettings; i++)
        {
            string sp = SceneUtility.GetScenePathByBuildIndex(i);
            string nam = sp.Remove(0, sp.LastIndexOf('/') + 1);
            nam = nam.Remove(nam.LastIndexOf('.'));
            if (SceneName == nam)
            {
                return true;
            }
        }
        return false;
    }

```

