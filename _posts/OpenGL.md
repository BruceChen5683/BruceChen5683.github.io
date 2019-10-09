#### android Open GL ES  https://blog.csdn.net/junzia/article/details/52793354
- OpenGL ES 是适用于嵌入式设备的OpenGL规范。
- **SurfaceView在View的基础上是创建了独立的Surface，拥有SurfaceHolder来管理它的Surface，渲染的工作可以不再在主线程中做了** 
- GLSurfaceView 
```
GLSurfaceView继承了SurfaceView，实现了SurfaceHolder.Callback2接口。 可以通过SurfaceHolder得到Canvas，在单独的线程中，利用Canvas绘制需要显示的内容，然后更新到Surface上。 
```
- GLSurfaceView 渲染模式 OpenGl ES关于渲染方式有以下两种：**RENDERMODE_CONTINUOUSLY和RENDERMODE_WHEN_DIRTY。**
```
默认渲染方式为RENDERMODE_CONTINUOUSLY，当设置为RENDERMODE_CONTINUOUSLY时渲染器会不停地渲染场景，
当设置为RENDERMODE_WHEN_DIRTY时只有在创建和调用requestRender()时才会刷新。

一般设置为RENDERMODE_WHEN_DIRTY方式，这样不会让CPU一直处于高速运转状态，提高手机电池使用时间和软件整体性能。

你可以通过设置GLSurfaceView.RENDERMODE_WHEN_DIRTY来让你的GLSurfaceView监听到数据变化的时候再去刷新，即修改GLSurfaceView的渲染模式。
这个设置可以防止重绘GLSurfaceView，直到你调用了requestRender()，这个设置在默写层面上来说，对你的APP是更有好处的。

```

#### GLSL**(http://www.cnblogs.com/renhui/p/8126121.html)**
-  类型转换
```
GLSL的类型转换与C不同。在GLSL中类型不可以自动提升，比如float a=1;就是一种错误的写法，必须严格的写成float a=1.0，也不可以强制转换，即float a=(float)1;也是错误的写法，但是可以用内置函数来进行转换，如float a=float(1);还有float a=float(true);（true为1.0，false为0.0）等，值得注意的是，低精度的int不能转换为低精度的float。
```

- 限定符
```
attritude：一般用于各个顶点各不相同的量。如顶点颜色、坐标等。
uniform：一般用于对于3D物体中所有顶点都相同的量。比如光源位置，统一变换矩阵等。
varying：表示易变量，一般用于顶点着色器传递到片元着色器的量。
const：常量。
限定符与java限定符类似，放在变量类型之前，并且只能用于全局变量。在GLSL中，没有默认限定符一说。
```

- 图形的绘制
```
前面提到OpenGL ES2.0的世界里面只有点、线、三角形，其它更为复杂的几何形状都是由三角形构成的。包括正方形、圆形、正方体、球体等。但是其他更为复杂的物体，我们不可能都自己去用三角形构建，这个时候就需要通过加载利用其他软件（比如3DMax）构建的3D模型。
```

#### ubuntu 
1. ubuntu 16.04 安装openGl
```
sudo apt-get install build-essential libgl1-mesa-dev
sudo apt-get install freeglut3-dev
sudo apt-get install libglew-dev libsdl2-dev libsdl2-image-dev libglm-dev libfreetype6-dev
```
