## LayaAir3D之模型

### 模型概述

3D模型是指通过三维软件按照物体的结构建模形成的3D立体对象。目前LayaAir 3D引擎中包括了两种模型显示类型，一为普通模型**MeshSprite3D**，二为蒙皮动画模型**SkinnedMeshSprite3D**。

区别是蒙皮动画模型是指在制作时加入了蒙皮与骨骼动画的模型，常用于有动画的角色。而普通模型是指未有动画的场景景观模型等。

它们都包括了模型网格与材质两部分。

**模型网格(Mesh)：**

模型网格是由点、线、面组成的三维数据，LayaAir引擎中有专门的Mesh网格数据类，将它赋予3D模型显示对象MeshSprite3D或SkinnedMeshSprite3D后就可以在场景中显示出来。

目前3D制作软件较多，最主流的是3d max与maya软件。3D模型的数据格式也较多，如FBX、3DS、OBJ等。

LayaAir引擎提供了模型导出工具FBXTools与unity3D导出插件，用于生成layaAir所需要的3D数据格式。建议使用unity导出插件，FBXTools工具以后将不会更新。

**材质(Material)：**

材质说明我们将在独立的章节介绍，在本章节中暂不说明。



### 创建引擎自带的基础模型

 在快速开启3D之旅的课程中，我们已用到了BoxMesh盒子模型，本节课中介绍LayaAir引擎提供的其他SphereMesh、CylinderMesh基础模型数据，我们依次创建它们，并通过transform属性去移动它们的位置，具体代码如下：。

创建时，需注意的是，加载到场景中的引擎自带模型，轴心点在模型正中心，因此我们是以模型中心点为参考进行移动、旋转、缩放。加载到场景中时，模型默认会放置到场景的世界座标原点上，与2D类似。

```java
package {
	import laya.d3.core.Camera;
	import laya.d3.core.MeshSprite3D;
	import laya.d3.core.Sprite3D;
	import laya.d3.core.light.DirectionLight;
	import laya.d3.math.Vector3;
	import laya.d3.math.Vector4;
	import laya.d3.resource.models.BoxMesh;
	import laya.display.Stage;
	import laya.utils.Stat;
	import laya.d3.core.scene.Scene3D;
	import laya.d3.core.material.BlinnPhongMaterial;
	import laya.webgl.resource.Texture2D;
	import laya.display.Scene;
	import laya.d3.resource.models.SphereMesh;
	import laya.d3.core.material.PBRStandardMaterial;
	import laya.d3.resource.models.CylinderMesh;
	import laya.utils.Handler;
	public class LayaAir3D {
		
		public function LayaAir3D() {

			//初始化引擎
			Laya3D.init(0, 0);
			
			//适配模式
			Laya.stage.scaleMode = Stage.SCALE_FULL;
			Laya.stage.screenMode = Stage.SCREEN_NONE;

			//开启统计信息
			Stat.show();
			var scene:Scene3D = Laya.stage.addChild(new Scene3D())as Scene3D;

			//创建摄像机（纵横比，进距裁剪，远距裁剪）------
			var camera:Camera = new Camera(0,0.1,1000);
			//加载到场景
			scene.addChild(camera);
			//移动摄像机的位置
			camera.transform.position = new Vector3(0,3,10);
			//旋转摄像机角度
			camera.transform.rotate(new Vector3(-15,0,0),true,false);
			//加入摄影机移动控制脚本
			//camera.addComponent();

			//创建方向光
			var light:DirectionLight = scene.addChild(new DirectionLight())as DirectionLight;
			//移动灯光位置
			light.transform.translate(new Vector3(0,2,5));
			//调整灯光方向
			light.transform.worldMatrix.setForward(new Vector3(0.15,-1.0,0.0));
			//设置灯光颜色
			light.color = new Vector3(0.3,0.3,0.3);
			//设置灯光环境色
			scene.ambientColor = new Vector3(1,1,1);

			//创建模型
			//创建盒子模型（参数为：长，宽，高，单位：米）
			var boxMesh:BoxMesh = new BoxMesh(2,2,2);
			//创建模型显示对象
			var box3D:MeshSprite3D = new MeshSprite3D(boxMesh);
			scene.addChild(box3D);

			//创建球体模型（参数为：半径，水平层数，垂直层数）
			var sphereMesh:SphereMesh = new SphereMesh(1,20,20);
			//创建模型显示对象
			var sphere3D:MeshSprite3D = new MeshSprite3D(sphereMesh);
			//x轴上移动-3米（世界坐标 向左）
			sphere3D.transform.translate(new Vector3(-3,0,0),false);
			scene.addChild(sphere3D);

			//创建圆柱体模型（参数为：半径，高，圆截面线段数）
			var cylinderMesh:CylinderMesh = new CylinderMesh(1,2,20);
			//创建模型显示对象
			var cylinder3D:MeshSprite3D = new MeshSprite3D(cylinderMesh);
			cylinder3D.transform.translate(new Vector3(3,0,0),false);
			scene.addChild(cylinder3D);
			//创建材质
			var material:PBRStandardMaterial = new PBRStandardMaterial();
			box3D.meshRenderer.material = material;	
			sphere3D.meshRenderer.material = material;
			cylinder3D.meshRenderer.material = material;
		}		 
	}
}
```

上面的代码中，创建了摄像机与灯光，并添加了三种基本几何体模型，它们使用了最基本的默认材质。显示效果如图1。

![图片1](img/1.png)<br>（图1）



### 创建三维软件生成的模型

上述三种基本模型主要用于开发者学习测试。游戏中的模型大都是三维软件制作后，导入unity编辑器中编辑拼接，再用layaAir导出工具转化产生，然后通过3D场景或模型显示类加载使用。

在此我们再次来说明一下导出的资源类别与文件使用方法。

导出的文件夹中，包括的资源较多（图2），有场景、3D模型容器、3D模型、3D材质等解析文件，还有光照贴图、材质贴图等数据文件。

![图片2](img/2.png)<br>（图2）

**loveScene文件夹**是在untiy中创建光照贴图后产生的文件夹，与在untiy中创建的场景名相同，光照贴图在场景Scene篇已有介绍。

**Materials文件夹**是在unity中导入FBX模型时创建材质球的文件夹，导出后的资源为对应的LayaAir材质数据解析文件，文件中存储着材质的渲染模式、贴图资源路径、材质的各种光色属性等。

**Texture文件夹**是在unity中创建的存放贴图的文件夹，其中资源为材质的贴图文件，是一系列的图片文件，在LayaAir引擎中我们使用jpg或png格式的图片，可使用导出工具把其他格式图片自动转化jpg或png，请开发者们一定注意。



#### *.ls格式Scene数据文件

导出的场景Scene类型数据文件，在之前的课程中我们已有讲解，在此不多作说明。




#### *.lh格式Sprite3D数据文件

导出的3D显示对象容器Spirte3D类型数据文件，JSON格式编码，是unity3D中layaAir导出插件选择导出”Sprite3D“类别生成，内部存储比*.ls格式少了光照贴图，其他全部相同。

“*.lh” 格式加载与场景加载方法类似，由异步加载Sprite3D.load()或预加载Laya.loader.create()方法加载，参考代码：

```java
......
//添加3D场景
var scene:Scene3D = new Scene3D();
Laya.stage.addChild(scene);

//方法一：直接异步加载
//Sprite3D.load("res/room.lh",Handler.create(this,function(sp:Sprite3D):void{
//	var sprite3D:Sprite3D = scene.addChild(sp)as Sprite3D;
//}));

//方法二：预加载
Laya.loader.create("res/room.lh",Handler.create(this,function():void{
	var sprite3D:Sprite3D = Laya.loader.getRes("res/room.lh");
	scene.addChild(sprite3D);
}));
```



#### *.lm格式数据文件

无论是导出”Scene“文件或”Sprite3D“文件类型，在导出的资源文件夹中都包含了系列*.lm格式文件，本项目中model文件夹为unity中开发者自建的存储FBX模型的文件夹，如图2，在导出时生成了对应的文件夹和.lm资源文件。

![图片3](img/3.png)<br>（图3）

"*.lm"文件是模型数据文件，可以生成MeshSprite3D或SkinnedMeshSprite3D类型显示对象的网格数据Mesh，包含了模型网格的顶点位置、法线、顶点色、顶点UV等信息。

通过异步加载MeshSprite.load()或预加载Laya.loader.create()方法加载，参考代码如下：

```java
......
//添加3D场景
var scene:Scene3D = new Scene3D();
Laya.stage.addChild(scene);

//方法一：直接异步加载
//Mesh.load("LayaScene_01/Assets/model/loveScene_jianzhu.lm",Handler.create(this,function(m:*):void{
	//var meshSprite3D:MeshSprite3D = new MeshSprite3D(m);
	//scene.addChild(meshSprite3D);
//}));

//方法二：预加载，.lm默认创建为Mesh类型
Laya.loader.create("LayaScene_01/Assets/model/loveScene_jianzhu.lm",Handler.create(this,function():void{		                         varmesh:Mesh=Laya.loader.getRes("LayaScene_01/Assets/model/loveScene_jianzhu.lm");
	var meshSprite3D:MeshSprite3D = new MeshSprite3D(mesh);
	scene.addChild(meshSprite3D);
}));
```

用上述的两种方法都可以在游戏画面中显示出模型，材质贴图引擎也会自动加载到模型上。在项目中我们可以根据情况使用上述两种方法，固定场景我们可以使用.ls格式加载，而活动的物品可以使用.ls或.lm方式加载。 



### 获取子对象模型及网格

3D模型在有时候会由多个子模型对象构成，例如场景模型.ls，基本都是由多个物体模型与材质构成，外层是Sprite3D容器，内部才是真正的模型MeshSprite3D或SkinnedMeshSprite3D。并且还可能会有多个层次嵌套。

#### 获取子对象模型

在编写游戏逻辑时，有的模型需要被修改，或者是切换与删除模型、或者是给模型加组件、或者是获取模型上的动画组件及修改模型的材质等。这都需要从加载的模型中去获取子对象，我们可以通过**getChildAt()、getChildByName()**方法去获取子对象，这与2D引擎获取子对象方法一样。

下面我们来加载一个卡车模型的.lh文件，然后获取它的子对象。在获取子对象之前，建议打开.lh文件查看模型的父子层级关系，因为在制作模型时，我们也不能确定模型是由多少个子对象模型构成，及它们的命名规则。

tips：在3ds max中建模时，建议对模型的子对象取名，并且制定项目的资源命名规则，不要用默认的模型名称。

下例加载从unity导出的卡车truck.lh，打开后通过JSON结构可以看到，外层是一个Sprite3D容器（相当于unity的场景），内部又是一个Sprtie3D容器（相当于unity场景中的卡车），卡车容器中是两个子对象模型MeshSprite3D（车头与车身模型）。因此我们需要两次getChildAt()方式才能获取到模型MeshSprite3D。

获取子对象时还应注意一个问题，就是模型与材质未加载完成，是无法获取子对象的，因此需要资源预加载，或异步加载时进行完成事件监听。

```java
//加载导出的卡车模型
Sprite3D.load("LayaScene_truck.lh",Handler.create(this,function(s:*):void{
	var truck3D:Sprite3D = s as Sprite3D;
    //获取模型（查看.lh文件，有两个子对象模型，一为车头“head”，一为车身"body",暂取其中一个模型）
	var meshSprite3D:MeshSprite3D = truck3D.getChildAt(0).getChildAt(0)as MeshSprite3D;
     //输出模型的名字
	trace(meshSprite3D.name);
}))
```

编译上例代码，我们可以看到模型显示了（图4），在浏览器下按F12打开控制台，我们可以看到输出了模型的名字为“body”，说明模型获取成功。

![图片4](img/4.png)<br>（图4） 



#### 获取模型网格Mesh

在游戏中，我们经常打造角色换装系统，有时是换模型，有时是换贴图，有时候两者都换。因为材质贴图部分在后续章节中才讲解，因此本章节中我们只介绍更换模型网格的方法。

模型MeshSprite3D或SkinnedMeshSprite3D中有**meshFilter**属性，它是一个网格过滤器类实例，这个属性中的**sharedMesh**就是模型的网格，可以对它进行重新创建更换及销毁。

查看以下示例，当加载完卡车模型2秒后，我们创建新的汽车头网格对象更换原有的车身网格，效果如（图4）。

```java
......
var truck3D:Sprite3D;
var meshSprite3D:MeshSprite3D;
//加载导出的卡车模型
Sprite3D.load("LayaScene_truck.lh",Handler.create(this,function(s:*):void{
	truck3D = s;
    //获取模型（查看.lh文件，有两个子对象模型，一为车头“head”，一为车身"body",暂取其中一个模型）
	meshSprite3D = truck3D.getChildAt(0).getChildAt(0)as MeshSprite3D;
     //输出模型的名字
	trace(meshSprite3D.name);
 	Laya.timer.once(2000,this,onTimerOnce);
}))

//2秒后更换模型网格
private function onTimerOnce():void
{
  //创建模型网格并更换原始网格
  Mesh.load("LayaScene_truck/Assets/truck-head.lm",Handler.create(this,function(m:*):void{
		meshSprite3D.meshFilter.sharedMesh = m;
 		 //因使用了卡车头网格，位置会重合，因此进行位置移动
 		 meshSprite3D.transform.translate(new Vector3(0,0,-8));
   }))
}
```

![图片5](img/5.gif)<br>（图5） 