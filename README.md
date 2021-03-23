# 路径追踪渲染器

## 项目介绍

&#8194;&#8194;本项目为双向路径追踪方法的光线追踪渲染器，其中以实现BVH加速结构。

## 部分模块说明

### BVH加速结构

&#8194;&#8194;在本项目中，每次添加模型时会调用一次` BVHBuildNode* BVHAccel::recursiveBuild(std::vector<Object*> objects)`，并在最终组建场景时会再次调用一次` BVHBuildNode* BVHAccel::recursiveBuild(std::vector<Object*> objects)`。

### 视平面坐标变换

&#8194;&#8194;为保证视平面像素免被挤压，需为X轴坐标进行一定缩放。又视平面的纵坐标朝向与三维空间中纵坐标朝向相反，故需对其进行矫正。[详情参考](https://www.scratchapixel.com/lessons/3d-basic-rendering/ray-tracing-generating-camera-rays/generating-camera-rays)

```
 float x = (2 * (i + 0.5) / (float)scene.width - 1) * imageAspectRatio * scale;
 float y = (1 - 2 * (j + 0.5) / (float)scene.height) * scale;
```
### 路径追踪

由渲染方程舍弃自发光项后，形式如下：
$$
L_{o}(p,\omega _{o})= \int_ {\Omega^{+} } L_{i}(p_{i},\omega _{i})f_{i}(p,\omega_{i},\omega _{o})(n\cdot \omega_{i})d\omega_{i} 
$$
转换积分域后（转化积分域时，舍弃其他物体反射的能量）：
$$
L_{o1}(p,\omega _{o})=\int_{A} L_{i}(p_{i},\omega _{i})f_{i}(p,\omega_{i},\omega _{o})(n\cdot \omega_{i})\frac{(n^{'} \cdot\omega_{i})}{l^{2}}dA 
$$
其中dA为发光物体面积微元，对上式蒙特卡洛方法有(按均匀采样处理)：
$$
L_{o1}(p,\omega_{i}) =\sum L_{i}(p,\omega_{i})f_{i}(p,\omega _{i},\omega _{o}) (n\cdot \omega_{i})\frac{(n^{'} \cdot\omega_{i})}{l^{2}}A
$$
对于被舍弃的其他物体所反射的能量，其表达式为：
$$
L_{o2}(p,\omega _{o})= \int_ {\Omega^{+}/A } L_{i}(p_{i},\omega _{i})f_{i}(p,\omega_{i},\omega _{o})(n\cdot \omega_{i})d\omega_{i} 
$$
在此引入轮盘赌，假设每次能量弹射的概率为p，则有：
$$
L_{o2} =\sum L_{i}(p,\omega_{i})f_{i}(p,\omega _{i},\omega _{o}) (n\cdot \omega_{i})\frac{(n^{'} \cdot\omega_{i})}{l^{2}\cdot pdf(p)}\rho 
$$
故渲染方程有：
$$
L_{o}(p,\omega_{i})=L_{o1}(p,\omega_{i})+L_{o2}(p,\omega_{i})
$$
## VS 相关

&#8194;&#8194;本项目有使用OpenMP，运行时需将模式改为Release。

## 运行效果

![Path tracing](https://dm2305files.storage.live.com/y4mu2bVD7-jh-3nyLn9DzWGQ06wUBwgu2inWwCo-q4OURCcj5zja7arir5pZ6wP-0cvMPrqL8TrO-0COo81SzvXOWo4Z0W2yy1Q8dK7Vi23cbZNeQe5uszB276K4e5bk3-J9sCuXApesiTQOXfylDpL3p5Vrw9BNiV1lhVeKLFOySq0QT3CyQevlassrjnIU1ID?width=660&height=653&cropmode=none)