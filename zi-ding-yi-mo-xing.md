# 自定义模型

## 获取模型

上一节中，我们学习了在AR中展示简单的模型，本节中我们将指导大家添加更为复杂的模型。那么，这种模型从何而来呢？一般来说有两种方式

1. 通过建模软件进行建模
2. 网上下载模型

因为3D模型涉及到的格式种类很多，本教程中仅以最常见的几种情况为例，如果在实际的项目中遇到特殊的格式，可能需要使用Blender等软件进行尺寸调整、坐标变换、格式转换。

### 使用建模软件制作模型

用建模软件Rhinoceros新建一个带贴图的图形，注意无论在Rhinoceros中设置什么单位，导出后都将被视作以米为单位。导出COLLADA\(dae\)格式，并勾选Save textures选项，得到模型的dae文件和png格式的贴图文件。

![](.gitbook/assets/image%20%283%29.png)

### 网络上下载模型

在Free3D等网站上可以下载到用户上传的模型，大家可以选择自己喜欢的模型，格式为.obj。

## 显示模型

### dae格式模型

在ARKitDemo的根目录下新建一个以scnassets为后缀的文件夹，取名art.scnassets，将先前得到的dea文件和存有贴图的文件夹放入art.scnassets目录，就能在Xcode中查看这个模型了，贴图能被正确加载，右侧的属性栏中可以查看模型的大小、贴图等。

![](.gitbook/assets/image%20%286%29.png)

代码部分，在`ViewController`类中新建方法，并在`viewDidLoad()`调用。

```swift
    func addModel(x:Float = 0, y: Float = 0, z:Float = -0.5){
        guard let url = Bundle.main.url(forResource: "model", withExtension: "dae", subdirectory: "art.scnassets") else {
            print("no file")
            return}
        if let modelNode = SCNReferenceNode(url: url) {
            modelNode.load()
            modelNode.position = SCNVector3(x,y,z)
            sceneView.scene.rootNode.addChildNode(modelNode)
        }else{
            print("fail")
        }
    }
```

实现思路是实例化一个`SCNReferenceNode`，用来加载dae文件中的内容。运行后效果

![](.gitbook/assets/image%20%282%29.png)

除了`SCNReferenceNode`方法以外，还有一种方法可以将dae模型导入到ARKit。通过新建一个SCNNode对象，用来附着模型子节点。当模型中存在多个节点时，添加for语句，遍历所有子节点，加入到一个SCNNode对象中，具体实现代码如下：

```swift
func addModelTwo(x:Float = -0.2, y: Float = 0, z:Float = -0.2){
    guard let modelScene = SCNScene(named: "art.scnassets/model.dae") else {return}
    let modelNode = SCNNode()
    let modelSceneChildNodes = carScene.rootNode.childNodes
    // 遍历modelScene的所有子节点，添加到modelNode中
    for childNode in modelSceneChildNodes {
        carNode.addChildNode(childNode)
    }
    modelNode.position = SCNVector3(x,y,z)
    sceneView.scene.rootNode.addChildNode(modelNode)
}
```

### obj格式模型

ARKit对dae和scn格式的模型支持效果较好，obj格式的模型可以通过Xcode内置的工具转换成scn格式，导入文件的过程与前面的dae模型类似，将obj格式的恐龙模型和贴图导入Xcode。

选中obj模型，选择Editor选项卡下的Convert to SceneKit file format，就可以将文件转换成scn格式。点击Chirostenotes.scn文件，检查模型尺寸和位置等信息，值得注意的是，此处的模型单位是m，可以通过调节scale来调整模型的显示比例，并为模型附着另一个文件中的材质。

![](.gitbook/assets/20.png)

![](.gitbook/assets/21.png)

scn格式的模型可以简单地示例化SCNScene创建，并设置为当前的场景

```swift
// Create a new scene
let scene = SCNScene(named: "art.scnassets/Chirostenotes.scn")!
// Set the scene to the view
sceneView.scene = scene
```

但是这样缺点是无法对模型进行控制，好在`SCNReferenceNode`方法同样适用于scn格式的模型，只需要将`addModel()`稍作改动就可以完成scn文件的加载。

```swift
    func addDinosaur(x:Float = -0.2, y: Float = 0, z:Float = -0.2){
        guard let url = Bundle.main.url(forResource: "Chirostenotes", withExtension: "scn", subdirectory: "art.scnassets") else {
            print("no file")
            return}
        if let dinosaurNode = SCNReferenceNode(url: url) {
            dinosaurNode.load()
            dinosaurNode.position = SCNVector3(x,y,z)
            dinosaurNode.eulerAngles.y = Float(arc4random() % 200) / 100.0 * .pi
            sceneView.scene.rootNode.addChildNode(dinosaurNode)
        }else{
            print("scn fail")
        }
    }
```

另外，遍历scn文件中的Node的方法也同样适用，可以根据项目实际需要自由选择。

最终效果如图，逼真的小恐龙模型就可以显示出来了。

![](.gitbook/assets/22.jpeg)

