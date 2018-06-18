---
title: 从Windows10的资源管理器里面删除OneDrive图标
tags:
  - Windows10
abbrlink: ed89bd7f
date: 2018-06-18 15:05:09
categories:
comments:
---
# 从Windows10的资源管理器里面删除OneDrive图标

## 引言
有时候感觉这个OneDrive图标在资源管理器里面看着碍眼，但又不想删除OneDrive程序，可以通过编辑注册表来隐藏这个图标，不影响OneDrive程序自身功能。

*  按下`Win+X`键或者在开始菜单右键点击，出现的菜单中选择“运行”。或者`Win+R`键在弹出的运行窗口输入regedit点击确定，打开注册表编辑器。
*  鼠标单击`HKEY_CLASSES_ROOT`项目，选中它。然后“编辑”菜单点击“查找”。
*  找到键值`System.IsPinnedToSpaceTree`键，双击该键值修改数值为0
* 然后重新打开Windows资源管理器，就会发现OneDrive图标已经没有了。如果没有消失，重启电脑即可。
* 如果再打开注册表的过程中出现用户账户控制界面点击“是”即可。也可以在控制面板\用户帐户\用户帐户里面关闭UAC提示。