# 多人互动展厅

## 多人互动

在WWDC 2018上，Apple展示了一款增强现实游戏SwiftShot，玩法主要是利用弹弓击倒对方积木来取胜。SwiftShot最多的亮点是支持2到6名玩家同时进行弹弓射击的游戏操作，也就是说，多个玩家可以共享同一个虚拟世界。多人互动的AR游戏可以让玩家聚集在同一个空间中玩乐，旁观者通过AR观看游戏带来了全新的视角和新的体验。

![SwiftShot](.gitbook/assets/image%20%2823%29.png)

在ARKit中，这种技术称为多用户\(Multiuser\)，当多用户被启用时，在整个会话过程中，不同设备中的信息会通过网络协议进行传输共享，除了有关物理环境布局的信息外，还包括指示用户位置的锚点，放置的虚拟内容等。ARKit 2新增的特性[ARWorldMap](https://developer.apple.com/documentation/arkit/arworldmap)中包含所有平面锚点和自定义锚点，我们需要利用它实现多用户的AR体验。



## 场景持续



{% embed url="https://developer.apple.com/documentation/arkit/swiftshot\_creating\_a\_game\_for\_augmented\_reality" %}



