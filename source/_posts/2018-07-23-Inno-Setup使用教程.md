---
title: Inno Setup使用教程
tags:
  - 软件打包
  - Inno Setup
categories:
  - 工具
abbrlink: b5cc1603
date: 2018-07-23 14:01:26
comments:
---
# Inno Setup 是什么？

 InnoSetup 是一个免费的 Windows 安装程序制作软件。第一次发表是在 1997 年，Inno Setup 今天在功能设置和稳定性上的竞争力可能已经超过一些商业的安装程序制作软件。

<!-- more -->

# 1. 初级使用

## 1.1 安装InnoSetUp

下载地址：http://www.jrsoftware.org/isdl.php

## 1.2 下载中文语音包

![mark](http://p3goxj4ar.bkt.clouddn.com/blog/180723/ebIhjL9BkH.png?imageslim)

Unofficial translations（非官方翻译）-Chinese (Simplified)（中文简体）-5.5.3+-右键-目标另存为，如下图所示：![mark](http://p3goxj4ar.bkt.clouddn.com/blog/180723/dkE6LfkHiL.png?imageslim)

ChineseSimplified.isl文件放入“C:\Program Files (x86)\Inno Setup 5\Languages”目录。

## 1.3 打包程序

1.3.1 新建脚本

![mark](http://p3goxj4ar.bkt.clouddn.com/blog/180723/e2a89CG88G.png?imageslim)

1.3.2 输入产品与公司信息

![mark](http://p3goxj4ar.bkt.clouddn.com/blog/180723/64mK78i66J.png?imageslim)

1.3.3 输入程序安装文件夹名称

![mark](http://p3goxj4ar.bkt.clouddn.com/blog/180723/AaABb11L8L.png?imageslim)

1.3.4 添加主程序地址与程序文件夹

![mark](http://p3goxj4ar.bkt.clouddn.com/blog/180723/aEJ3IDBdjK.png?imageslim)

1.3.5  开始菜单文件夹相关与座面快捷方式相关设置

![mark](http://p3goxj4ar.bkt.clouddn.com/blog/180723/DHjme0DffK.png?imageslim)

1.3.5 添加许可文件

![mark](http://p3goxj4ar.bkt.clouddn.com/blog/180723/BGAH1l18Gi.png?imageslim)

1.3.6 安装语言选择，可多选（刚安装的中文支持就在这用）

![mark](http://p3goxj4ar.bkt.clouddn.com/blog/180723/6hd2fi11fA.png?imageslim)

1.3.7 选择输出文件夹与图标选择

![mark](http://p3goxj4ar.bkt.clouddn.com/blog/180723/l8HK7GlA8k.png?imageslim)

1.3.8 使用编译命令

![mark](http://p3goxj4ar.bkt.clouddn.com/blog/180723/9L63gK57H7.png?imageslim)

1.3.9  Finish 如下图

![mark](http://p3goxj4ar.bkt.clouddn.com/blog/180723/eBgj85CBAe.png?imageslim)

1.3.10  是否编译

![mark](http://p3goxj4ar.bkt.clouddn.com/blog/180723/lbbkJciB5G.png?imageslim)

1.3.11 是否保存脚本（选择否，脚本自动保存”我的文档“文件夹），如下图所示： 

 ![mark](http://p3goxj4ar.bkt.clouddn.com/blog/180723/4llh5aeaGB.png?imageslim)



1.3.12 保存脚本

![mark](http://p3goxj4ar.bkt.clouddn.com/blog/180723/FB2dgEJJ65.png?imageslim)



1.3.13 编译完成。



# 2.扩展

## 2.1 脚本文件

上面的过程生成了一个后缀为`iss`的脚本文件，脚本控制着安装程序的所有方面。由它指定哪些文件将被安装到什么地方，在哪里创建快捷方式，且被命名为什么。  脚本文件一般可以用安装程序编译器程序内置的编辑器进行编辑。在你编写完脚本后，下一个最终步骤就是选择安装程序编译器中的“编译”。创建完成后，就可以运行根据你脚本编译的安装程序了。 

## 2.2 添加开始菜单快捷方式

用编辑器创建脚本最多自动添加三个快捷方式，分别是主程序快捷方式，卸载快捷方式与公司网站快捷方式，如果我想添加其他快捷方式怎么办呢？打开脚本做如下修改：

``` reStructuredText
[Icons]
Name: "{group}\{#MyAppName}"; Filename: "{app}\{#MyAppExeName}"
Name: "{group}\other"; Filename: "{app}\other"
；上面添加了一个自定义的一个快捷方式
Name: "{group}\日志"; Filename: "{app}\LOG"
；这里添加了一个自定义的文件夹
```

说明：

* `{group}`后是快捷方式的名称
* `Filename`指定目标地址,`{app}\`对应着程序文件夹
* `｛｝`中的内容是前面定义的变量

## 2.3 添加桌面快捷方式

与上面类似，只不过需要定义`[Tasks]`字段，`Icons` 里后面需要指定任务。

```
[Tasks]
Name: "desktopicon"; Description: "{cm:CreateDesktopIcon}"; GroupDescription: "{cm:AdditionalIcons}"; Flags: unchecked

[Icons]
Name: "{commondesktop}\{#MyAppName}"; Filename: "{app}\{#MyAppExeName}"; Tasks: desktopicon
```

