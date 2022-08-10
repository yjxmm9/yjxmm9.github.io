GAMES101第一部分:光栅化





## 课程简介:四大板块

- 光栅化:三维空间的几何形体显示在屏幕上

- 曲线和曲面

- 光线追踪

- 动画/模拟/仿真



不学的内容:图形API(OpenGL/DX/Vulcan),理解课程内容后对图形API更好上手



## 向量与线性代数

图形学涉及的学科:

- 线性代数,微积分,统计

- 光学,力学

- 信号处理,数值分析



### 向量概念

![image-20220712203958787](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220712203958787.png)

![image-20220712204144569](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220712204144569.png)

- ==注意==:**模用两条杠表示,向量上箭头变成三角的为单位向量**

- 单位向量只表示方向,不关心长度

### 向量相加

![image-20220712204302867](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220712204302867.png)

- 笛卡尔坐标系表示向量:可以直接用x和y表示一个向量,**图形学上通常写为列向量**

![image-20220712204551852](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220712204551852.png)

- 使用笛卡尔坐标系的原因之一:便于计算长度



### 向量点乘和叉乘

#### 点乘

![image-20220712204908671](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220712204908671.png)

- 点乘的结果是一个数字

- **点乘图形学的应用:可以快速得到两个向量的夹角,尤其是两个向量都是单位向量时**

- 点乘的运算性质:

![image-20220712205313834](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220712205313834.png)

- 笛卡尔坐标系中的点乘:

![image-20220712205452960](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220712205452960.png)

- 点乘和投影:

![image-20220712205639338](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220712205639338.png)

![image-20220712205900761](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220712205900761.png)

![image-20220712210056691](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220712210056691.png)

- 投影和点乘的应用:
  - 分解向量;
  - 判断两个方向是否接近(点乘的结果是否接近1);
  - 判断方向相同还是相反(点乘的正,负,零)



#### 叉乘

![image-20220712210807140](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220712210807140.png)

- 结果是一个和原来的两个向量都**垂直的向量**,换言之结果不在这两个所在面

- 叉乘的方向 ----- ==**右手螺旋定则**==:对于a×b,四指的转向为a转向b,大拇指的方向为叉乘结果的方向

- 向量叉积不满足交换律:a×b=-b×a

![image-20220712211720591](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220712211720591.png)

- 若一个坐标系x×y=z,则称为**右手坐标系**

- ==**这门课都是右手坐标系**==

- 向量叉乘的笛卡尔坐标计算:

![image-20220712212147785](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220712212147785.png)

- 向量叉乘也可以写成矩阵形式

- **图形学中叉乘的应用:判断左右,内外**

![image-20220712212432337](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220712212432337.png)

- 左侧图:一个xy平面,通过a×b与z正方向相同得到,b在a的左侧

- 右侧图:分别用AB×AP,BC×AP,CA×AP,得到AP永远在三个边的左侧,所以P在三角形内部(若ABC顺时针同理)

![image-20220712214505736](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220712214505736.png)

- 直角坐标系便于分解向量



### 矩阵

![image-20220712214631494](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220712214631494.png)

- 矩阵的乘积:

![image-20220712214822573](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220712214822573.png)

- 矩阵乘积的性质:**没有交换律**,有结合律和分配律

![image-20220712214959378](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220712214959378.png)

- 矩阵向量相乘

![image-20220712215126641](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220712215126641.png)

- 矩阵转置:

![image-20220712215211996](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220712215211996.png)

- 单位矩阵:

![image-20220712215304245](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220712215304245.png)

- 单位矩阵用于定义矩阵的逆



- 点乘和叉乘的矩阵形式:

![image-20220712215502789](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220712215502789.png)

- ==注意==:此处A^*^是一种特殊的矩阵









## 二维变换(使用==矩阵==表示)

### 缩放

- 均匀的:

![image-20220713205057205](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220713205057205.png)

- 不均匀的:

![image-20220713205114338](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220713205114338.png)

### 反射

![image-20220713205216490](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220713205216490.png)

### 切变

![image-20220713205245027](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220713205245027.png)

- 分析过程:切变变换中,y不变,x变为x+ay	[通过原来(0,1)变成(a,1)以及原来(0,1/2)变成(a/,1/2)]

### 旋转

- 默认绕原点(0,0)旋转,旋转方向为逆时针方向

![image-20220713210320672](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220713210320672.png)

- 推导过程:如上图的(1,0)点和(0,1)点的变换前后矩阵相乘分析

![image-20220713210517604](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220713210517604.png)

- 此处以(1,0)为例



- **旋转-θ角度:相当于旋转θ角度的矩阵的转置**

![image-20220715092812290](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220715092812290.png)

- 又由于旋转θ和-θ正好是逆操作,因此**旋转-θ的矩阵也等于旋转θ的矩阵的逆**(见逆变换)

- 所以:==旋转矩阵的转置等于旋转矩阵的逆==,数学上称这类矩阵为==正交矩阵==





### 线性变换的概念

- 以上所有变换(**==能够表示为下式矩阵相乘形式的变换==**)都被称为线性变换

- 矩阵必须和向量在同一维度(即2×2的矩阵不能操作三维向量)

![image-20220713210714358](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220713210714358.png)

### 平移

![image-20220713214233872](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220713214233872.png)

![image-20220713214257399](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220713214257399.png)

- **结论:平移不能用矩阵的形式(线性变换)来表示,必须在后面加上一个向量**

- ==注意==:**由该式可得:齐次坐标的变换是先进行线性变换再进行平移**



### 齐次坐标

- **引入齐次坐标的原因**:==平移不能用矩阵的形式(线性变换)表示,因此为了将平移和其他线性变换统一,使用齐次坐标==

- 齐次坐标:在原来维数的基础上多加一维

![image-20220713214521335](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220713214521335.png)

- 我们发现:多加一维时,**平移也可以使用矩阵进行运算了,所有的变换都统一了!**

- 多加的一维上数字的意义:1代表点	0代表向量

- 如此设计的意义:

  - 1.向量具有平移不变性,所以最后一维为0保护其不会被平移

  - 2.

![image-20220713214842703](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220713214842703.png)

- 如此设计也满足了:

  - 向量加向量最后一位仍为0,仍旧为向量

  - 点-点最后一位为0,变成向量

  - 点+向量最后一位为1,是一个点

- ==注意==:当最后一位为大于1的数时,该齐次坐标表示的值为x/w和y/w;因此**点+点的结果是两个点的中点**



### 仿射变换

![image-20220713215902563](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220713215902563.png)

- 类似上图第一个式子的变换,称为仿射变换

- 仿射变换通过齐次坐标就可以用一个矩阵表示

- ==注意==:**齐次坐标表示的变换是先线性变换再进行平移(第二个式子和第一个式子等价)**

![image-20220713220016777](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220713220016777.png)

- ==注意==:对于二维的齐次坐标的变换矩阵的最后一行永远是0,0,1;**而对于其他维数则不一定**



### 逆变换

![image-20220713220214693](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220713220214693.png)

- **逆变换在数学上正好对应乘以逆矩阵**



### 组合变换

- 复杂的变换可以通过一系列简单变换得到

- **变换顺序十分重要**(从矩阵角度理解,矩阵相乘不满足交换律,所以乘以的先后发生变化结果也会发生变化)

- 先旋转后平移:

![image-20220713221021527](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220713221021527.png)

- 先平移后旋转:

![image-20220713221035229](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220713221035229.png)

- 组合变换矩阵相乘的顺序:矩阵乘在向量的左侧,先发生的靠近向量,后发生的远离向量(理解为后发生的变换要在之前的变换完成后在发生)

![image-20220713222040957](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220713222040957.png)

![image-20220713222207238](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220713222207238.png)

- ==注意==:矩阵虽然不满足交换率,**但满足结合律!**可以先将向量左侧的所有变换矩阵综合起来,同时也说明无论多复杂的变换都能用3×3的矩阵表示

- 分解变换:

![image-20220713222430504](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220713222430504.png)

如何让如图左侧的图形**绕其左下角**旋转45°?

- 先移到原点再旋转(只能绕原点旋转),再移回去



## 三维变换

- 与二维类似

![image-20220713223557881](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220713223557881.png)

![image-20220713223611865](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220713223611865.png)

- **同样的,三维的齐次坐标代表的是先进行线性变换再平移**

### 缩放,平移

![image-20220715102451917](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220715102451917.png)

### 旋转

- 分别绕三个轴进行:

![image-20220715102606328](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220715102606328.png)

- 由于y轴是z×x得到,所以形式上与x和z的结果略有区别

![image-20220715102827116](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220715102827116.png)

- **复杂的旋转也是通过在三个轴上的旋转合成得到的**

- 这三个角在数学上被称为:**欧拉角**



**罗德里格斯公式**:

- 任意的旋转都可以用该公式写成矩阵形式,其本质就是在三个轴上的旋转组合而成

![image-20220715103001005](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220715103001005.png)

- **n为所绕的旋转轴的向量(默认从原点出发),α为旋转角度**

- ==注意==:如果想要绕着不是从原点出发的轴进行旋转,可以通过先平移到这个轴的位置,再旋转,再平移回去的操作实现









## 视图变换和投影变换

三维物体变换的过程:

- 模型变换(模型放在一起)->视图变换(找拍摄角度)->投影变换(拍摄照片)

- 简称mvp变换(model	view	projection)

### 视图变换

![image-20220715104044436](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220715104044436.png)

- 关键参数:位置	拍摄方向	向上方向(为了确定相机的旋转,是横过来的还是竖过来的)

![image-20220715104440556](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220715104440556.png)

- 相机的相对运动:如果所有的物体和相机保持相对位置不变,整体移动,所得到的画面也不变

- ==我们规定==:**相机位置放在(0,0,0),并朝-z方向看**,并将所有物体与相机保持相对位置不变移动

如何将相机移动到规定位置?

![image-20220715104855493](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220715104855493.png)

- 先将相机平移到原点,再将拍摄方向旋转到-z,再将向上方向旋转到y,最后将g×t对齐x

- 矩阵表示:M(视图变换)=R(旋转矩阵)T(平移矩阵)

![image-20220715105552481](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220715105552481.png)

- ==注意==:由于齐次坐标是先线性变换再平移,因此这里要先平移的话就要先乘平移的矩阵,再在左边乘以线性变换的矩阵

- 旋转矩阵:由于直接写变换到xyz轴上比较困难,因此**考虑逆变换:先写出xyz轴如何变换到当前相机旋转,再求该矩阵的逆**;又因为我们已知旋转矩阵为正交矩阵,因此**旋转矩阵的逆等于矩阵的转置**,从而得到逆矩阵(见二维旋转)

![](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220715110226404.png)

- 由于相对运动,我们**同时移动相机和模型**,因此又被称为**模型视图变换**

- 模型视图变换是为了之后的投影变换做准备



### 投影变换

正交投影和透视投影的区别:

![image-20220715110806629](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220715110806629.png)

- 正交投影:没有近大远小	透视投影:有近大远小

![image-20220715111333867](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220715111333867.png)



#### 正交投影

简单理解:

![image-20220715111638407](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220715111638407.png)

- 相机看向-z方向

- ==我们规定==:最后的结果放在一个(-1,1)的矩形中,便于之后操作



正规做法:

![image-20220715112205015](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220715112205015.png)

![image-20220715112534812](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220715112534812.png)

- 先确定物体在xyz三个轴的左右,上下,前后范围

- 再平移到原点

- 最后缩放为标准的立方体(-1到1)

- ==注意==:这里远f<近n,因为我们向-z观察,因此OpenGL等使用左手坐标系(避免该问题)

变换矩阵:

![image-20220715114401146](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220715114401146.png)

- 缩放到长度为2:因为-1到1



#### 透视投影

![image-20220715115242188](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220715115242188.png)

**==注意==:齐次坐标中的点 乘以 任何数都仍然表示同一个点**

![image-20220715115849862](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220715115849862.png)

- **透视投影的过程**:先将远平面挤压成近平面的大小,再进行正交投影

- 规定:

  - 近平面大小不变	

  - 远平面的z轴位置(即f)不变	

  - 远平面的中心点位置不变



**寻找挤压得对应关系:**

![image-20220715120756411](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220715120756411.png)

- 由**相似三角形**得:y'=ny/z	(x道理相同)

- 透视转正交得矩阵推导:

![image-20220715121253582](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220715121253582.png)

- **==注意==:这里的z≠f,z只是任意一个点,因此z还不知道**

- 利用齐次坐标的点 乘以 相同的数仍然是那个点的性质可以得到右边的式子

- 也就是左边的式子经过透视到正交的矩阵计算后得到右边的式子

![image-20220715121523868](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220715121523868.png)

- 已知结果的三个量,可以推导出矩阵的大部分信息,接下来推导第三行的信息

![image-20220715122258737](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220715122258737.png)

- 利用之前的条件:

  - 近平面的任何点不会变	

  - 远平面的任何点的z不会变

- 代入近平面上的点:

![image-20220715122351795](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220715122351795.png)

- 将(x,y,n,1)(即近平面上任意一点)代入推导的矩阵式子,可以得到:**第三行的前两个数一定为0**
- 用AB替换后两个未知数;
- 又利用齐次坐标的点乘以相同的数仍然是那个点的性质得到第三行的计算结果为n^2^,即:**==An+B=n^2^==**

![image-20220715122801291](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220715122801291.png)

- 同理,利用远平面中点的位置不变,以及齐次坐标的点 乘以 相同的数仍然是那个点的性质(保证最后一个数是z的坐标值),带入中点的位置得到:==**Af+B=f^2^**==

![image-20220715123013531](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220715123013531.png)

- 两式联立,即可计算出AB大小,因此矩阵求得!

- **透视变换的整体矩阵为:正交变换矩阵乘以透视转正交矩阵**



**思考:挤压以后,中间平面上的某一点的z是被推向n还是推向f?**









## 三角形的光栅化

![image-20220719152128843](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220719152128843.png)

- 长宽比:近平面的宽度:高度	
- 垂直FOV:相机位置到平面上边和下边中点的两条连线(两条红线)所形成的角度

- 水平FOV可以通过长宽比和垂直FOV计算出来

- ==注意==:**这里的lrtb是指近平面上可视范围的上下左右范围**

![image-20220719153536413](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220719153536413.png)

- **定义一个视锥只需要定义一个宽高比,定义一个垂直FOV即可,其他的(远近,上下,左右)通过推导得到就行**

![image-20220719154303708](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220719154303708.png)

- 在视图和投影变换以后,所有的物体都在(-1,1)的立方体内

![image-20220719155058395](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220719155058395.png)

- 屏幕:一个二维数组,每个元素是一个像素

- 光栅=屏幕	光栅化=将画面显示到屏幕上

- 像素Pixel是picture element的缩写,**默认像素是屏幕上的最小单位**

![image-20220719160020262](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220719160020262.png)

- 规定:

  - 像素的坐标用(x,y)表示,范围从从(0,0)到(width-1,height-1)

  - 中心点在(x+0.5,y+0.5)的位置

![image-20220719160306040](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220719160306040.png)

![image-20220719160616516](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220719160616516.png)

### 视口变换

- 将(-1,1)范围的画面先缩放到width和height的长宽,在将该画面的中心点移动到左下角(和屏幕规定的保持一致)

- ==注意==:区别MVP变换和视口变换

- 显示的图像其实就是显存的一块区域,把这块区域映射到屏幕上

![image-20220719162121539](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220719162121539.png)

**LCD:液晶显示器**

- 显示原理:利用光的波动性,通过第一个光栅把光变成竖的,再通过液晶的扭曲,使得光通过横的光栅

- 光经过光栅以后只能按照光栅的方向振动(竖着或横着)

![image-20220719162718644](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220719162718644.png)

LED:利用发光二极管



### 三角形作为多边形的表示形状

![image-20220719163318886](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220719163318886.png)

- 选择三角形的原因:

  - 三角新是最基础的多边形,不能被拆分成其他多边形

  - 三角形一定是在一个平面上的

  - 三角形便于判断点的内外关系

  - 三角形方便内部做插值,做出渐变

![image-20220719180631099](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220719180631099.png)

- 判断像素中点和三角形之间的关系



### 采样:光栅化最简单的方法

![image-20220719180918407](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220719180918407.png)

- ==**采样就是把函数离散化的过程**==

![image-20220719194859271](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220719194859271.png)

- 判断点是否在三角形内,在三角形内则设数组元素为1,否则为0

![image-20220719195434772](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220719195434772.png)

- 如何判断点与三角形的关系:向量的叉积

- **按照顺时针或逆时针顺序,将三角形三边的向量分别×起点到需要判断的点构成的向量,如果三条边叉乘的结果都朝z轴正方向或负方向,则在三角形内**

![image-20220719195750762](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220719195750762.png)

- 边界上的点自己定义,课程中不做处理

![image-20220719200119797](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220719200119797.png)

- 优化光栅化:

  - 轴向包围盒:取x~min~到x~max~和y~min~到y~max~之间的矩形区域

  - 光栅化遍历整个屏幕太过浪费,只需要遍历包围盒即可



实际屏幕光栅化

![image-20220719200558265](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220719200558265.png)

**锯齿问题:信号走样**

![image-20220719201111950](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220719201111950.png)







## 反走样(抗锯齿)

### 采样的artifact(瑕疵,问题)

![image-20220802220102409](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220802220102409.png)

- 锯齿:学名走样

![image-20220802220446576](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220802220446576.png)

- 摩尔纹

![image-20220802220630035](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220802220630035.png)

![image-20220802220721942](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220802220721942.png)

**问题的本质:信号的变化速度太快以至于采样速度太慢,跟不上**



### 反走样的方法:采样前做模糊(滤波)

![image-20220802221222358](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220802221222358.png)

- 采样前做模糊(滤波)

![image-20220802221449706](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220802221449706.png)

- ==注意==:不能先采样再用走样的结果进行模糊



### 为什么这么解决?

![image-20220802221635835](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220802221635835.png)

- 利用余弦研究问题:频率不同

![image-20220802222021325](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220802222021325.png)

- 傅里叶级数展开:任何一个函数都可以用一系列的sin和cos项以及常数项的线性组合来表示

- **==重点==:任何函数都可以被分解为频率从小到大的一系列函数的线性组合**

![image-20220802222441083](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220802222441083.png)

- 任何函数经过傅里叶变换都可以转换成新的函数,并且通过逆傅里叶变换可以重新转换回去

![image-20220802223328349](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220802223328349.png)

- 上图是对一个函数的不同频率进行相同间隔的采样,当函数频率越高时,采样结果越失真

- 结论:当采样频率较低时,可以用频率低的函数进行采样;同理,当采样频率较高时,可以用频率高的函数进行采样

![image-20220802223714785](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220802223714785.png)

- ==**两个频率截然不同的函数用相同的采样频率进行采样,得到完全一样的结果,这种现象称为走样**==

![image-20220802223924062](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220802223924062.png)

#### 滤波:把特定的频率抹掉

![image-20220802224147063](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220802224147063.png)

- 照片通过傅里叶变换从**时域**变到**频域**

- 频域图的观察方法:中心区域代表低频信息,四周区域代表高频信息,哪里亮度越高,这部分频率的信息越多

- 横竖的线:由于默认照片左右上下重复导致,在这里我们忽略

![image-20220802224502531](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220802224502531.png)

- 抹去低频信号,留下高频信号,然后逆傅里叶变换回去,得到图片边界(高通滤波器)

- 边界:左右信号频率发生剧烈变化,因此高频对应边界

![image-20220802224853587](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220802224853587.png)

- 只留下低频信息,抹去高频,再逆傅里叶变换,得到模糊图(低通滤波器)

![image-20220802225011236](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220802225011236.png)

- 高频和低频都去掉,留下中间部分,图像上表现的是留下不明显的边界特征

![image-20220803164818735](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220803164818735.png)

#### 滤波=平均=卷积

![image-20220803164856066](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220803164856066.png)

![image-20220803164927940](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220803164927940.png)

![image-20220803165036565](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220803165036565.png)

- 如上图是卷积的计算

- **实际上卷积的结果就是==信号在对应位置和周边信号的平均结果==**

![image-20220803165155575](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220803165155575.png)

- ==**卷积的定理:时域卷积=频域相乘,时域乘积=频域卷积**==

![image-20220803165509722](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220803165509722.png)

- 图像与上图的3*3卷积盒卷积后可以得到模糊的图像

- 图像通过傅里叶变换变成频域信息后与3*3卷积盒傅里叶变换的频域信息(其实转换后发现就是一个低通滤波器)相乘的结果,将该结果逆傅里叶变换可以得到相同的图像

- 结论:证明了时域卷积=频域相乘

![image-20220803174930108](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220803174930108.png)

- 低通滤波器

![image-20220803175315861](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220803175315861.png)

![image-20220803175605358](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220803175605358.png)

- 对比结论:采用更大的卷积盒,得到的频域图越小(卷积盒越大,平均的像素越广,原图像越模糊,因此该图像的频域图也更集中在低频)

- 通过对比帮助理解不同卷积盒的特点

![image-20220803180200985](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220803180200985.png)

#### 采样=重复频域上的内容

![image-20220803180239961](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220803180239961.png)

- **时域相乘=频域卷积**

- **时域上:被采样的函数与冲激函数相乘得到采样的结果(一群离散的点)**

- **频域上:被采样的函数与冲激函数卷积得到被采样函数频域图像的重复**

![image-20220803180823977](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220803180823977.png)

为什么会产生走样?

- ==**因为采样的频率越低,频域上重复图像的间隔越小,间隔太小导致产生混叠失真**==

![image-20220803181332183](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220803181332183.png)

解决方案:

- 增加采样率

- 先模糊(拿掉高频信息)再采样

![image-20220803181441016](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220803181441016.png)

- **当去掉信号的高频部分后,再用原来的采样率进行采样,就不会发生混叠,从而解决走样问题**

![](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220803181606938.png)

如何对三角形进行模糊操作?

![image-20220803181947421](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220803181947421.png)

![image-20220803182034764](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220803182034764.png)

![image-20220803182134861](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220803182134861.png)

- 使用大小为一个像素的卷积盒,求像素内的平均值,最后再采样

- 问题:计算量比较大,难以算出每个像素具体的值



### MSAA(multi sample antialiasing):使用更多采样点反走样

- 问题:只是对反走样的近似,并不能解决走样问题

![image-20220803212517254](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220803212517254.png)

![image-20220803212734850](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220803212734850.png)

![image-20220803212813312](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220803212813312.png)

![image-20220803213055529](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220803213055529.png)

- 将每个像素划分为更小的N*N的点,并判断在三角形内的点占整个像素内的点的比例,从而近似一个像素的值

- **==MSAA并没有提高采样率!==**而是通过更多的点去近似得到模糊操作后的值

- MSAA的问题:计算量大

- **实际情况中MSAA应用**:不是标准的N*N的划分,而是有特殊的图案来划分,并且点之间存在复用来减小计算量

### 其他反走样方法

![image-20220803220906383](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220803220906383.png)

- FXAA:先获得有锯齿的图,通过图像匹配找到边界,最后将边界换成没有锯齿的状态(快)

- TAA:复用上一帧的数据,将MSAA的样本分布在时间上(高效!)



超分辨率/超采样:

- 将小分辨率拉成大分辨率:会产生锯齿(采样样本不足产生的问题)

- DLSS:深度学习,猜测缺失的细节





## 消隐问题

![image-20220804185435748](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220804185435748.png)

### 画家算法

- 由远及近将物体光栅化
  - Eg:画立方体,先画最后面的一个面,再依次画左上右下四个侧面,最后画正面

- 问题:四个侧面如果按照右上左下的顺序画,就会出现问题,因此画家算法存在问题

![image-20220804190213225](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220804190213225.png)

- 物体深度排序的时间复杂度:O(nlogn)

- 问题:如上图的复杂情况,画家算法无法奏效

![image-20220804190518358](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220804190518358.png)

### Z-Buffer算法

- 以每个像素为单位,使用两个缓存图像,一个记录每个像素当前最近的物体的距离信息(Z-Buffer),另一个记录最近物体的颜色(frame buffer)

- ==**注意**==:在这里研究的z表示物体到相机的距离,因此始终为正(原来的概念中是相机在原点往-z看)

![image-20220804202306400](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220804202306400.png)

- 右边的是z-buffer,深度约大,颜色越浅

![image-20220804202729328](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220804202729328.png)

![image-20220804202846926](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220804202846926.png)

- 一开始z-buffer中都存储无限大的数

- 接着判断新的三角形(物体由许多三角形构成)中每一个像素与z-buffer中对应像素的深度值大小,并保留深度更小的值

![image-20220804203812347](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220804203812347.png)

- 时间复杂度:O(n)

- 该算法不受物体顺序影响

- 如果两个三角形在同一个像素有相同深度如何处理?
  - 一般情况下由于计算都用浮点数进行,基本不会出现同一像素两个三角形深度完全相同,若完全相同之后课程解释

- 根据MSAA,考虑到可能根据采样点的深度进行z-buffer

- ==**z-buffer是目前最流行的消隐算法**==





## 着色Shading

![image-20220804205053906](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220804205053906.png)

- **shading的计算机图形学定义:对不同物体应用不同材质**

![image-20220804205255629](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220804205255629.png)

### Blinn-Phong反射模型

![image-20220804205454727](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220804205454727.png)

- 高光

- 漫反射

- 间接光照

总结:着色有三种不同的部分

![image-20220804205746762](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220804205746762.png)

- ==注意:==默认在平面的一个很小的范围,物体表面是直的

- n:法线方向 surface normal

- v:观测方向 view direction

- l:光照方向 light direction

**以上三种向量都是单位向量**

![image-20220804210150717](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220804210150717.png)

- ==**注意**==:暂时不考虑其他物体在这个点的阴影造成的影响

#### 漫反射

![image-20220804210338347](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220804210338347.png)

- 漫反射在每个观测角度上光线能量都是一样的

![image-20220804213232200](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220804213232200.png)

- 着色点要考虑单位面积接收到多少能量

- **兰伯特余弦定律:接受到光线得能量与光照方向和法线方向之间夹角得余弦成正比**

![image-20220804214242053](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220804214242053.png)

**结论:点光源为中心的球壳上光的能量与到点光源的距离的平方成反比**

![image-20220804214518197](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220804214518197.png)

**漫反射=点光源到shading point的光照强度*光线和法线夹角的余弦值**

解释:max(0,n·l)

- n·l是余弦值(单位向量点乘),取max是因为当余弦值为负时代表光线从平面下方照射过来,物理上没有意义,因此取0

解释:k~d~

- k~d~是反射颜色的系数

- **物理学上**:物体吸收光线一部分波长的能量,并反射无法吸收的,反射出的能量波长对应我们看到的颜色
  - Eg.k~d~为1时说明shading point不吸收光线能量,光线全部反射;k~d~为0时说明所有光线能量都被吸收,我们看到的是黑色的

- ==**实际上**==:k~d~是rgb三个通道的值表示的向量,每个通道都是0~1,因此表示颜色

解释:为什么公式与v(观察角度)没有关系

- 漫反射在每个方向上的光线能量相同,因此与v无光

![image-20220804220052873](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220804220052873.png)



#### 高光项

![image-20220805144729615](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220805144729615.png)

- 当观察角度在镜面反射角度附近时(v约等于R),则能够看到高光,反之则看不到

![image-20220805145020688](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220805145020688.png)

- 半程向量h:光照方向和观测方向的角**平分线上**的**单位向量**
- 将观测方向和镜面反射方向是否相近的问题转化为:**半程向量和法线向量是否相近**
- 用点乘判断是否相近,点乘值越靠近1则越相近,越靠近0则越远

公式解释:

- k~s~仍然表示颜色
- 实际上高光项也要考虑单位面积光线能量接受率`max(0,n·l)`,只不过这个Blinn-Phong模型中将其省略了
- 不用R·v而选择用h·n是因为R·v计算量更大;使用R·v的为Phong模型;使用h·n是Blinn-Phong模型
- 指数p:用于控制高光有多大

![image-20220805150313535](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220805150313535.png)

- 可以看到随着指数变大,cos函数变化的越剧烈
- **实际情况中p取100~200,符合我们看到的实际情况**

![image-20220805150511028](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220805150511028.png)





#### 环境光项

![image-20220805150537565](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220805150537565.png)

- 环境光非常复杂,Blinn-Phong模型中简化为恒定的环境光
- 环境光与l,v,n都没有关系,**是一个常数**
- 具体计算需要全局光照的知识



#### 整个Blinn-Phong反射模型公式

![image-20220805150817856](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220805150817856.png)



### 着色频率

![image-20220805151157387](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220805151157387.png)

- 左边:着色应用在每一个**平面**上	中间:着色应用在每一个**顶点**上(三角形中间部分做插值)	右边:着色应用在每一个**像素**上

![image-20220805151445921](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220805151445921.png)

- flat shading:着色应用在每个三角形平面上,三角形内部不做插值

![image-20220805151611176](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220805151611176.png)

- Gouraud shading:每个顶点应用着色,三角形内部做插值

![image-20220805151753488](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220805151753488.png)

- Phong shading:先求出三角形顶点法线方向,再插值出三角形内部的法线方向,最后将着色应用在每个像素上

- 概念区分:phong模型是着色模型,phong shading是着色频率,是两个不同的概念

![image-20220805152352151](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220805152352151.png)

- 当面数足够多时,phong shading不一定比flat shading效果更好,反而可能更差,计算量也不一定更小,反而可能更大

如何求顶点的法线?

![image-20220805152722269](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220805152722269.png)

- 对顶点所关联的平面的法线求(面积的加权)平均

如何插值得到三角形中每个像素的法线?

![image-20220805152940957](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220805152940957.png)

- 利用重心坐标





## 图形(实时渲染)管线Graphics Pipeline

![image-20220805161249314](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220805161249314.png)

![image-20220805161859025](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220805161859025.png)

![image-20220805161916390](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220805161916390.png)

![image-20220805161933849](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220805161933849.png)

![image-20220805162044624](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220805162044624.png)

- 如果是Gouraud shading则发生在顶点处理阶段,如果phong shading则发生在采样点(像素)处理阶段
- GPU的渲染管线中有可编程的代码,用来决定顶点/像素如何着色,这些代码被称为**shader**

![image-20220805164152357](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220805164152357.png)

渲染管线三大步骤:

- 顶点处理:MVP变换
- 光栅化:采样,消隐
- 着色

![image-20220805164235159](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220805164235159.png)

Shader

- 用于指定每一个像素如何着色,通用于每一个像素
- 两类:vertex shader(顶点)     pixel/fragment shader(像素)
- 网站:shadertoy.com





![image-20220805165220609](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220805165220609.png)

- 游戏引擎

![image-20220805165327090](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220805165327090.png)

- GPU:高度并行化处理器





## 纹理映射Texture Mapping

- 作用:定义任何一个点的不同属性
- 区别纹理和着色:纹理用于规定着色时不同点的属性

![image-20220805173254034](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220805173254034.png)

- **任何三维物体的表面都是二维的**

![image-20220805173424393](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220805173424393.png)

- 纹理映射:将二维的图像蒙在三维物体表面
- 前提条件:已知组成三维模型的三角形的每个顶点对应在二维贴图上的坐标
- 再通过**插值**确定三角形内部每个点各自的属性

![image-20220805173910479](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220805173910479.png)

- **纹理上的坐标:UV**
- u,v的范围都是0~1

![image-20220805174221857](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220805174221857.png)

![image-20220805174245316](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220805174245316.png)

纹理所对应的UV图

解释:为什么UV重复但是贴上纹理后边界没有明显分界?

- 因为是无缝贴图(tilable texture),边界可以无缝衔接



那如何在三角形内部进行插值?

### 重心坐标Barycentric Coordinates

![image-20220806104841298](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220806104841298.png)

- 为什么需要插值?
  - 需要确定三角形内部每个点的属性

- 哪些属性需要插值?
  - 纹理,颜色,法线向量

![image-20220806105114824](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220806105114824.png)

- 一个三角形对应一套重心坐标
- **在三角形ABC所对应的平面上的任意一点都可以表示成ABC三个点的线性组合**
- **必须满足α+β+γ=1**
- **当αβγ都>0时,点一定在三角形内**
- 将αβγ作为一个点的坐标来描述点的位置
- 因此不需要坐标系依然能够得到点的坐标
- 已知αβγ中两个数就可以得到坐标(因为平面式二维的)

![image-20220806110106122](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220806110106122.png)

- 三角形内点的重心坐标可以通过面积比得到
- 面积可以通过叉乘得到:S=(absinc)/2

![image-20220806110325096](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220806110325096.png)

- 通过面积的重心坐标定义可以得到:三角形重心的重心坐标为(1/3,1/3,1/3)

![image-20220806111005408](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220806111005408.png)

- 给定坐标也能得到重心坐标
- 没有必要记忆公式

![image-20220806115622708](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220806115622708.png)

- 通过三个点属性的线性组合实现插值
- 问题:**三维图形投影以后重心坐标会发生变化!**
- 因此插值需要在三维图形的重心坐标中计算,而不能在投影以后的二维坐标上运算
- **实际问题:在光栅化中必须先找到屏幕上的像素中心点所对应的三维图形的坐标,在三维图形上进行插值,最后逆变换回屏幕**



### 应用纹理

![image-20220806163510051](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220806163510051.png)

- 利用重心坐标算出三维物体上点对应的UV值
- 将三维物体上的点的UV对应到纹理上的UV
- 根据纹理设定这个点的k~d~

问题1:纹理放大

![image-20220806163912014](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220806163912014.png)

- 纹理分辨率太小而模型分辨率高导致纹理被放大
- nearest:对获取到的模型坐标取最近的纹理元素(texel),使得一个像素周围的几个像素都对应相同的纹理元素

  - 效果不好,如左图

#### bilinear双线性插值

![image-20220806165457351](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220806165457351.png)

- 对于三维物体上的点无法一一对应到纹理元素上时

![image-20220806173914840](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220806173914840.png)

- **线性插值Lerp**:对两个已知点,当x=0时线性插值结果为第一个点,x=1时线性插值结果为第二个点(见上图公式)
  - eg.x=0.5时,线性插值为两个点的中点

- 以三维物体上的点四周的四个点为基准,以左下的纹理元素作为原点
- 先在上下两条边对三维物体上的点**进行水平方向的线性插值**
- 再**用水平方向线性插值的结果计算该点竖直方向上的线性插值**,得到双线性插值后的结果
- 最后效果为中间图



- bicubic:取周围的16个做线性插值





问题2:纹理太大

![image-20220806191106794](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220806191106794.png)

- 远处发生摩尔纹,近处发生走样

![image-20220806192212514](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220806192212514.png)

- 近处的像素覆盖纹理正常,远处的像素覆盖大量纹理
- 远处的像素用像素中心采样导致走样:像素中心无法代表像素覆盖区域的值

![image-20220810154106540](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220810154106540.png)

- 使用超采样可以解决,但**计算量太大**

![image-20220810154330370](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220810154330370.png)

- **产生问题的根本原因**:采样频率跟不上纹理出现的频率(纹理太大)
- 思考:使用不采样的方法,直接得到像素区域内的平均值

![image-20220810154540077](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220810154540077.png)

- 范围查询:数据结构与算法的问题,具体算法有很多

![image-20220810154840721](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220810154840721.png)

#### **Mipmap**

根据图像,提前生成mipmap,每一层是上一层分辨率的一半

- 可以范围查询
- 查询速度快,结果不够准确,只能应用于方形范围
- mipmap的存储空间比原图大小多原图的1/3
  - 推导:1+1/4+1/16+.....



如何得到已知像素应该去哪一层mipmap上查询对应纹理?

![image-20220810160111370](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220810160111370.png)

![image-20220810160144829](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220810160144829.png)

- 将需要操作的像素的中心与其上方和右方的像素中心投影到纹理坐标上
- 分别大致的计算出在纹理坐标上,需要操作的像素中心到上方像素中心 和 到右方像素中心的距离
- 取两个距离中的最大值,命名为L
- 以最大值为边长,近似该像素在纹理上所覆盖的区域
- 在log~2~L层mipmap上找到该像素对应的平均值

![image-20220810161015935](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220810161015935.png)

- 按照上例计算,得到的像素的mipmap层数不连续
- 会导致纹理不连续的缝隙
- 思考:如何让查找的mipmap层数连续?(eg.找到第1.5层的mipmap)

![image-20220810161232682](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220810161232682.png)

#### 三线性插值

- 对这个像素所得到层数(非整数)在前后两个整数的mipmap层数上先进行双线性插值
- 对两个双线性插值的结果再做一次线性插值,得到最后的结果
- 该算法应用广泛

对比结果:

![image-20220810161902969](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220810161902969.png)



使用mipmap的结果:

![image-20220810162016837](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220810162016837.png)

- 仍然存在问题:远处全部被糊掉(Overblur)
- 原因:mipmap只能查找正方形区域,以及三线性插值也是一种近似,**各种近似导致结果不准确**



解决只能方形查询问题

![image-20220810162226453](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220810162226453.png)

#### 各向异性过滤

![image-20220810162621563](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220810162621563.png)

- 在mipmap的基础上,增加单独压缩长或宽的mipmap
- 存储空间是原图的三倍
- 各向异性:考虑不同的方向性
- 参数:numX
  - 加载到第num层
  - 增加层数会稍微增加存储空间,但不多,与算力无关

![image-20220810162716004](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220810162716004.png)

- 在实际情况中,**一个像素可能对应的区域是矩形区域,导致用方形去近似结果不准**
- 各向异性过滤使得矩形的区域结果更加准确
- 但是! 对于斜着的矩形(如上图),仍然无法更加准确的去近似
- **因此,各向异性过滤只能解决部分问题**

![image-20220810163037353](https://jupiter-typora-pic.oss-cn-shanghai.aliyuncs.com/image-20220810163037353.png)

- EWA
- 将不规则的形状分解成多个形状
- 多次查询mipmap得到结果
- **效果好,计算量大**
