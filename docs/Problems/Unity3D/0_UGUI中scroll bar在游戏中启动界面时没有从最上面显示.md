# Unity UGUI中scroll bar在游戏中启动界面时没有从最上面显示



问题描述:

​	在Unity中显示篇幅较长的text的时候时常要使用scroll bar控件,然而即使将Inspector中scroll bar的value属性设置为1以后,在游戏中显示仍旧不是从最上方开始的,而是从文章的中间部分开始,非常影响视觉效果

解决前效果:

![image-20220724121532524](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220724121532524.png)

解决办法:

![image-20220724121952704](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220724121952704.png)

​	第一步:将scroll bar所控制的==**text控件的pivot y设置为1**==

![image-20220724122017793](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220724122017793.png)

​	第二步:将text控件的best fit属性勾选上,并将文字大小设置成合适的大小

![image-20220724122055480](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220724122055480.png)

解决后效果:

![image-20220724122117678](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220724122117678.png)