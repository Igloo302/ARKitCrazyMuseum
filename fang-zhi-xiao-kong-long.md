# 放置小恐龙

根据ARKit在真实世界的实物表面上侦测到的特征点，放置立方体。首先，我们先要有一个hit test，很像是我们第一次测试，除了这个，我们清楚定义.featurePoint属于types参数。types参数要求hit test经由AR单元的相机图像来搜寻真实世界的实体物或是表面。它内含许多类型，但本教学目前只针对特征点。

经由特征点的hit test后，我们可以安全地移除第一次hit test的结果，这观念很重要，因为不会一直都有特征点，ARKit并不会一直侦测真实世界的实体物与表面。

如果第一次hit test能成功移除，然后我们就将转换矩阵类型matrix\_float4x4到float3，因为我们之前已增加了一个extension来完成此功能。然后，我们在一特征点上输入x, y和z来加入一个立方体。

https://www.jianshu.com/p/a3d1c48f0d43

## 命中测试

## 应用物理学

https://www.jianshu.com/p/641af448830c

## 渲染和光照估计

