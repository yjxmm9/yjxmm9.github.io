# Unity Notes

- [Unity | Documentation](https://docs.unity3d.com/Manual/index.html)
- Completed [Bilibili | Unity 10分钟快速入门 - 奇乐编程学院](https://www.bilibili.com/video/BV1PL4y1e7hy)
    - 讲的清楚但完全不够用 只是个简介
- Completed [Bilibili | 1h Unity入门 - 游戏谭](https://www.bilibili.com/video/BV1Yh411h7zk)
    - 讲的不清楚但勉强够用
- Completed [YouTube | Everything to know about the PARTICLE SYSTEM](https://www.youtube.com/watch?v=FEA1wTMJAR0)
    - 粒子效果介绍 就是一堆例子呈现在三维空间中的效果 想要很酷炫的话 你可以给粒子加上颜色、模型、物理、发光、时变等多种效果

---

- Mesh 三维物体的轮廓（网格）
    - 我的理解就是物体的位置 Mesh由多个三角形构成 一个三角形在空间中就表示一个位置上的小面 然后你要往这个面上面放什么颜色 放什么图片 确定这个三角形与背景的作用、最终到屏幕上加的后处理。总之这个小的三角形就是第一步 确定位置
- Material 渲染方式和参数（材质球）
- Shader 渲染的算法（着色器）
- Texture 图片 增加颜色细节（贴图）
    - 把Texture按照Mesh切分成多个三角形 然后一个一个放到Mesh的位置上 然后玩家看到的实际东西是一个三角形里面的Texture的图 而不是很多个三角形分别做的渲染
    - 举个例子：我的世界 一个方块的一个正方形面 就是两个三角形 你通过更换Texture来更换这个正方形面上的东西 而不是将这个正方形面切成很多个三角形 然后一个三角形一个三角形去渲染
- Normal 增加物体的凹凸细节等（法线等其他贴图）
- AnimationClip 让物体动起来（动画）

---

- Shader输入：Mesh Matrix4x4 Colors Values Textures
- VertexShader：只有两种作用：改变Vertex的位置 传数据给FragmentShader
- VertexShader对Mesh中的每一个顶点并行作用 在最终三角形Rasterizing之后 FragmentShader为每个像素上色
- FragmentShader：为什么叫Fragment而不是Pixel，这是因为Pixel往往是与你的屏幕上的像素一一对应的，比如我现在的电脑屏幕是2560×1600。但如果我看一个1080x1920的视频全屏，这时，1080p的视频的一个小色块对应一个Fragment，这些所有的Fragment可能还要经过一些变化才能成为真正屏幕上显示的Pixel（不一定对但大概是这个意思）