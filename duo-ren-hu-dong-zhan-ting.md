# 多人互动展厅

## 多人互动

在WWDC 2018上，Apple展示了一款增强现实游戏SwiftShot，玩法主要是利用弹弓击倒对方积木来取胜。SwiftShot最多的亮点是支持2到6名玩家同时进行弹弓射击的游戏操作，也就是说，多个玩家可以共享同一个虚拟世界。多人互动的AR游戏可以让玩家聚集在同一个空间中玩乐，旁观者通过AR观看游戏带来了全新的视角和新的体验。

![SwiftShot](.gitbook/assets/image%20%2823%29.png)

在ARKit中，这种多人互动技术称为多用户\(Multiuser\)，这也是在WWDC 2018上发布的ARKit 2.0的重点特性。当多用户被启用时，在整个会话过程中，不同设备中的信息会通过网络协议进行传输共享，除了有关物理环境布局的信息外，还包括指示用户位置的锚点，放置的虚拟内容等。

所以说，多用户的实现包含两个部分：

1. ARKit 2新增的特性[ARWorldMap](https://developer.apple.com/documentation/arkit/arworldmap)中包含所有平面锚点和自定义锚点，可以帮助我们获取带有空间信息的数据。
2. 通过MultipeerConnectivity完成数据在设备间传输。

在本节中，我们将创建一个最简单的多用户AR应用，它将完成：

1. 在任意一台设备上添加小恐龙模型
2. 在某一台设备上发送世界地图
3. 另一台设备收到地图并将小恐龙模型显示在相同位置

## 运行AR会话并放置AR内容

首先，我们根据之前的学习创建一个通过点击屏幕放置小恐龙模型的项目。

![](.gitbook/assets/img_fc337861bdd9-1.jpeg)



{% embed url="https://developer.apple.com/documentation/arkit/swiftshot\_creating\_a\_game\_for\_augmented\_reality" %}



