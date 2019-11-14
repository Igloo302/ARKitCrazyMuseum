# ARKit 概述

## ARKit简介

## 主要特性

## 开发环境

硬件：ARKit不能使用Simulator进行开发，需要在一台具有A9处理器或更好配置的iOS设备上进行测试，包括iPhone 6s及以上、iPad 2017版及以上、iPad Pro全系列等机型

测试系统：iOS 12.4.1

开发软件：Xcode 10

## 工作原理及流程

https://blog.csdn.net/u013263917/article/details/72903174

一个简单的AR场景实现所需要的技术和实现步骤如下：

1. 多媒体捕捉现实图像：如摄像头
2. 三维建模:3D立体模型
3. 传感器追踪:主要追踪现实世界动态物体的六轴变化，这六轴分别是X、Y、Z轴位移及旋转。其中位移三轴决定物体的方位和大小，旋转三周决定物体显示的区域。
4. 坐标识别及转换：3D模型显示在现实图像中不是单纯的frame坐标点，而是一个三维的矩阵坐标。这基本上也是学习AR最难的部分，好在ARKit帮助我们大大简化了这一过程。
5. 除此之外，AR还可以与虚拟物体进行一些交互。

![](.gitbook/assets/4.png)

另外让开发者们惊喜的就是ARKit对 Unity3D和Unreal也是全线支持。我们来看看ARKit的架构图：![](.gitbook/assets/5.png)

[https://blog.csdn.net/xiangzhihong8/article/details/77770485](https://blog.csdn.net/xiangzhihong8/article/details/77770485)

### 核心类

https://www.jianshu.com/p/d0721aabcbf7

AR工程中有一个ARSCNView，它用来加载3D模型的AR视图的，它继承于SCNView，相对的加载2D视图的就是ARSKView。

SpriteKit是用来创建2D模型，在游戏开发中，指的是以图像方式呈现在屏幕上的一个图像。这个图像也许可以移动，用户可以与其交互，也有可能仅只是游戏的一个静止的背景图。而在AR中，2D模型会随着手机的远近放大缩小，而不能像3D模型那样可以从侧面观察。

![](.gitbook/assets/6.png)

SCeneKit结构图

## 课程概要

🦕️恐龙博物馆

![](.gitbook/assets/1.jpeg) ![](.gitbook/assets/2.jpeg) ![](.gitbook/assets/3.jpeg)

## 开发环境

## 学习资源

