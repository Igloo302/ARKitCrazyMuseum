# 读懂一幅图

## 图像跟踪流程

在之前的课程中，我们已经学习了在环境中检测平面，并将模型放置在平面上，这也是一种常见的放置模型的方式，适用于很多AR游戏的场景布置。

在本节中，我们将继续学习另一种常见的放置虚拟场景的方式：以特定的图片为参考物建立坐标，在图像上叠加信息。例如，当用户识别一张图画，博物馆应用可以显示虚拟策展人，而当玩家将设备指向棋盘时，可以在棋盘上显示虚拟棋子。

一个基本的图像跟踪流程非常简单：

1. 创建参考图，也就是被识别的图片
2. 将参考图放在资源目录的AR Resources组中
3. 设置ARKit会话，其中图像检测
4. 在会话配置中加载参考图
5. 启动识别图像的ARKit会话
6. 识别到图像时添加锚点并回调
7. 将想要的虚拟模型添加到场景中或执行其他操作

## 添加参考图像

首先，我们需要准备参考的图像，也就是让AR识别的图片，这张图片可以是拍摄的照片，也可以从网上下载。为了最好的识别效果，清晰、对比度大的照片将更容易被识别。下图是从Unsplash获取的霸王龙图片。

![Photo by Huang Yingone on Unsplash](.gitbook/assets/24.jpeg)

我们使用ARKit模板重新创建一个项目CrazyDetect。打开项目的资产目`Assets.xcassets`，右击空白处或单击底部的`+`按钮添加新的AR资源组`New AR Resource Group`，将其重命名为`AR Resources`。

![&#x521B;&#x5EFA;New AR Resource Group](.gitbook/assets/25.png)

将准备好的参考图片拖到Xcode中的`AR Resources`中，不出意外的话，你将会获得多个黄色三角形警告⚠️。最常见的是下图，这种警告是`Unsupported Configuration`，这是因为参考图像必须具有非零的正宽度，也就是说，ARKit在识别图像的时候，会考虑图像的实际尺寸。

![warning: Unsupported Configuration: AR reference image &quot;Mammoth&quot; must have non-zero, positive width.](.gitbook/assets/26.png)

在右侧的属性设置中，将每张AR参考图设置宽度，根据纵横比，高度将自动设置。

![&#x8BBE;&#x7F6E;&#x56FE;&#x7247;&#x5BBD;&#x5EA6;](.gitbook/assets/27.png)

尺寸设置完成后，可能还会有其他的警告，比如`Ambiguous Content`是因为图像太小，没有足够对比度，具有大量空白空间，颜色太少或缺乏独特形状的图像，以至于难以检测。下图中的Pterosaur就是颜色太过单一而难以被检测。在这种情况下，可以暂时忽略警告，实际的检测效果可能可以接受。

![warning: Ambiguous Content: AR reference image &#x201C;Pterosaur&#x201D;: The histogram of the image is narrow or not well distributed. Recognition works better on images with wider, flatter histograms.](.gitbook/assets/28.png)

现在，是时候继续编写代码来识别这些图像了。

## 配置图像跟踪

在viewWillAppear\(\_:\) 中，添加一个新的常量了设置参考图像，这将使用您刚刚在资源目录中创建的`AR Images`组中的图像创建`ARReferenceImage`集

```swift
guard let referenceImages = ARReferenceImage.referenceImages(
        inGroupNamed: "AR Resources", bundle: nil) else {
        fatalError("Missing expected asset catalog resources.")
    }
```

然后，将配置`configuration`更改为`ARWorldTrackingConfiguration()` ，然后将`referenceImages`添加到配置中。

```swift
// Create a session configuration
let configuration = ARWorldTrackingConfiguration()
configuration.detectionImages = referenceImages
```

## 可视化检测到的图像

和检测平面相似，当图像被检测到的时候，ARSession会添加ARImageAnchor，并且调用renderer\(\_:didAdd:for:\)方法，代码如下：

```text
func renderer(_ renderer: SCNSceneRenderer, didAdd node: SCNNode, for anchor: ARAnchor) {
    // 获取锚点对应的图片
    guard let imageAnchor = anchor as? ARImageAnchor else { return }
    let referenceImage = imageAnchor.referenceImage
    // 输出检测到的图片的名字
    let name = referenceImage.name!
    print("you found a \(name) image")
    // 创建一个平面来显示检测到的图片(与图片相同大小)
    let plane = SCNPlane(width: referenceImage.physicalSize.width,
                         height: referenceImage.physicalSize.height)
    let planeNode = SCNNode(geometry: plane)
    planeNode.opacity = 0.25
    // `SCNPlane`在它的本地坐标系是竖直的, 但`ARImageAnchor`会假设图片在自身本地坐标系中是水平的,所以要旋转平面.
    planeNode.eulerAngles.x = -.pi / 2
    // Add the plane visualization to the scene.
    node.addChildNode(planeNode)
}

```

![](.gitbook/assets/29.png)

在检测到的图片上显示半透明长方形

再给这个半透明的长方形添加一个高亮动画，长方形就会在高亮后消失

```text
// 高亮动画
var imageHighlightAction: SCNAction {
    return .sequence([
        .wait(duration: 0.25),
        .fadeOpacity(to: 0.85, duration: 0.25),
        .fadeOpacity(to: 0.15, duration: 0.25),
        .fadeOpacity(to: 0.85, duration: 0.25),
        .fadeOut(duration: 0.5),
        .removeFromParentNode()
    ])
}
```

在renderer\(\_:didAdd:for:\)方法进行调用

```text
// 显示高亮动画
planeNode.runAction(self.imageHighlightAction)
```

### 显示信息

### 显示图片

### 播放视频

![](.gitbook/assets/30.png)

Video by Engin Akyurt from Pexels

将Tyrannosaurus.mp4导入到项目中，创建函数makeVideoNode\(\)创建生成包含视频的Node

```swift
func makeVideoNode(size:CGSize ) -> SCNNode?{
    // 从资源包中抓取文件名为Tyrannosaurus.mp4的视频
    guard let videoURL = Bundle.main.url(forResource: "Tyrannosaurus", withExtension: "mov") else {return nil}
    // 为该视频创建和启动AVPlayer
    let avPlayerItem = AVPlayerItem(url: videoURL)
    let avPlayer = AVPlayer(playerItem: avPlayerItem)
    avPlayer.play()
    // AVPlayer实例不会自动重复。此通知块通过监听播放器来完成视频循环。然后它回到开头并重新开始。
    NotificationCenter.default.addObserver(
        forName: .AVPlayerItemDidPlayToEndTime,
        object: nil,
        queue: nil) { notification in
            avPlayer.seek(to: .zero)
            avPlayer.play()
}

```

### 显示文字

### 小动画

## 参考资料

{% embed url="https://www.raywenderlich.com/6957-building-a-museum-app-with-arkit-2\#toc-anchor-015" %}

{% embed url="https://www.jianshu.com/p/dfe33efbdc11" %}

{% embed url="https://developer.apple.com/documentation/arkit/detecting\_images\_in\_an\_ar\_experience" %}





