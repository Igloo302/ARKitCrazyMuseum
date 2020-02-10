# 一起合影吧

在之前的章节中，我们已经学习了如何进行图像追踪和对象追踪，接下来我们将学习iPhone X ARKit的强大功能：追踪和可视化人脸。人脸作为一种特殊的对象，想必大家已经想到其工作机制了。那么，第一步是创建参考人脸吗？答案是否定的。无论是男人、女人、老人还是小孩，都是一个鼻子一张嘴，两只眼睛两个耳朵，因此我们并不需要像对象追踪那样，首先拿着手机扫描自己的脸。下图是ARKit 以 `ARFaceGeometry` 格式提供面部三角网格，在识别的过程中，可以简单理解为，ARKit能调整这个三角网格的形状，贴合出现在摄像头画面中的脸。

![](.gitbook/assets/image%20%2815%29.png)

在本节中，我们将利用TrueDepth前置摄像头追踪人脸，并以人脸的锚点ARFaceAnchor来添加AR效果。

如概述中说明，此项目要求：

* 带有TrueDepth前置摄像头的iOS设备：
  * iPhone X，iPhone XS，iPhone XS Max或iPhone XR或更高版本
  * iPad Pro（11英寸）或iPad Pro（12.9英寸，第三代）。
* iOS 11.0或更高版本。
* Xcode 10.0或更高版本。

## 追踪人脸

打开上一节的项目，如果您使用的设备已经更新到了iOS 13，那只需要增加几行代码，就能启动人脸追踪了。

```swift
if ARFaceTrackingConfiguration.isSupported {
    configuration.userFaceTrackingEnabled = true
}
```

`userFaceTrackingEnabled`是iOS 13最新支持的一个标志，用于确定ARKit是否在世界跟踪会话`ARWorldTrackingConfiguration()`中跟踪用户的面部。在提供需要面部跟踪AR会话的功能之前，需要检查`ARFaceTrackingConfiguration.isSupported`类的属性，以确定当前设备是否支持ARKit面部跟踪。

## 拍照按钮

如何把有趣的一幕永存？让我们在App中实现拍照的功能吧。

在Xcode的左侧项目目录中点击`Main.storyboard` 出现如下界面：

![](.gitbook/assets/image%20%281%29.png)

StoryBoard 的本质是一个 XML 文件，描述了若干窗体、组件、Auto Layout 约束等关键信息。使用StoryBoard可以让我们用可视化的方式去编辑App的界面。





## 参考资料

{% embed url="https://developer.apple.com/documentation/arkit/tracking\_and\_visualizing\_faces" %}

{% embed url="https://juejin.im/post/5b2349aee51d4558842a9a0b" %}





