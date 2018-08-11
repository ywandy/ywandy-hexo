---
title: 解决gnome3自己添加的程序dock栏图标重复问题
date: 2018-05-10 00:22:43
tags:
    - Gnome
    - freedesktop
categories: 
    - 技巧总结
---

### 背景： 
最近我的主力笔记本**Archlinux**开始用**Gnome3**桌面，主要是**Kde**看腻了，Gnome3总体感觉还是非常好看的，这里先秀上一张我的**Gnome3**桌面，嘻嘻。
![2018-05-10 00-28-19 的屏幕截图](http://owm7tyxac.bkt.clouddn.com/2018-05-10%2000-28-19%20的屏幕截图.png)  
但是**Gnome3**安装完后，我以前在**Kde**桌面的程序入口全部都没有了，包括我以前新建的一些程序快捷方式，因此只能按照**freedesktop**的标准去编写 **.desktop** 文件
然后编写的时候发现一个问题，就是我的 **.desktop** 文件虽然能够在**Gnome**的所有程序找到，也可以添加到收藏夹![2018-05-10 00-33-43 的屏幕截图](http://owm7tyxac.bkt.clouddn.com/2018-05-10%2000-33-43%20的屏幕截图.png) 
但是存在一个问题，就是会出现如下当应用程序启动后**多个**图标问题。 
![2018-05-10 00-36-22 的屏幕截图](http://owm7tyxac.bkt.clouddn.com/2018-05-10%2000-36-22%20的屏幕截图.png) 
这种问题对于追求完美的我来说，怎么能够忍受呢？因此，下面教程将会用一个简单的方法去处理这个问题。 

### 解决方案： 
我们首先来对比一下，其他没有问题的应用程序的**.desktop**是怎么编写的，和我们有什么不同。 
- **以下是一个正常没有出现双图标的.desktop**： 
![2018-05-10 00-38-31 的屏幕截图](http://owm7tyxac.bkt.clouddn.com/2018-05-10%2000-38-31%20的屏幕截图.png)

- **以下是我们出现双图标的eclipse的```.desktop```**: 
![2018-05-10 00-41-07 的屏幕截图](http://owm7tyxac.bkt.clouddn.com/2018-05-10%2000-41-07%20的屏幕截图.png) 

通过对比两图我们发现，正常的**.desktop**最下面有一行```StartupWMClass=```，这个也是我们今天的解决办法，下面，我们如何知道我们的```StartupWMClass=```需要填入什么呢，于是，我们需要接触一个新工具```xprop WM_CLASS```  
　　当我们在终端执行以下命令:
```bash
# yewei @ yewei in ~/yyw/code/ywandy.github.io on git:master x [0:45:13] 
$ xprop WM_CLASS 
```  
屏幕会出先一个像截图的 X ，然后这时候，我们用 X 去点击我们的eclipse窗口（对于其他应用一样，就去点击那个应用的窗口就好了） 
　　此时的命令行会出现: 
```bash
# yewei @ yewei in ~/yyw/code/ywandy.github.io on git:master x [0:47:18] C:130
$ xprop WM_CLASS
WM_CLASS(STRING) = "Eclipse", "Eclipse"
```  
我们只需要把Eclipse填入我们的eclipse的```.desktop```最后一行即可，然后尝试启动eclipse 
最终的```.desktop```是这样的： 
![2018-05-10 00-49-22 的屏幕截图](http://owm7tyxac.bkt.clouddn.com/2018-05-10%2000-49-22%20的屏幕截图.png) 

此时，自建的```.desktop```导致dock栏的双图标问题已经解决了: 
![2018-05-10 00-50-29 的屏幕截图](http://owm7tyxac.bkt.clouddn.com/2018-05-10%2000-50-29%20的屏幕截图.png) 


##附录:
- 完整的```eclipse.desktop```文件： 

{% codeblock %}
    [Desktop Entry]
    Type=Application
    Name=Eclipse-cpp
    Comment=Eclipse Integrated Development Environment
    Icon=/home/yewei/yyw/application/eclipse-cpp/icon.xpm
    Exec=/home/yewei/yyw/application/eclipse-cpp/eclipse
    Terminal=false
    Categories=Development;IDE;Java;
    StartupWMClass=Eclipse
{% endcodeblock %}
