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

![1](https://dm2305files.storage.live.com/y4mD4JJ3hd_tVn4HnbTVAcRx1Q8YNd-2Dh9RlnWPnXlbSWk5k6PFucNpFwh5IbUaBmDLQqU-LH5kClJOaNOJeArtHwXM3JH7a3rvtjbk0oF1T6AARgbUizwOMwPqe65j2m-AyUNitz_ggD3BldzZh1kRL6RXBTeNo_Hk9HA5LYi28GwNIE0eAxBNoW6fGHMMHQz?width=3918&height=531&cropmode=none)

转换积分域后（转化积分域时，舍弃其他物体反射的能量）：

![2](https://dm2305files.storage.live.com/y4myZhxRL6vYHCCN5dm5nSVArXK1qUGAaKTbXP9fRGS5Ey_90b4ygYOUlPcP-t7BSIBX26dyoy6H4bKnI3MRl3NFTn1YQjpugZZj93josxCH1c_848bXlvefp5S4R7Vt_PCXu_WKqam_i5jX-MXWTXgjI3TYDmy0U0FNoGESkA6Ez2wuYXdmdtbTlBiRCTE0bEm?width=3930&height=509&cropmode=none)

其中dA为发光物体面积微元，对上式蒙特卡洛方法有(按均匀采样处理)：

![3](https://dm2305files.storage.live.com/y4mWt_0-QXGR26cHQ8aPlfCiL_0DRkyQFs4bxCSfJ06CDCta-WjSCnN0BJPheSnj7ezDWiJz_7gnnybIJYE9wMH0T_8dwYZt4lDUkMPsc2mWe102n2foCsvvEnpRgpPu6amfLNbBxDlq5flIjJLoGpQ_sFLYXvRJrlXziMpJ51uYGcate49aYlK8tDKox3GGxiD?width=3930&height=491&cropmode=none)

对于被舍弃的其他物体所反射的能量，其表达式为：

![4](https://dm2305files.storage.live.com/y4m9vAIkskTm21i2qH0ogEbWSGnbg6Zi4VLOpg3z00og9l66sYP5EeIClOcid-Mk4QyLGrvdltZyw0n4_dsGuNwhCU2HosHt0DEoTdDHSP6zaz5i3dq_akt3lZmAcEhqz4El0O2QCG5L7VfwzC8CKXGxyVau91r2BvmOAK0fqvcuaUWWzxmNBnoCneuIluC-u_M?width=3920&height=537&cropmode=none)

在此引入轮盘赌，假设每次能量弹射的概率为p，则有：

![5](https://dm2305files.storage.live.com/y4mUJqAXxwAoCaXk9-i_CT_LPdPAQHOFUevCUA6Fkpk0Vs9CL7SIWjE-jkWFiivCR4SrxCAa958GjB28jO2tYGYpBxeTU4mIaif8BKulxmTRvcdYxdXvCykARbjvbN8NRqpIohc3K-vVVbfR3ycvhALbasKiwUp-0R6TfRyZopr_ZvaqWmZUWn5vGEKOz0Y48g2?width=3933&height=569&cropmode=none)

故渲染方程有：

![6](https://dm2305files.storage.live.com/y4m7PjjqQ7iy3SSvu9HyHHvG5SHkVthAppky9q78Re0XKfZyNZ6fwC8vumILB-3Dv_WiWhf0zGYP8TyyWyZvzaYmyB4zV-0pDROZr7ucXKTHC05dXzOJS5bxQirXqOP9mE0f4_qb8Ju1CkGBC0knusHjcJVxhTqERGhOFtxjNdOHyw3aeAWdG9JBOKldJygX4B6?width=3906&height=367&cropmode=none)

## VS 相关

&#8194;&#8194;本项目有使用OpenMP，运行时需将模式改为Release。

## 运行效果

![Path tracing](https://dm2305files.storage.live.com/y4mu2bVD7-jh-3nyLn9DzWGQ06wUBwgu2inWwCo-q4OURCcj5zja7arir5pZ6wP-0cvMPrqL8TrO-0COo81SzvXOWo4Z0W2yy1Q8dK7Vi23cbZNeQe5uszB276K4e5bk3-J9sCuXApesiTQOXfylDpL3p5Vrw9BNiV1lhVeKLFOySq0QT3CyQevlassrjnIU1ID?width=660&height=653&cropmode=none)
