# 计算机图形学GAMES101几何



## 几何归类

![image-20220820162532964](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220820162532964.png)

- 几何分为:隐式和显示



### 隐式几何

![image-20220820162907487](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220820162907487.png)

- 隐式几何:点通过关系式来描述,而不是一个个明确的点

![image-20220820174025188](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220820174025188.png)

- 隐式表示的缺点:难以直接判断形状

![image-20220820174144884](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220820174144884.png)

- 隐式表示的优点:便于判断点与几何之间的位置关系,代入函数:
  - =0,在表面
  - <0,在物体内
  - ＞0,在物体外



### 显示几何

![image-20220820175036659](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220820175036659.png)

- 显示几何:直接给出点	或者	通过参数映射表示

![image-20220820175157488](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220820175157488.png)

- 如上式,就是一个参数uv对应到xyz的表示
- 显示表示的优点:便于直接观察得到几何形状

![image-20220820175507027](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220820175507027.png)

- 显示表示的缺点:难以判断点与几何的位置关系

==**根据需要选择表示方法**==



### 隐式的其他表示方法

![image-20220820175754300](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220820175754300.png)

- 数学代数表示,不直观

#### CSG

![image-20220820175935803](C:\Users\qingl\AppData\Roaming\Typora\typora-user-images\image-20220820175935803.png)

- 通过基本几何的布尔运算得到复杂几何

#### 距离函数

![image-20220820180226635](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220820180226635.png)

- 距离函数:空间中的点到几何体最短的距离
- 距离为正,在几何体外;距离为负,在几何体内
- 两个几何的距离函数进行融合后,再恢复成物体,得到融合后的物体

![image-20220820181211872](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220820181211872.png)

- 上例:阴影代表物体对屏幕的遮挡,求出输入A运动成输入B的中间状态
- 如果直接融合(blend,即相加求平均值),得到的结果并不是中间状态,而是黑灰白三块
- 正确做法:求出两种输入的边界函数,再进行blend(即找到距离函数=0的位置),最后恢复成最终结果,得到的就是中间值

![image-20220820200607429](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220820200607429.png)

- 水平集:距离函数不方便用解析的形式去表示,因此使用水平集(如图)去表示
- 水平集也可以在三维上
- 水平集的应用:水滴之间的融合效果



### 特殊的几何:分形

![image-20220820201224030](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220820201224030.png)

- 分形:自相似,自己的一部分和整体很像,类似于计算机中的递归



### 总结

![image-20220820202133676](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220820202133676.png)

- 隐式缺点
  - 不便于直接判断形状
  - 复杂的几何难以表述

- 隐式优点
  - 但是式子便于储存
  - 便于查询与点之间的位置关系
  - 便于求光线和表面的交
  - 适合描述拓扑结构
  - 几何弧度描述优秀
