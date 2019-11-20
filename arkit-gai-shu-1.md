# ARKit 概述

## 何为ARKit

要了解什么是ARKit，首先要理解什么是增强现实（Augmented Reality）。AR可以简单理解为将虚拟世界的信息叠加到现实世界中，它并非什么新鲜事物，当年红极一时的Google Glass就是典型的AR设备，另外，近年，HoloLens、Magic Leap等更轻便可靠的AR头显都相继推出。

AR具备三个主要特点：虚实结合，实时交互和三维注册（计算机生成的虚拟物体和真实的环境空间上对应，设备移动的时候，虚拟物体还可以位置正确的对准关系）。

目前，AR的应用场景正在不断拓展，在娱乐互动、智能营销、智能设备、景区行业、交互行业、汽车行业业已应用，艺术家甚至开始[用AR进行创作](https://www.apple.com/today/feature/augmentedrealities/?mnid=s7pA29YxT-dc_mtid_1870765e38482_pcrid_375854188485_pgrid_78554185882_&cid=aos-us-kwgo--brand-today-art--slid---product-&mtid=1870765e38482&aosid=p238&AnonymizeIP=set)。

种种迹象表明，和微软一样，Apple把对未来的期待放在AR而非VR上，据报道，Apple CEO Tim Cook多次表示看好AR的未来。他在一次[电视采访](https://www.androidpit.com/tim-cook-a-life-without-ar-unimaginable)上说：

> In a few years, we are not going to be able to imagine our lives without AR. It's that profound a platform.
>
> 在几年后，我们将不能想象没有AR的生活——这是一个意义深远的平台。

从技术发展角度上来看，制造真正的AR眼镜的技术尚不成熟，Apple公司决定先“构建平台”，也就是本教材的主人公，ARKit。

ARKit是Apple公司在2017年9月的全球开发者大会上推出的软件开发工具包，供开发者为iPhone和iPad制作增强现实应用程序。ARKit框架拥有强大的世界追踪、场景理解和渲染能力，还能和SceneKit、SpriteKit以及RealityKit（一个全新的高级增强现实框架）无缝结合。简而言之，ARKit是一个超级好用的AR应用开发工具。

## 开发环境

基于ARKit开发，需要准备以下的软硬件环境。

* iOS设备：ARKit不能使用模拟器进行开发，需要在一台具有A9处理器或更好配置的iOS设备上进行测试，包括iPhone 6s及以上、iPad 2017版及以上、iPad Pro全系列等机型
* iOS系统：iOS 11.0+
* 开发软件：Xcode 9.0+

从2017年ARKit亮相，到2019年推出ARKit 3，ARKit的功能越来越多，不同的功能对系统版本、CPU芯片、摄像头类型的要求不尽相同，如果没有达到相应的要求，很多功能是无法执行的，所以本教程建议的开发环境如下。

* iOS 13.0+
* Xcode 11.0+
* 具有Apple A12处理器的iOS设备，且有前置原深感摄像头

## 课程概要

当我们在逛博物馆的时候，精美的展品总会让我们赞叹不已，但是，如果不是相关领域的专家，在看展的过程中总会因为储备的不足，而错失很多展品背后的奇妙故事和知识。如果有一款AR应用，可以帮助我们更好地理解展品，让沉默的展品说话，甚至展品能否在虚拟世界中和我们互动，那会不会带来更有趣的观展体验呢？

![](.gitbook/assets/2.jpeg)

![](.gitbook/assets/3.jpeg)

![](.gitbook/assets/1.jpeg)

本教程将利用ARKit的各种强大功能，构建一款恐龙博物馆AR互动App。通过这个课程的学习，相信你就能释放自己的创意，用ARKit创建更多有趣好玩的AR应用啦。

## 工作原理

下图是Apple在其Apple Park的访客中心供游客使用的[AR应用](https://9to5mac.com/2017/11/17/hands-on-with-apple-park-visitor-centers-ar-campus-experience-video/)。该应用程序将树叶，照明效果和建筑物细节叠加到实物模型上，访问者在AR世界中可以一览整个园区，查看一天的光线变化，甚至可以看到建筑的内部。

![Photo by Patrick Schneider on Unsplash](.gitbook/assets/patrick-schneider-87oz2sov9ug-unsplash.jpg)



将真实世界和虚拟物体结合在一起，一个简单的AR系统的基本组成部分和需要的技术有：

1. 捕捉真实世界：如摄像头
2. 世界追踪：实时获取设备（手机）的位置姿态变化，追踪还包括人脸、图像、物体的追踪。
3. 场景理解：手机通过一系列的软件算法来分析相机图像，“理解“空间中水平面、垂直面以便在场景中放置物体，场景理解还包括理解环境中的光照环境，以便在虚拟场景中精确模拟真实的光照，以防物体看起来过亮或过暗。
4. 虚拟模型三维建模：3D立体建模
5. 虚拟物体和现实世界结合：将虚拟物体渲染到捕捉的真实世界中
6. 此外，还可以和虚拟物体进行交互，如拖动、旋转模型。

![ARKit&#x67B6;&#x6784;&#x56FE;](.gitbook/assets/5.png)

