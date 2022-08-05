# Unity导入fbx模型后,运行时物体的旋转和位置自动改变的问题解决方法



## 问题描述

​	今天在开发游戏的过程中,导入了一个网上的船的FBX素材,想用该模型替换原本设计好功能的用cube作为模型的船

​	模型导入进unity后默认是竖过来的

​	于是我将旋转和位置调整好后,再将各个组件以及脚本挂在好.

​	可是运行的时候却发现,**船会发生鬼畜并竖着前进**

![image-20220713174544175](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220713174544175.png)



## 解决过程

​	首先通过隐藏人物模型排除由于两个物体之间碰撞所发生的问题

​	其次修改脚本的移动速度为0后发现模型仍旧会竖起来,从而想到可能是模型本身的问题

​	于是上网搜索可能的解决方案,找到了unity的一篇官方文档:

​	https://docs.unity3d.com/cn/2019.4/Manual/HOWTO-FixZAxisIsUp.html



## 原因分析

​	发生问题的原因是:==**maya中默认朝上的轴为z轴,而unity中默认朝上的轴为y轴**==,导致maya导入unity后,模型的transform在reset以后船的rotation是错误的,最终导致**脚本内与改变transform.rotation相关的代码会使得你之前在inspector面板直接调整的transform.rotation的参数归零或发生不符合预期的情况**



## 解决方法

### 	法一(简便,需要maya):

​		在maya导出时设置轴向为y轴朝上

### 	法二(相对复杂,参考unity文档):

​		1.创建一个空物体作为该模型的父物体

​		2.将所有的组件都放到空物体上(包括脚本)

​		3.修改子物体模型的transform值到想要的状态,并保持空物体的transform参数为默认状态