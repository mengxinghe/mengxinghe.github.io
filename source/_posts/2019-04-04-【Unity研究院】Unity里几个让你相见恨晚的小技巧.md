---
title: 【Unity研究院】Unity里几个让你相见恨晚的小技巧
tags:
  - Unity
categories:
  - Unity
abbrlink: 49ab44d2
date: 2019-04-04 16:45:44
comments:
---
不多不少10个，很有用
<!-- more -->



# 1

如果编辑器意外崩溃了，但场景未保存，这时可以打开工程目录，找到/Temp/_Backupscenes/文件夹，可以看到有后缀名为.backup的文件，将该文件的后缀名改为.unity拖拽到项目视图，即可还原编辑器崩溃前的场景。

![img](https://mmbiz.qpic.cn/mmbiz_gif/ia8lKd7zKUsMLXMGUCQsTw0IIQs6WxWPVvibTicTxu4NYNU1tmeMTswDN8z45ib8NzpA5FAFMng0iaVfdocrdGjqJRA/640?wx_fmt=gif&tp=webp&wxfrom=5&wx_lazy=1)

# 2

所有数值类型的字段，都支持在检视面板中直接输入简单的数值表达式。

![img](https://mmbiz.qpic.cn/mmbiz_gif/ia8lKd7zKUsMLXMGUCQsTw0IIQs6WxWPVeib7icQjCN9pnddqK6y8PkYv3TbWZ4fMO7PiawFib7HqORiagJPiaDcFFkpw/640?wx_fmt=gif&tp=webp&wxfrom=5&wx_lazy=1) 

# 3

好不容易才调好的坐标，结果发现是在运行模式下，如果退出运行模式就还原了怎么办？可以在检视面板右键点击组件名，在弹出界面中选择Copy Component，然后退出运行模式后同样右键点击组件名，在弹出界面中选择Paste Component Values即可。

![img](https://mmbiz.qpic.cn/mmbiz_gif/ia8lKd7zKUsMLXMGUCQsTw0IIQs6WxWPVYw0Vwl0HX86AicibKQ0BfzicXUAgEmPCvf6rbEsBNw0Ficuo1aVoCesMUw/640?wx_fmt=gif&tp=webp&wxfrom=5&wx_lazy=1) 

# 4

分别按键盘键Q、W、E、R、T可以依次切换界面上的小工具，除此之外，按数字键2或3还可以切换场景为2D模式或3D模式。

![img](https://mmbiz.qpic.cn/mmbiz_gif/ia8lKd7zKUsMLXMGUCQsTw0IIQs6WxWPV39fZaF973q1ldhpGvwhF1hrEJDibrFA3qLNsPmM8x0DY1B7G5FbIfzQ/640?wx_fmt=gif&tp=webp&wxfrom=5&wx_lazy=1)

# 5

右键点击检视面板下方的预览窗口即可让预览窗口跳出来，然后自己选择合适的地方停靠，这样切换模型查看就不会影响到其它面板。想让预览窗口回到原位，只需右键点击窗口，在弹出菜单中选择Close Tab即可。

![img](https://mmbiz.qpic.cn/mmbiz_gif/ia8lKd7zKUsMLXMGUCQsTw0IIQs6WxWPVDeu3icicKLFeqnR5ibKMQdTdDfB5NQIC5AKWo89Uc44bibv8utuA1ZNbQA/640?wx_fmt=gif&tp=webp&wxfrom=5&wx_lazy=1) 

#  6

在层次视图的搜索框中输入完整的脚本或组件名称，即可找到所有绑定了该脚本或组件的对象。或者在搜索框中输入t:加上某个类别如light，即可找到使用同类组件的对象。

![img](https://mmbiz.qpic.cn/mmbiz_gif/ia8lKd7zKUsMLXMGUCQsTw0IIQs6WxWPVzX2ocXYEL2Q6TRAq1FBsFL5NNFhQbnhlLUXSHKTRgcILlp7VVXmR7w/640?wx_fmt=gif&tp=webp&wxfrom=5&wx_lazy=1)

# 7

如果想在检视面板查看脚本的私有变量，只需点击Inspectore，在弹出菜单中选择Debug即可。

![img](https://mmbiz.qpic.cn/mmbiz_gif/ia8lKd7zKUsMLXMGUCQsTw0IIQs6WxWPVBXVPibZyXeiaqTiaGsWTsPU5zhlSRtsHPa8wwbY5iaAJ9BMGK9xueVEOuA/640?wx_fmt=gif&tp=webp&wxfrom=5&wx_lazy=1) 

# 8

如果希望游戏运行第一帧暂停，可以先点击暂停按钮，然后点击播放按钮，这样程序就会在Update函数执行一次后暂停。

![img](https://mmbiz.qpic.cn/mmbiz_gif/ia8lKd7zKUsMLXMGUCQsTw0IIQs6WxWPVDL97q7Abx9h6usuDSFWOKU1lnmlkh0LvU9Ozf4NNcNScf1IzUibGic5A/640?wx_fmt=gif&tp=webp&wxfrom=5&wx_lazy=1) 

# 9

在使用Debug.Log函数时传递游戏对象给第二个参数，既可在点击控制面板的输出信息时自动定位到对应的游戏对象。

![img](https://mmbiz.qpic.cn/mmbiz_gif/ia8lKd7zKUsMLXMGUCQsTw0IIQs6WxWPVUxwxvB2F5vHa7jWbLw25PCrYYIw59d6Bw1wyjVFtuDxlTHpt28RLgw/640?wx_fmt=gif&tp=webp&wxfrom=5&wx_lazy=1) 

# 10

可以借助编辑器自带的标记功能为脚本分类，在检视面板中点击脚本图标下方的小三角，即可为脚本设置颜色或选择图标。

![img](https://mmbiz.qpic.cn/mmbiz_gif/ia8lKd7zKUsMLXMGUCQsTw0IIQs6WxWPVibw2C5tviaIRf3QfB8tfag2nWBnhbqyjz21AU45thEFLOcSomVdgHtJw/640?wx_fmt=gif&tp=webp&wxfrom=5&wx_lazy=1) 