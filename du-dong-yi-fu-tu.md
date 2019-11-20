# 读懂一幅图

## 图像跟踪流程

1. 创建参考图
2. 将参考图放在资源目录的AR Resources组中
3. 设置ARKit会话
4. 在会话配置中加载参考图
5. 启动ARKit会话
6. 识别到图像时添加锚点并回调
7. 讲想要的节点模型添加到场景中或其它操作

使用ARKit模板新建一个项目CrazyDetect

## 导入参考图像

![](.gitbook/assets/24.jpeg)

Photo by Huang Yingone on Unsplash![](.gitbook/assets/25.png)

![](.gitbook/assets/26.png)

warning: Unsupported Configuration: AR reference image "Mammoth" must have non-zero, positive width.

![](.gitbook/assets/27.png)

![](.gitbook/assets/28.png)

warning: Ambiguous Content: AR reference image “Pterosaur”: The histogram of the image is narrow or not well distributed. Recognition works better on images with wider, flatter histograms.

## 配置图像跟踪

```text
override func viewWillAppear(_ animated: Bool) {
    super.viewWillAppear(animated)
    // 设置参考图像
    guard let referenceImages = ARReferenceImage.referenceImages(inGroupNamed: "AR Resources", bundle: nil) else {
        fatalError("Missing expected asset catalog resources.")
    }
    // Create a session configuration
    let configuration = ARWorldTrackingConfiguration()
    configuration.detectionImages = referenceImages
    // Run the view's session
    sceneView.session.run(configuration)
}
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

{% embed url="https://www.raywenderlich.com/6957-building-a-museum-app-with-arkit-2\#toc-anchor-015" %}



{% embed url="https://www.jianshu.com/p/dfe33efbdc11" %}



{% embed url="https://developer.apple.com/documentation/arkit/detecting\_images\_in\_an\_ar\_experience" %}





