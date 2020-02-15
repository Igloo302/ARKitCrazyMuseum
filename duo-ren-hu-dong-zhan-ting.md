# 多人互动展厅

## 多人互动

在WWDC 2018上，Apple展示了一款增强现实游戏SwiftShot，玩法主要是利用弹弓击倒对方积木来取胜。SwiftShot最多的亮点是支持2到6名玩家同时进行弹弓射击的游戏操作，也就是说，多个玩家可以共享同一个虚拟世界。多人互动的AR游戏可以让玩家聚集在同一个空间中玩乐，旁观者通过AR观看游戏带来了全新的视角和新的体验。

![SwiftShot](.gitbook/assets/image%20%2823%29.png)

在ARKit中，这种多人互动技术称为多用户\(Multiuser\)，这也是在WWDC 2018上发布的ARKit 2.0的重点特性。当多用户被启用时，在整个会话过程中，不同设备中的信息会通过网络协议进行传输共享，除了有关物理环境布局的信息外，还包括指示用户位置的锚点，放置的虚拟内容等。

所以说，多用户的实现包含两个部分：

1. ARKit 2新增的特性[ARWorldMap](https://developer.apple.com/documentation/arkit/arworldmap)中包含ARKit用于在真实世界空间中定位用户设备的所有空间映射信息，可以帮助其他的设备对AR内容“重新定位”。[ARWorldMap](https://developer.apple.com/documentation/arkit/arworldmap)同时包含所有现有的用自定义的锚点。我们也可以只传输ARAnchor对象来同步用户的操作。
2. 通过MultipeerConnectivity完成数据在设备间传输。

在本节中，我们将创建一个最简单的多用户AR应用，该应用参考了官方例程，读者可对照阅读。它将完成：

1. 在任意一台设备上添加小恐龙模型
2. 在某一台设备上发送世界地图
3. 另一台设备收到地图并将小恐龙模型显示在相同位置

## 配置AR会话

首先，我们根据之前的学习创建一个通过点击屏幕放置小恐龙模型的项目。它定义了一个`ARWorldTrackingConfiguration`的AR会话，当`UITapGestureRecognizer`检测到`ARSCNView`上的点击时，`didTap()`方法使用ARKit命中测试查找相交的特征点，在该点处显示3D小恐龙模型。为了方便理解，我们打开特征点显示`sceneView.debugOptions = [ARSCNDebugOptions.showFeaturePoints]`。

在HitTest显示小恐龙模型部分，需要创建一个名为dinosaur的`ARAnchor`并设置位置，将其添加到会话中。

```swift
let tapPoint = recognizer.location(in: sceneView)
let result = sceneView.hitTest(tapPoint, types: .featurePoint)
if  let  hitResult =  result.first {
    // 放置一个ARanchor
    let anchor = ARAnchor(name: "dinosaur", transform: hitResult.worldTransform)
    sceneView.session.add(anchor: anchor)
```

然后响应`ARSCNViewDelegate`回调`renderer(_:didAdd:for:)`，为每个dinosaur锚点提供SceneKit内容，ARKit将为您添加场景内容并将其放置在场景中。

```swift
    func renderer(_ renderer: SCNSceneRenderer, didAdd node: SCNNode, for anchor: ARAnchor) {
        if let name = anchor.name, name.hasPrefix("dinosaur") {
            node.addChildNode(loadDinosaurModel())
        }
    }
```

这种方式和此前采用的方式（创建一个`SCNNode`，设置其`simdTransform`，并将其添加为场景的子级`rootNode`）相比的优势在于，当世界跟踪失败，恢复中断的会话，或重新开始会话的时候，可以让ARKit删除锚点，而不会把此前的Node保留在错误的位置。

```swift
sceneView.session.run(configuration, options: [.resetTracking, .removeExistingAnchors])
```

同时，采用ARAnchor可以让数据的传输和并在另一台设备上重新定位变更加简单。

![&#x914D;&#x7F6E;AR&#x4F1A;&#x8BDD;&#x548C;&#x653E;&#x7F6E;&#x865A;&#x62DF;&#x5C0F;&#x6050;&#x9F99;](.gitbook/assets/img_fc337861bdd9-1.jpeg)

## AR World Map

ARWorldMap对象使用用户周围的特征点信息，ARKit本身不包含经度/纬度信息，但是可能会包含个人敏感信息。在一个AR应用中，我们可以加载准备好的地图数据：

```text
// Unarchive data to ARWorldMap
let worldMap = try NSKeyedUnarchiver.unarchivedObject(ofClass: ARWorldMap.self, from: data)

// Create tracking configuration
let configuration = ARWorldTrackingConfiguration()
configuration.initialWorldMap = worldMap

// Run session
sceneView.session.run(configuration, options: [.resetTracking, .removeExistingAnchors])
```

当地图被传输到其他用户的设备上时，还需要



## Multipeer Connectivity网络

MultipeerConnectivity可以帮助我们完成数据在设备间的传输。



除了本节从介绍的主从多用户策略，iOS 13.0以上还支持对等多用户策略，读者可以查阅这份[文档](https://developer.apple.com/documentation/arkit/creating_a_collaborative_session)学习了解。

{% embed url="https://developer.apple.com/documentation/arkit/swiftshot\_creating\_a\_game\_for\_augmented\_reality" %}

{% embed url="https://developer.apple.com/documentation/arkit/creating\_a\_multiuser\_ar\_experience" %}



