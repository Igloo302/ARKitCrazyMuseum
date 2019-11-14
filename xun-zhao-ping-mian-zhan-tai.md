# 寻找平面展台

世界跟踪是ARKit的重要能力，也是任何虚拟现实体验的基本要求。关于世界跟踪，苹果的官方API是这样解释的

“World tracking provides 6 degrees of freedom tracking of the device. By finding feature points in the scene, world tracking enables performing hit-tests against the frame. Tracking can no longer be resumed once the session is paused.”

Apple’s Documentation

也就是说，世界跟踪指的是在真实空间和虚拟空间之间创建一个对应关系，让App认识它处于的世界环境。通过世界跟踪技术平面检测（参见planeDetection），App能够检测摄像机画面中的水平面、竖直面，并获取其位置和大小;使用命中测试方法（参见ARHitTestResult 类），可以和检测到的平面或场景中放置的虚拟内容进行交互。（https://www.jianshu.com/p/1407f8bbba89）

我们在之前的课程中已经学会了在AR世界中添加模型和自己想要的虚拟内容，为了让我们的小恐龙模型能够站在平面上，本节中我们讲学习如何检测平面，并且让平面可视化。

## 配置和运行AR会话

首先，在viewWillAppear\(\_:\)方法中，通过将ARWorldTrackingConfiguration的planeDetection属性设置为.horizontal，这告诉ARKit去查找任何水平面。一旦ARKit检测到一个水平面，该水平面将被添加到sceneView的session中。

```text
let configuration = ARWorldTrackingConfiguration()
configuration.planeDetection = [.horizontal, .vertical]
sceneView.session.run(configuration)
```

为了检测水平面，必须采用ARSCNViewDelegate协议。将ViewController类修改为

```swift
class ViewController: UIViewController, ARSCNViewDelegate {
    // 省略
｝

```

同时在viewWillAppear\(\_:\)中设置代理

```swift
// 设置代理
sceneView.delegate = self
```

当启用平面检测时，ARKit会为每个检测到的平面添加和更新锚点，默认情况下，ARSCNView类为每个锚点的SceneKit场景添加一个 SCNNode对象，代理就会调用renderer\(\_:didAdd:for:\)方法。

这里还可以添加ASRCNView的一个额外功能，使用该行代码可以方便我们再在调试的过程中查看特征点和世界坐标原点。

```swift
#if DEBUG
sceneView.debugOptions = [ARSCNDebugOptions.showWorldOrigin, ARSCNDebugOptions.showFeaturePoints]
#endif

```



目前，viewWillAppear\(\_:\)方法应该是这样的：

```swift
override func viewWillAppear(_ animated: Bool) {
    super.viewWillAppear(animated)
    let configuration = ARWorldTrackingConfiguration()
    configuration.planeDetection = [.horizontal, .vertical]
    sceneView.session.run(configuration)
    sceneView.delegate = self
    // 添加一些默认光照以便看清立方体的边缘
    sceneView.autoenablesDefaultLighting = true
    #if DEBUG
    sceneView.debugOptions = [ARSCNDebugOptions.showWorldOrigin, ARSCNDebugOptions.showFeaturePoints]
    #endif
}

```

可视化平面

现在，我们需要修改ARSCNViewDelegate 的回调方法renderer\(\_:didAdd:for:\)，每次 ARKit 报告新的 ARPlaneAnchor时，可以在摄像头画面中看到这个平面。

相关代码如下所示：

```swift
func renderer(_ renderer: SCNSceneRenderer, didAdd node: SCNNode, for anchor: ARAnchor) {
    // Place content only for anchors found by plane detection.
    guard let planeAnchor = anchor as? ARPlaneAnchor else { return }
    // Create a SceneKit plane to visualize the plane anchor using its position and extent.
    let plane = SCNPlane(width: CGFloat(planeAnchor.extent.x), height: CGFloat(planeAnchor.extent.z))
    let planeNode = SCNNode(geometry: plane)
    planeNode.simdPosition = float3(planeAnchor.center.x, 0, planeAnchor.center.z)
    /*
     `SCNPlane` is vertically oriented in its local coordinate space, so
     rotate the plane to match the horizontal orientation of `ARPlaneAnchor`.
     */
    planeNode.eulerAngles.x = -.pi / 2
    // Make the plane visualization semitransparent to clearly show real-world placement.
    planeNode.opacity = 0.25
    /*
     Add the plane visualization to the ARKit-managed node so that it tracks
     changes in the plane anchor as plane estimation continues.
     */
    node.addChildNode(planeNode)
}

```

更新平面

![](.gitbook/assets/23.png)

编译运行App，ARKit就可以识别不同的平面，并且用半透明的平面显示出来了，但是平面不会正确地变大。

随着ARKit接收到关于我们的环境的额外信息，我们需要扩展之前检测到的水平面，以利用更大的表面，或者用新的环境信息更准确地表示，实现方法renderer\(\_:didUpdate:for:\):

代码如下：

```swift
func renderer(_ renderer: SCNSceneRenderer, didUpdate node: SCNNode, for anchor: ARAnchor) {
    // Update content only for plane anchors and nodes matching the setup created in `renderer(_:didAdd:for:)`.
    guard let planeAnchor = anchor as? ARPlaneAnchor,
        let planeNode = node.childNodes.first,
        let plane = planeNode.geometry as? SCNPlane
        else { return }
    // Plane estimation may shift the center of a plane relative to its anchor's transform.
    planeNode.simdPosition = float3(planeAnchor.center.x, 0, planeAnchor.center.z)
    /*
     Plane estimation may extend the size of the plane, or combine previously detected
     planes into a larger one. In the latter case, `ARSCNView` automatically deletes the
     corresponding node for one plane, then calls this method to update the size of
     the remaining plane.
     */
    plane.width = CGFloat(planeAnchor.extent.x)
    plane.height = CGFloat(planeAnchor.extent.z)
}

```

每次更新SceneKit节点的属性以匹配其对应的锚（anchor）时，都会调用此方法。node参数为我们提供了锚的更新位置。锚参数给出了更新后的宽度和高度。有了这两个参数，我们可以更新之前实现的SCNPlane，用更新后的宽度和高度反映新的位置。

为了让视觉效果更好看，还可以给SCNPlane添加上纹理。找一张喜欢的纹理图片，比如找一张草地的纹理图grass.jpg，将其添加到Project的Assets.xcassets中，在renderer\(\_:didAdd:for:\)方法修改plane的材质即可：

```swift
let material = SCNMaterial()
let img = UIImage(named: "grass")
material.diffuse.contents = img
material.lightingModel = .physicallyBased
plane.materials = [material]

```

[https://www.jianshu.com/p/7abbb3efdbcb](https://www.jianshu.com/p/7abbb3efdbcb)[https://developer.apple.com/documentation/arkit/tracking\_and\_visualizing\_planes](https://developer.apple.com/documentation/arkit/tracking_and_visualizing_planes)

https://www.jianshu.com/p/0ec5bd03c269

https://mp.weixin.qq.com/s/ib59tBzR3GjN48q\_Dph\_\_Q

