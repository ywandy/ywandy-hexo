---
title: 为python3编译安装opencv3.4
date: 2018-05-04 15:52:32
tags: 
    - opencv
    - 计算机视觉
    - 人工智能
    - python 
categories:
    - 技巧总结
layout : false
---

### 本文关于如何在Archlinux下面，基于Anaconda3+编译安装opencv+Pycharm的opencv开发环境搭建
### 引言：  
    前段时间折腾opencv，在图形工作站上面能够很好的运行，然后想着给我现在用的Archlinux笔记本也装上个Anaconda3+opencv+pycharm的opencv开发环境 
尝试过使用conda安装的opencv，在conda环境能够正确```import cv2```  但是在执行```cv2.inshow()```时候就会报错，分析原因后，是因为conda安装的opencv没有带gtk库的支持，而```cv2.imshow()```因为要调用到gtk做图形界面，因此就有了重新编译opencv3的想法。  

### 安装编译的依赖：  
1.cmake 和 gcc  
2.ffmpeg或者libav库(libavcodec-dev, libavformat-dev, libswscale-dev)等
3.Git  
4.使用Conda安装的numpy  
具体的库安装指令:
```sudo pacman -S cmake ffmpeg libdc1394 libgphoto2 gtkglext hdf5 