# UE4 笔记

## 蓝图

更改组件中的变量：拿到组件的引用就能拉线来访问组件上的变量

Event：用Event模块才能触发逻辑

变量后面的眼睛点开就能在detail界面修改变量值

​	（注意：detail面板的修改不会应用到蓝图中，需要在edit blueprint的选项中手动保存到蓝图）

升级为变量：将一个参数变成一个单独的变量



### 试验运行中的变量

Event：Mouse Wheel Up/Down

Press:发生事件		Release：未发生事件

+/-模块：给变量加、减某个指定值

print string模块：在editor面板上打印变量



### 添加冲刺

绑定冲刺的按钮：project settings->input->Action Mappings中添加冲刺的按钮

添加Action Event：搜索冲刺设置的名字



### 添加蹲伏

时间轴模块：触发变量，通过时间轴上打关键帧，自定义变量的值的变化过程，用于连续的变化，常与Lerp一起使用

​	参数：双击模块：创建新的时间轴关键帧

​	结点：play：正向播放关键帧

​				reverse：逆向播放关键帧

FirstPersonCamera引用->SetRelativeLocation：设置相机的相对位置（UE4中默认人物的模型【子物体】跟随相机位置移动）

FirstPersonCamera引用->SetRelativeLocation->NewLocation连线添加插值模块

插值（Lerp）模块：两个变量AB（变量类型取决于你连到的变量是什么类型）分别代表变化起始位置和终点位置，Alpha代表变换趋势，即连上时间轴模块设置的变量

​	参数：A/B：起始，终点参数，参数类型根据需要return的value决定

​				Alpha：通常是时间轴的关键帧参数

​				ShortestPath：建议勾选，保证按照AB间最短路径运动

![微信截图_20220531165318](C:\Users\qingl\Downloads\微信截图_20220531165318.png)

改变CapsuleComponent高度：

CapsuleComponent引用->SetCapsuleNewHeight->HalfHeight连接插值模块

CapsuleNewHeight值也会影响到摄像机（子物体）的高度

修复蹲伏时冲刺：

自定义Event：通过自定义事件可以在触发自定义事件的模块（相同名称）被执行的时候调用

Branch分支：类似if，判断bool值真假，分别执行不同的模块





### 添加生命值和调试伤害

execute console command：输入命令发送到控制台，如restartlevel

返回bool值的模块（如判断大小关系等）用branch接受，分别对应true和false两种情况



### 控件蓝图（WidgetBlueprint）

类似于UI设计

创建控件蓝图：content browser->右键->userinterface->widgetblueprints

控件蓝图右上角Designer界面：设计UI显示的样式

控件放在Horizontal Box中（水平）

progress bar：进度条

​	detail面板：

​		slot调整是否充满（Fill）还有进度条格式

​		apperance->FillColorandOpacity:修改颜色

​		progress->Percent右边bind按钮->createbinding：创建绑定，逐帧判断玩家的血量（此处不用这个实现）

控件蓝图右上角Graph界面：编辑蓝图逻辑

自定义事件模块中可以添加变量作为Inputs

Progress Bar引用->Set Percent：控制Progress Bar显示的百分比

​	参数：inPercent：0到1的数来控制百分比

Progress Bar引用->Set Fill Color and Opacity->In color选项：设置血条的颜色

​	参数：inColor：接受颜色参数

select：在index设置的情况下进行2选1

​	参数：index：接收判断的条件

​				True：设置Ture情况下返回什么变量

​				False：设置False情况下返回什么变量（返回什么类型的变量是由ReturnValue后面接受的框的类型决定的）

血量蓝图思路：

![image-20220531214849339](C:\Users\qingl\AppData\Roaming\Typora\typora-user-images\image-20220531214849339.png)

控制人物的蓝图中：

​	beginplay事件中：

​	create widget：用于中创建一个控件

​		参数：class：即放入要创建的控件蓝图

​					return：返回一个控件对象

​	add to viewport：将target添加到画面上

![CreateWidgetNode.png](https://docs.unrealengine.com/4.27/Images/InteractiveExperiences/UMG/UserGuide/CreatingWidgets/CreateWidgetNode.jpg)

（图中main menu widget就是要显示的控件）

造成伤害的蓝图中：

healthbar引用（Widget Bar控件）->Update Healthbar（自定义事件）：触发自定义事件，并且获得人物脚本中的health变量

![微信截图_20220531220152](C:\Users\qingl\Downloads\微信截图_20220531220152.png)



### 后期处理体积（PostProcessVolume）

detail面板：

​	color grading：环境的颜色相关设置

​		color grading->WhiteBalance：改变环境的色温

​	Post Process Volume Settings：

​		Post Process Volume Settings->InfiniteExtent：勾选后影响的体积设置成无限范围（否则只能在进入这个体积时才有效果）

​		Post Process Volume Settings->Priority：高优先级的先生效

​		Post Process Volume Settings->BlendWeight：混合权重，即PostProcess的效果的表现程度

实现血量越低画面饱和度越低效果：设置PostProcessVolume饱和度为0，然后通过混合权重改变影响的大小

**PostProcess可以设置成一个控件**，用于影响这个物体看到的画面情况

控件的detail面板：

​	PostProcessVolume->color grading->Global->Saturation：饱和度

​	PostProcessVolume->BlendWeight：混合权重（注意设置初始值）

在人物蓝图中的人物扣血模块中：

PostProcess引用->setblendweight：设置混合权重的大小

​	参数：BlendWeight：0到1的一个数

MapRangeClamp模块：将一个范围的值转换为另一个范围等比例的值（如设置了Input范围0到100，Output范围0到1，输入值为50时会输出0.5）

​	参数：Value：输入的变量

​				InRangeA/B：输入的范围的左右值

​				OutRangeA/B：输出的范围的左右值

![image-20220603215259816](C:\Users\qingl\AppData\Roaming\Typora\typora-user-images\image-20220603215259816.png)



### 父蓝图

蓝图viewport界面：

​	add component->static mesh：静态网格体，在details面版下的Static Mesh中选择静态网格体模型

实现创建门父蓝图：利用时间轴，经过Lerp，来控制Door的旋转参数

蓝图EventGraph界面：

​	时间轴：

​		控件关键帧曲线圆滑：选中所有点，右键选择User

​		关键帧界面参数Length：整个动画的长度

​	door（之前创建的静态网格体门）引用->setRelativeRotation：设置物体旋转参数

![image-20220604205835286](C:\Users\qingl\AppData\Roaming\Typora\typora-user-images\image-20220604205835286.png)



### 子类蓝图

右键父类蓝图，创建子类蓝图

蓝图viewport界面：

​	add component->box collision：盒形碰撞组件

​		details面板：

​			Shape->BoxExtent：调整碰撞器大小

​			Collision->Collision Presets：选择OverlapAllDynamic（和所有动态物体重叠）

​			Collision->Generate Overlap Event：勾选后与碰撞器重叠时触发重叠事件

​			Events：可以创建重叠开始/结束时触发的事件；事件会被添加成为蓝图中的一个触发事件模块

实现门的子类，当人物进入碰撞器内时门自动打开，离开时自动关闭：添加Box Collision，在Box Collision的details面板的Event中添加OnComponentBegin/EndOverlap事件，在满足重叠物体是角色时，触发Open/CloseDoor事件

OnComponentBegin/EndOverlap事件：当和碰撞器开始/结束重叠时触发此事件

​	参数：Other Actor：输出和碰撞器重叠的Actor

GetPlayerActor：获取玩家引用的模块

![image-20220604214123167](C:\Users\qingl\AppData\Roaming\Typora\typora-user-images\image-20220604214123167.png)



### 两个Actor之间的通信

实现用一个踏板触发门：通过OnComponentBeginOverlap触发opendoor（自定义）事件；再通过OnComponentEndOverlap触发closedoor（自定义）事件，注意closedoor触发前需要检查与碰撞器重叠的Actor是否有效，即保证所有重叠的物体都离开了才关门

在蓝图EventGraph界面：

​	获取其他蓝图对象的引用：新建变量，将类型改为蓝图的对象引用	

​	变量的details面板->InstanceEditable：允许在蓝图界面外的项目窗口的details中修改这个变量

​	Box引用->GetOverlappingActors：获取到所有和碰撞器重叠的一类物体，return一个数组，存放所有重叠的这类物体（不会把自己排除，所以返	回的数组的第一个是物体本身）

​		参数：ClassFilter：选择获取和什么类型的物体重叠

​	Get（a cpoy）模块：获取到数组

​		参数：下标

​	is Valid模块：判断输入进来的物体是否有效，分为true和false两条路

在项目面板里：

​	物体需要勾选上GenerateOverlapEvent并且CollisionPreset是PhysicsActor才可以触发重叠事件

![image-20220605094513461](C:\Users\qingl\AppData\Roaming\Typora\typora-user-images\image-20220605094513461.png)



### Actor触发交互事件

实现进入范围后按e开关门：设置新的互动按键，在进入盒体范围内显示TextRender并允许玩家输入按键，触发对应的输入事件，通过FlipFlop模块来轮流触发开关门事件

在蓝图EventGraph界面：

​	InputAction xxx事件：右键搜索xxx可以找到该事件，在projectsettings里的inputs设置新的按键xxx后，即可在蓝图中创建InputAction 	xxx事件，当按下xxx对应设置的键时触发该事件

​		参数：Press结点：按下对应按键触发

​					Release结点：松开对应按键触发

​	Enable/DisableInput模块：允许/禁止玩家进行输入操作

​		参数：player control：通常连上GetPlayerControl模块，用于获取指定玩家的控制输入

​					Target：允许/禁止被玩家进行输入操作的对象

​	FilpFlop模块：被触发后就会做AB对应的两条指令轮流做，如第一次按触发A，第二次按就触发B，一直轮流

​		参数：A/B：连接对应需要触发的指令

在蓝图ViewPort界面：

​	控件TextRender：显示一个文本

​		details面板->Rendering->Hidden in Game：选择游戏开始时文本是显示还是隐藏

在蓝图EventGraph界面：

​	TextRender引用->setHiddenInGame：设置TextRender是否隐藏

​		参数：NewHidden：设置是否隐藏

![image-20220605111457050](C:\Users\qingl\AppData\Roaming\Typora\typora-user-images\image-20220605111457050.png)

![image-20220605111520792](C:\Users\qingl\AppData\Roaming\Typora\typora-user-images\image-20220605111520792.png)



### 蓝图接口

创建蓝图接口：contentBrowser面板右键->blueprints->blueprintsInterface

蓝图接口中可以自己创建函数

给蓝图类添加接口：蓝图面板中classSettings->details面板->Interface->Add；蓝图界面左侧会出现该接口中的函数，右键->ImplementFunction来创建接口事件，当该事件触发时会执行后面的语句

实现对着门按e就能开门（射线检测）：给人物添加之前设置的InputAction Interact事件，并使该事件触发射线检测，如果射线检测到的物体且该物体有interact接口就开门

在蓝图EventGraph界面：

​	SphereTraceByChannel：圆柱形的射线检测

​		参数：Start：起始位置

​					End：终点位置

​					Radius：圆柱射线的半径

​					TraceChannel：Visibilty，对可见并且可以阻挡射线的物体进行返回

​					DrawDebugType：设置为ForDuration，就会画出射线的情况

​	FirstPersonCamera引用->GetWorldLocation：获取其世界坐标						

​	FirstPersonCamera引用->GetForwardVector：获取其朝向的正面的一个单位向量

​	BreakHitResult：可以返回检测到的物体的各类信息

​		参数：Blocking Hit：bool值，是否击中了物体

​					HitActor：返回击中得Actor

​	DoesImplementInterface模块：检查是否有选定的接口，返回bool

​	触发接口模块（此例中为interact）：Target：有该接口的类型

物体碰撞问题：

​	如果物体没有碰撞，先检查物体的蓝图中的CollisionPreset是否正确，如果设置正确但还是没有碰撞，就进入staticmesh检查是否有碰撞，工具栏->collision->simpleCollision可以查看碰撞形状，如果没有碰撞，在上方小的选项中的collision中选择合适的add collision进行添加

ForstPersonPlayer中：

![image-20220605204749945](C:\Users\qingl\AppData\Roaming\Typora\typora-user-images\image-20220605204749945.png)

![image-20220605204707811](C:\Users\qingl\AppData\Roaming\Typora\typora-user-images\image-20220605204707811.png)

子类门蓝图中：

![image-20220605205648680](C:\Users\qingl\AppData\Roaming\Typora\typora-user-images\image-20220605205648680.png)



### 加入粒子系统和声音

声音文件有两种：cue和正常的音频文件

​	在cue中可以修改声音每一次播放，让他们有一些区别

声音进入场景后有一定的范围，越靠近声音越响，越远离声音越轻



在蓝图EventGraph界面：

​	在蓝图中添加声音：

​	playSoundAtLocation模块：在指定位置播放声音

​		参数：Location：位置

​					Sound：要播放的声音

​	在蓝图中添加粒子系统：

​	spawnEmitterAtLocation：在指定位置生成粒子系统

​		参数：Location：位置

​					EmitterTemplate：粒子

​	

解决子弹撞墙反弹后没有触发声音和粒子系统就自动销毁的问题：

class default->Actor->InitialLifeSpan：初始生命周期长度，修改为默认，就不会被销毁了

使用事件beginplay触发delay三秒，再执行播放声音和触发粒子系统和销毁的命令

事件beginplay是在这个该蓝图类开始执行时执行





## 美术资产

### maya模型导入

导入时的选择栏中几个重要选项：

​	ConvertScene：把FBX坐标系转换为虚幻坐标系

​	ConvertSceneUnit：把FBX单位转换为虚幻的单位

​	CombineMeshes：将场景中的单独的网格体合并成一个



### 网格

摁住鼠标中键拖动：可以量取距离

在视口中选中模型右键->transform->Snap/Align：有各种对齐网格的选项可以选择



### 纹理Texture

Texture分类：

​	BaseColor：材质的颜色

​	Normal：法线贴图，让模型的光照何反射更精准，偏蓝色

​	Roughness：粗糙度信息，纯灰度

​	Metalness：金属度信息，只有01信息，0不是金属1是金属

​	AO：环境光遮蔽贴图，也被称为AO纹理贴图，暴露在环境光下的为白，被遮挡的为黑

​	ChannelPackedTexture（RMA）：多个纹理分别通过RGB通道组合而成，通常是粗糙度，金属度和AO，每个通道8位

纹理密度

导入选项：

​	SkeletalMesh：骨骼网格体

​	AutoGenerateCollision：自动生成碰撞

​	CombineMeshes：其他3d软件的场景中的网格体组合成一个

​	NormalImportMethod：选择ImportNormals

​	NormalGeneratonMethod：选择Mikk Tspace，确保表面渲染错误数量减少

​	MaterialImportMethod：选择是否添加材质，一般不会使用3d软件中的材质

​	ImportTextures：选择是否导入贴图

贴图导入选项（details面板）：

​	CompressionSettings：压缩设置

​		base color：default	Normal：Normalmap	ChannelPacked：Mask

​	sRGB：色彩校准，选择是否开启，Basecolor需要开启

​	FlipGreenChannel：在normal贴图中，DX渲染的不用勾选，OpenGL渲染的要勾选

​	

### 基础模型（block out）

模型的简单版本

对模型在外部3d软件进行修改并重新导出后，点击模型窗口上的reimportBaseMesh，就可以重新导入模型



### 碰撞设置

碰撞网格体必须是凸包的

在外部3d软件创建碰撞网格体：

​	UBX：盒子碰撞器	UCX：自定义形状碰撞器	USX：球型碰撞器

​	在maya中用几何体做出碰撞网格体的样子并且以UBX_开头命名，作为物体模型的子物体，导入虚幻时会自动识别成碰撞网格体

SimpleCollision：简单碰撞，用于阻挡玩家	ComplexCollision：复杂碰撞，用于互动时对于特定面有特殊要求

在ue中创建碰撞网格体：

​	模型界面菜单中Collision选项：提供多种自动生成碰撞的选项

​		DOP：根据XYZ坐标生成更加精确的碰撞器

​		AuotoConvesCollision：自动生成碰撞器，可以调节精度，凸包的数量等（精度越高消耗越大）

​		details面板->useComplexCollisionAsSimple：用复杂碰撞的网格体作为简单碰撞器（消耗大）



### 优化（Optimization）

环境美术优化：多边形数量polycount，绘制调用，LOD，材质

​	绘制调用：一个网格体+一个材质需要消耗一个绘制调用，每加一个材质就需要多消耗一个绘制调用

​	LOD：Level of Detail，在近处为高质量模型，在远处自动换成低质量模型，从而节省性能

​		LODSettings->LOD Group：选择LOD的各种预设

​		LOD X->ReductionSettings->PercentTriangles：控制三角形数量

​		LODSettings->LOD Import：自定义导入不同LOD模型

​		LODSettings->AutoComputeLODDistance：选择是否自动计算不同LOD的切换位置

​		LOD X->ScreenSize：与上一个LOD切换的屏幕大小

​		MaterialSlots->MaterialSlots：添加材质



### 创建材质

contentBrowser面板右键->创建材质

将贴图移到材质面板，会生成TextureSample结点

​	参数：RGB：RGB同时输出

​				R/G/B：三个分别单独输出

将贴图结点通过连线输出到**材质结点**上

​	参数：对应的不同贴图类型

details面板：

​	选中贴图结点，检查MaterialExpressionTextureBase->SampleType设置是否和贴图类型一致

在工作界面的视口界面，左上角显示模式中选择OptimizationViewMode->ShaderComplexity可以查看场景中模型显示性能的消耗



### 主材质和材质实例

类似父子关系，父类拓展参数，子类可以根据自己的需要对参数修改

利用主材质创建多个材质实例应用在不同的模型上：

​	主材质中：

​		将贴图结点转换为参数，这样材质实例中就能将贴图作为参数进行更换

​	材质实例中：

​	将模型赋上对应的材质实例，在材质实例中勾选使贴图参数可以被更改

​	注意贴图的类型要和贴图的CompressionSettings一致防止报错

使材质实例可以控制UV平铺的纹理量：

​	在主材质中：

​		TexCoordinate结点：控制者两个方向上的纹理平铺参数

​		让TexCoordinate结点乘以一个constant变量，从而通过控制constant参数的大小来修改平铺

​		贴图参数结点的参数UV控制平铺

![image-20220607115352514](C:\Users\qingl\AppData\Roaming\Typora\typora-user-images\image-20220607115352514.png)

使材质实例可以控制颜色：

​	主材质中：

​		添加constant3参数，分别代表RGB三个通道的值，可以调整颜色

​		将constant3参数乘以BaseColor贴图的RGB，输出到材质结点的BaseColor

![image-20220607125046693](C:\Users\qingl\AppData\Roaming\Typora\typora-user-images\image-20220607125046693.png)

为了节省性能，使主材质中一些参数调节在选中情况下才开启：

​	利用StaticSwitchParam结点：可以在details界面选择该参数结点为true还是false，分别执行两条不一样的命令

![image-20220607125118866](C:\Users\qingl\AppData\Roaming\Typora\typora-user-images\image-20220607125118866.png)

修改材质的着色模型和混合模式：

​	在材质实例面板的details面板中General->MaterialPropertyOverrides->BlendMode和ShadingModel



### 光照

光照类型：

​	在灯光的右侧details面板修改mobility选项

​	Static：消耗少，直接渲染在光照贴图上并且之后不再实时渲染；可移动的物体不会被照亮

​	 Stationary：消耗介于中间，不能动，但能够修改亮度和颜色，渲染到光照贴图上

​	 Moveable：消耗大，实时进行渲染

光源类型：

​	Directional：定向光源，无穷远处，模拟日光

​	Sky：天空光，使场景远处的天空照亮

​	Spot：聚光，锥形点向外发光

​	Point：点光源，向所有方向发射光

​	Rectangle：长方形光，从一个长方形面板发出光，模拟窗的光照等

天空光需要天空贴图来显示效果：

​	天空光->details面板->Light->SourceType：选择天空光依据的天空类型（场景中的天空/根据指定贴图）

​	天空光->details面板->Light->Cubemap：选择渲染天空光的天空贴图



### 光照贴图

用于减少实时运算的光照

build按键用于烘焙光照

build->LightingQuality：烘焙质量

WorldSettings->Lightmass：烘焙质量具体选项

在虚幻中创建光照贴图：

​	BuildSettings->GenerateLightmapUVs：勾选

​	GeneralSettings->LightMapResolution：光照贴图分辨率，越高质量越好，性能消耗越大

