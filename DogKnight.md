# 升级URP渲染

Window菜单  ->  包管理器  ->  搜索Universal RP

![image-20230419205537298](C:\Users\Pluto\AppData\Roaming\Typora\typora-user-images\image-20230419205537298.png)

在 Project 窗口中单击右键，然后选择 **Create > Rendering > URP Asset**

![image-20230419211259873](C:\Users\Pluto\AppData\Roaming\Typora\typora-user-images\image-20230419211259873.png)



edit -> 项目设置  ->  Graphics（图形）

![image-20230419211427039](C:\Users\Pluto\AppData\Roaming\Typora\typora-user-images\image-20230419211427039.png)

None那里放入刚刚创建的Render

接下来前往Quality  那里的None也选择Render



## 项目升级渲染管线

edit  ->  Render Pipeline  ->  Universal Render Pipline  ->  项目升级URP

# 添加素材

天空盒子：3D  可以切换  就是天空  

切换方法：Window Rendering lighting environment 更换skyBox

影子相关：渲染管线设置  shadows  例如Max Distance    Cascade Count渲染级数，在范围内实际渲染，范围外虚渲染

ReSoulution：渲染的性能

抗锯齿

soft shadows

## 光照设置

还是找到lighting ->  New Lighting Settings

改变光的类型（一般为baked indirect）  然后渲染一般使用GPU  然后右下角生成渲染

此时地面变成skyBox颜色，因为environment中光源就是skyBox，可以改为color为原本颜色



###  素材放置

**自动吸附： 按住v键，鼠标拖动**

**改变摄像机位置，ctrl shift f（改变繁体，取消即可）**