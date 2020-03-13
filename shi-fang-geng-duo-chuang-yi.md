# 释放更多创意

## ARKit 3 新特性

美国时间2019年6月3日，苹果WWDC 2019在美国圣何塞召开，苹果不出意外地发布了在增强现实领域的最新进展，包括3大亮点——Realitykit、Reality Composer以及全新版本的ARKit 3。

### Realitykit和Reality Composer

Realitykit是苹果推出的全新AR框架，Realitykit主要由四大组件构成：ARView, Anchor, Scene, Entity。相比我们已经比较熟悉的SceneKit，它在实现渲染、动画、物理效果、网络同步、音频等方面更为强大，使用更为简单。

![Realitykit](https://images.xiaozhuanlan.com/photo/2019/a4f13710bb0812b62ac6b555446798d6.png)

至于Reality Composer则是一款专为AR应用设计的建模、构建动画和交互的工具，可以快速制作原型并为AR体验生成内容。它内置了数百个可以自由使用的AR模型，开发者开始自由组合编辑模型，甚至能制作动画，Reality Composer让开发者不需要有很强的3D技术就能创建简单的AR素材。

![Reality Composer](.gitbook/assets/image%20%2831%29.png)

![&#x4F7F;&#x7528;Reality Composer&#x53EF;&#x4EE5;&#x65B9;&#x4FBF;&#x5730;&#x7F16;&#x8F91;&#x6750;&#x8D28;](.gitbook/assets/image%20%2826%29.png)

### ARKit 3

ARKit 3进行了全新的升级，其中有两大亮点。

#### People Occlusion

之前的学习中相比各位读者已经发现，AR信息在屏幕画面中显示时，会遮挡住真实的物体，这个限制制约了在部分场景下AR体验的真实感和沉浸感。People Occlusion功能可以将画面中的人物抠出来，ARKit 3 中的这个新特性将允许人们在 AR 世界中围着虚拟对象移动（甚至穿过虚拟对象）。

#### 动作捕捉

ARKit 3可以通过手机摄像头可以实时捕捉人体全身的动作，包括手臂、躯干等等，通过这个技术，用户就能用肢体的动作而非屏幕操作和AR进行交互了。  
  
此外ARKit 3还支持一次跟踪最多三个面孔，同时使用前置和后置摄像头来进行面部和现实场景跟踪等功能。

## 结语

在5G+AI浪潮到来之际，AR作为一个备受瞩目的新技术，拥有无限的想象空间。通过这份教材的学习，相比读者应该对ARKit的基本知识和使用方法有了一定的了解。但是，距离一个完整的iOS应用的实现，还需要学习很多知识。希望各位读者再接再厉，在现有的基础上，将更多的创意和AR进行结合，创造出杀手级的AR应用。

