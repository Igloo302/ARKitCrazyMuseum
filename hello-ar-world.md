# Hello AR World

## 官方示例初体验

首先打开 Xcode，创建一个新的Xcode项目，选择 Augmented Reality App模板，点击Next。这个模版中将包括一个最最基本的ARKit应用所需要的代码和资源。

![](.gitbook/assets/7.png)

ARKit框架中的视图对象继承于UIView，本身只包括相机追踪，并不包含创建虚拟世界的引擎，所以需要使用其他 3D/2D 引擎进行创建虚拟世界。iOS可以使用的引擎包括SceneKit、SpriteKit、Metal、OpenGl、Unity3D、Unreal Engine等。例如，开发一个3D场景的AR项目，你需要用到ARKit和SceneKit这两个库，ARKit用来捕捉现实场景参数，SceneKit则用来在AR视图中加载显示3D模型。更简单的理解，**在在图像中显示虚拟模型，需要靠其他的引擎来帮忙**。

回到我们的教程，填上必要的项目信息，可以看到，ARKit框架提供了四种AR技术，SceneKit是基于3D场景实现的增强现实，SpriktKit是基于2D场景实现的增强现实。在Content Technology 选择 “SceneKit”。

![](.gitbook/assets/image%20%281%29.png)

稍等片刻，Xcode会自动为我们生成一段简单的AR代码，在Xcode左上方区域1处选择连接的iOS设备，然后点击2处Run，应用即会进行安装。

![](.gitbook/assets/image%20%2810%29.png)

打开HelloARWorld应用并允许摄像头权限，就能从摄像头画面中看到一架飞机的 3D 模型，你可以全方位地查看它，同时，无论你如何移动手机，这架飞机都不会移动它的位置，

![HelloARWorld&#x4F8B;&#x7A0B;](.gitbook/assets/image%20%289%29.png)

我们先来看一看这个App的核心代码ViewController.swift。

sceneView属于ARSCNView，是用来加载AR的3D场景视图的，它显示在Main.storyboard的`Scene View`的界面上，这个界面上将显示摄像头捕捉到的画面和新增上去的虚拟模型。11-14行中，将事先准备好的3D飞机模型（art.scnassets 文件夹）设置为当前的场景（sceneView.scene）。

ARSession是一个ARKit体验的中枢，协调ARKit的流程，每个增强现实会话都都需要有一个 ARSession 实例。它负责控制摄像头、聚合所有来自设备的传感器数据等等以构建无缝体验。ARConfiguration是ARSession的配置，我们可以选择这个配置来启用不同的ARKit功能。`ARWorldTrackingConfiguration`可以进行设备6个自由度的追踪，包括roll、pitch、yaw 以及 X轴、Y轴、Z轴上的变换，同时使用后置摄像头检测追踪平面、人体、图像、物体的位置和方向，是比较常用的一种配置。在[这里](https://developer.apple.com/documentation/arkit/arconfiguration)可以查看所有的配置和功能。

21-24行中，创建了一个`ARWorldTrackingConfiguration`的配置，并启动ARsession，ARKit就开始工作了。    

```swift
    override func viewDidLoad() {
        super.viewDidLoad()
        // Set the view's delegate
        // 设置代理
        sceneView.delegate = self
        // Show statistics such as fps and timing information
        // 显示ARKit的统计数据
        sceneView.showsStatistics = true
        // Create a new scene
        // 使用ship.scn素材创建一个新的场景scene（scn格式文件是一个基于3D建模的文件，这里系统有一个默认的3D飞机）
        let scene = SCNScene(named: "art.scnassets/ship.scn")!
        // Set the scene to the view
        // 设置scene为SceneKit的当前场景
        sceneView.scene = scene
    }
    
    override func viewWillAppear(_ animated: Bool) {
        super.viewWillAppear(animated)
        // Create a session configuration
        // 使用ARWorldTrackingSessionConfiguration来充分利用所有的运动信息，并给出最佳的结果。
        let configuration = ARWorldTrackingConfiguration()
        // Run the view's session
        // run(_:options)开启ARKit进程，并开始捕捉视频画面。该方法将会让设备请求使用相机，如果用户拒绝该请求，那么ARKit将无法工作。
        sceneView.session.run(configuration)
    }
    
    override func viewWillDisappear(_ animated: Bool) {
        super.viewWillDisappear(animated)
        
        // Pause the view's session
        sceneView.session.pause()
    }
```

因为我们的目标是在空间中显示3D的物体，所以这里只展示了 SceneKit。同学们也可以尝试运行其他几个引擎的例程，对不同的引擎能有直观的认知。

SpriteKit是用来创建2D模型，2D模型会根据手机的远近放大缩小，但是由于模型是没有厚度的，所以不能像3D模型那样可以从侧面观察。对基于SpriteKit的2D场景感兴趣的同学可以可以参考这个[官方例程](https://developer.apple.com/documentation/arkit/arskview/providing_2d_virtual_content_with_spritekit)。

2019年WWDC上，同ARKit 3同时发布了全新的高级增强现实框架[RealityKit](https://developer.apple.com/documentation/realitykit)，这个全新的高级框架专门为增强现实量身定制，能够提供逼真的图像渲染、相机特效、动画、物理特效等等。它还提供 Swift API。借助原生 ARKit 整合、基于物理的超逼真渲染、变换和骨骼动画、空间音频和刚体物理，RealityKit 让您可以比以往更加快速轻松地进行 AR 开发。RealityKit这将会是这将会是此后进行ARKit应用开发的不二之选，但由于本教程成书时间问题，教程中主要还是使用SceneKit进行模型的模拟和渲染，当然，概念方法都是相通的，在掌握了SceneKit的使用之后再转而学习使用RealityKit也不迟。

## 从零创建项目

这回我们不使用Xcode的模板，直接从零开始创建项目，来更清晰地理解一个ARKit应用的运行机制。

### 新建项目

打开Xcode，点击File &gt; New &gt; Project…，选择Single View App，点击Next创建项目，命名为ARKitDemo。如下图操作所示：

![&#x521B;&#x5EFA;&#x9879;&#x76EE;](.gitbook/assets/image%20%285%29.png)

### 设置SceneKit View

打开Main.storyboard，点击右上方的添加按钮，将`ARKit SceneKit View`拖放到视图控制器上，并设置好约束，让`ARKit SceneKit View`充满全屏。

![&#x8BBE;&#x7F6E;SceneKit View](.gitbook/assets/image%20%2815%29.png)

### 连接IBOutlet

在ViewController.swift文件的顶部添加一个import语句来导入`ARKit`

```swift
import ARKit
```

![&#x8FDE;&#x63A5;IBOutlet](.gitbook/assets/image%20%2812%29.png)

然后按住control键并从ARKit SceneKit视图拖拽到ViewController.swift文件。命名为sceneView。

### 配置ARSCNView会话

我们需要应用通过后置摄像机镜头检测环境，在`ViewController`类中插入以下代码

```swift
    override func viewWillAppear(_ animated: Bool) {
        super.viewWillAppear(animated)
        let configuration = ARWorldTrackingConfiguration()
        sceneView.session.run(configuration)
    }
```

在该`viewWillAppear(_:)`方法中，我们初始化了名为`ARWorldTrackingConfiguration`的AR配置，用于运行世界跟踪。设置sceneView的AR会话以运行初始化的配置。AR会话管理视图内容的运动跟踪和相机图像处理。 

在`ViewController`中添加另一个方法：

```swift
    override func viewWillDisappear(_ animated: Bool) {
        super.viewWillDisappear(animated)
        sceneView.session.pause()
    }
```

`viewWillDisappear(_:)`方法主要处理停止追踪和视图内容的图像。

### 相机权限

iOS要求所有涉及隐私的功能都需要用户确认，AR所必须的摄像头也不例外。打开项目设置中的Info页，右键单击空白区域，然后选择`Add row`。将密钥设置为`Privacy - Camera Usage Description`_。_将`value`设置为对用户的提示语_。_

![&#x76F8;&#x673A;&#x6743;&#x9650;](.gitbook/assets/image%20%288%29.png)

至此，一个ARKit应用就创建好了，是不是很简单？

## 创建简单模型

运行刚刚创建的ARKitDemo，虽然App运行了，但是似乎只是打开了一个没有拍照按钮的相机应用，小飞机都没有出现，那么，我们如何在AR中添加自己想要的模型呢？我们先从简单的基本模型开始。

### 几何图形

事不宜迟，我们先创建3D几何图像。要向sceneView中添加几何图形，首先要创建SCNGeometry，它可以是简单的长方体、球体等形状，也可以是复杂的图形，将SCNGeometry包装成SCNNode，在ARKit中，SCNNode是所有3D内容的容器。SCNNode就能作为子节点添加到场景中并被渲染出来。在`ViewController`中添加以下代码

```swift
    func addBox(x:Float = 0.2, y: Float = 0, z:Float = -0.2){
        // 定义boxNode
        let box = SCNBox(width: 0.1, height: 0.1, length: 0.1, chamferRadius: 0)
        let boxNode = SCNNode()
        boxNode.geometry = box
        boxNode.position = SCNVector3(x,y,z)
        // 把boxNode添加到rootNode中
        sceneView.scene.rootNode.addChildNode(boxNode)
    }
```

ARKit 中的坐标单位为米，这个函数的功能就是创建一个边长为0.1米的立方体`SCNBox`，将其添加到名为`boxNode`的节点中，然后将其设置为`sceneView`的子节点。ARKit 和 SceneKit 的坐标系如下图所示，ARSession 开始时，摄像头 position 被初始化为 X=0, Y=0, Z=0，第6行将`boxNode`的位置定义为`(0.2, 0, -0.2)`则将box置于摄像头左前方。

![](.gitbook/assets/image%20%287%29.png)

在`viewDidLoad()`方法中调用`addbox()`后运行应用，就可以看见悬浮在空中的3D立方体，移动摄像头这个立方体的位置可以保持

在3D场景中，可以添加一些默认光照以便看清立方体的边缘，可以设置sceneView的autoenablesDefaultLighting值为true。

```swift
// 添加一些默认光照以便看清立方体的边缘
sceneView.autoenablesDefaultLighting = true
```

除了长方体，SCNGeometry还有多种类型，那如何知道其他的类型是什么，怎么创建呢？按住键盘上的option键，鼠标移到代码中的`SCNBox`，鼠标将变成问号。这就是Xcode的内联帮助，它可帮开发者快速学习类或代码片段的用法。在变量、类、或者方法名上执行Option + Left-click操作来获得更多细节信息。假使你点击了弹出视图底部的参考链接，那么就可以方便地跳转到Xcode提供的文档中。你还可以在变量、类或者方法名上执行Option+双击名称操作，从而更方便地跳转至文档。

![](.gitbook/assets/image.png)

查看文档可以看到，SCNBox继承于SCNGeometry，在后者详情页中就可以看到创建各种几何图形的方法了，试试创建一个球体吧。

### 显示文字

SCNText也继承于SCNGeometry，我们创建一个大小为0.1米的`Hello AR World`，置于摄像头前0.5米处，添加以下代码并运行

```swift
    func addText(x:Float = 0, y: Float = 0, z:Float = -0.5) {
        let text = SCNText(string: "Hello AR World", extrusionDepth: 0)
        text.font = UIFont(name: "Optima", size: 0.1)
        let textNode = SCNNode(geometry: text)
        textNode.position = SCNVector3(x,y,z)
        sceneView.scene.rootNode.addChildNode(textNode)
    }
```

代码的运行结果是不是不太正常？在画面中找不到文字。这是因为和立方体和球体不同，文字的坐标轴位置并不在模型的中心，为了解决这个问题，需要将坐标轴的位置移动以下，在第5行代码之前添加下列代码，就能将坐标轴移到textNode的底部中心了。

```swift
let (min, max) = textNode.boundingBox
textNode.pivot = SCNMatrix4MakeTranslation(min.x + 0.5 * (max.x - min.x), min.y, min.z + 0.5 * (max.z - min.z))
```

但是这个时候可以发现，字体的边缘似乎不太平滑。

![](.gitbook/assets/image%20%2814%29.png)

这个问题的[解决方案](https://medium.com/s23nyc-tech/arkit-planes-3d-text-and-hit-detection-1e10335493d)是，首先将SCNText的尺寸创建为1米，再SCNNode缩放为0.1。

```swift
    func addText(x:Float = 0, y: Float = 0, z:Float = -0.5) {
        let text = SCNText(string: "Hello AR World", extrusionDepth: 0)
        text.font = UIFont(name: "Optima", size: 1)
        let textNode = SCNNode(geometry: text)
        
        let fontSize = Float(0.1)
        textNode.scale = SCNVector3(fontSize, fontSize, fontSize)
        
        let (min, max) = textNode.boundingBox
        textNode.pivot = SCNMatrix4MakeTranslation(min.x + 0.5 * (max.x - min.x), min.y, min.z + 0.5 * (max.z - min.z))
        textNode.position = SCNVector3(x,y,z)
        
        sceneView.scene.rootNode.addChildNode(textNode)
    }
```

### 显示图片

![](.gitbook/assets/11.jpeg)

将图片添加到Assets.xcassets目录下

![](.gitbook/assets/12.png)

为了在AR空间中展示图片，我们可以创建一个几何体SCNPlane，在表面上添加这张fish图片的漫反射贴图。

首先创建一个material，将 material 的 contents 设为fish图片，然后将光线模型设为physicallyBased，然后创建一个几何体SCNPlane，将这个平面的材质修改为material，下一步要把它添加到场景中。代码如下：

```swift
func showImage(x:Float = 0, y: Float = 0, z:Float = -0.2){
    let material = SCNMaterial()
    // 将材质的漫反射贴图改为fish图片
    guard let img = UIImage(named: "fish") else {return}
    material.diffuse.contents = img
    material.lightingModel = .physicallyBased
    // 创建一个SCNPlane，并修改其材质
    let imgPlane = SCNPlane(width: 0.3, height: 0.2)
    imgPlane.materials = [material]
    let imgNode = SCNNode(geometry: imgPlane)
    imgNode.position = SCNVector3(x,y,z)
    sceneView.scene.rootNode.addChildNode(imgNode)
}
```

这样一张图片就凭空出现在空气之中了，因为光线是physicallyBased的，不仔细看真的可以以假乱真。

![](.gitbook/assets/14.png)

### 播放视频

![Video by Ruvim Miksanskiy from Pexels](.gitbook/assets/15.png)

我们准备一段视频road.mp4，将其添加到ARKitDemo的根目录，添加playVideo函数就能在AR中播放视频了，而且戴上耳机仔细听，你会发现声音还有随着距离、方向发生变化。

```swift
func playVideo(x:Float = 0, y: Float = 0, z:Float = -0.2){
    // 从资源包中抓取文件名为road.mp4的视频
    guard let videoURL = Bundle.main.url(forResource: "road", withExtension: "mp4") else {return}
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
    // SceneKit不使用UIViews，而是使用节点渲染场景。无法直接添加AVPlayer。相反，视频播放器可以用作节点的纹理或“材料”。这将视频帧映射到相关节点。
    let avMaterial = SCNMaterial()
    avMaterial.diffuse.contents = avPlayer
    // 创建SCNPlane，将材料修改为avPlayer
    let videoPlane = SCNPlane(width: 0.32, height: 0.18)
    videoPlane.materials = [avMaterial]
    // 创建将成为场景一部分的实际节点
    let videoNode = SCNNode(geometry: videoPlane)
    videoNode.position = SCNVector3(x,y,z)
    sceneView.scene.rootNode.addChildNode(videoNode)
}

```

### 

