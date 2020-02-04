# 化石修复术

## 创建参考对象

从开发的角度来看，对象识别和图像识别功能的工作方式和流程基本是一致的，主要区别在于识别的是三维物体而非平面图像，设置对象检测稍微复杂一些，对象引用创建也更复杂一些。

首先需要创建想要识别对象的参考信息，在图像检测中，我们可以创建、拍摄、扫描二维的图像，而对于3D的物体，参考的创建会更麻烦。ARKit提供了自己的API帮助我们仅仅使用iPhone就能完成参考对象的创建，Apple提供了所需的App



![](.gitbook/assets/32.png)![](.gitbook/assets/33.png)![](.gitbook/assets/34.png)

1. 选择iOS设备
2. 定位对象
3. 定义边界框
4. 扫描对象
5. 调整原点
6. 测试和导出

## 导入参考对象

![](.gitbook/assets/35.png)

## 配置对象跟踪

载入参考对象，修改之前创建的ARWorldTrackingConfiguration\(\)类的配置，就能让ARKit同时支持图像跟踪和对象跟踪，代码如下：

```text
// 设置参考对象
guard let referenceObjects = ARReferenceObject.referenceObjects(inGroupNamed: "AR Objects", bundle: nil) else{
    fatalError("Missing expected asset catalog resources.")
}
configuration.detectionObjects = referenceObjects
```

此时，新增的锚点有ARImageAnchor和ARObjectAnchor两种可能，因此要再renderer\(_:didAdd:for:\)方法中进行区分，为了程序清晰，将识别到图像和对象之后的操作封装成一个函数，renderer\(_:didAdd:for:\)替换为：

```swift
if let imageAnchor = anchor as? ARImageAnchor {
    crazyImage(imageAnchor, node)
}else if let objectAnchor = anchor as? ARObjectAnchor {
    crazyObject(objectAnchor, node)
}
```

## 显示信息

### 显示文字

![](.gitbook/assets/36.png)

```swift
// 让文字朝向摄像头
let billboardConstraints = SCNBillboardConstraint()
textNode.constraints = [billboardConstraints]

```

### 添加声音

在crazyObject（\_：\_ :\)添加一行

```swift
// 播放声音
node.addAudioPlayer(SCNAudioPlayer(source: SCNAudioSource(fileNamed: "growls.wav")!))
```

## 参考资料

{% embed url="https://developer.apple.com/documentation/arkit/scanning\_and\_detecting\_3d\_objects" %}

{% embed url="https://www.raywenderlich.com/6957-building-a-museum-app-with-arkit-2\#toc-anchor-015" %}

{% embed url="https://www.jianshu.com/p/dfe33efbdc11" %}

{% embed url="https://developer.apple.com/documentation/arkit/detecting\_images\_in\_an\_ar\_experience" %}



