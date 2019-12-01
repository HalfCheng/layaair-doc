##Camera, la caméra layaair3d.

Les caméras de layaair peuvent être comprises comme des caméras prises au moment du tournage d 'un film ou d' une pièce de télévision pour capturer une image du monde en trois dimensions, qui est ensuite affichée à l 'écran et qui, si aucune caméra n' est disponible, ne montre pas l 'objet de la Mission.La caméra dans la scène peut également être placée dans le Sous - noeud de la scène, par exemple dans le modèle de personnage principal.

Dans le même temps, des caméras VR ont été ajoutées au moteur layaair 3D pour permettre aux concepteurs de développer des applications ou des jeux stéréoscopiques VR.

Bien entendu, la caméra a d 'autres propriétés plus importantes, dont les fonctions sont décrites ci - après.



###Exporter la caméra de l 'Unity

Moteur 1.7.10 avec le module d 'exportation Unity 1.5.0 après la publication, la caméra créée dans l' Unity peut être exportée!Et le fichier d 'exportation conserve des paramètres tels que la position, l' angle, la couleur d 'arrière - plan, le cisaillement de charge, le champ de vision, etc., de la caméra dans l' espace 3D.

Dans le même temps, comme le moteur layaair 3D supporte des caméras multiples, il est possible d 'installer et d' exporter plusieurs caméras dans l 'unité, pour les réglages visuels des caméras multiples, consultez la dernière section de ce cours consacrée à l' utilisation de caméras multiples.

Si une caméra est créée et exportée dans l 'unité, comment peut - on obtenir une caméra une fois que le fichier d' exportation est chargé dans le code?Ceci peut être obtenu par l 'index ou le nom du sous - noeud de la scène, auquel nous pouvons également effectuer des opérations de rotation mobile, de réglage de boîtes dans le ciel, d' ajout de scripts, etc.

Code:


```java

package {
	import laya.d3.core.Camera;
	import laya.d3.core.scene.Scene;
	import laya.display.Stage;
	import laya.utils.Handler;
	import laya.utils.Stat;
  
	public class LayaAir3D
	{
		public function LayaAir3D() 
		{
			//初始化引擎
			Laya3D.init(1000, 500,true);			
			//适配模式
			Laya.stage.scaleMode = Stage.SCALE_FULL;
			Laya.stage.screenMode = Stage.SCREEN_NONE;
			//开启统计信息
			Stat.show();			
			//预加载角色动画资源
			Laya.loader.create("monkey/monkey.ls",Handler.create(this,onSceneOK));
		}		
		
		private function onSceneOK():void
		{
			//添加3D场景
			var scene:Scene = Laya.loader.getRes("monkey/monkey.ls");
			Laya.stage.addChild(scene);  
          
         	//从场景中获取摄像机
            var camera:Camera = scene.getChildByName("Main Camera") as Camera;
          	//后续对摄像机的逻辑操作.......
        }
	}
}
			
```


À untiy, la caméra est désignée par défaut "Main Camera", de sorte que, dans le Code susmentionné, une caméra est obtenue par le biais de la méthode getchildbyname de Scene (« main camera») pour une logique ultérieure.Les développeurs peuvent également définir le nom de la caméra dans l 'unité.



###Camera Movement and rotation

La caméra est héritée de sprite3d et peut donc être Transform ée en 3D pour permettre à un public ou à un joueur d 'acquérir une expérience spatiale plus réelle en déplaçant l' attribut transformant en rotation dans une scène 3D par l 'intermédiaire d' une caractéristique Transform et d 'une vision Multi - angle.

Définit la rotation de l'appareil photo:


```java

//实例化一个相机，设置纵横比，0为自动匹配。0.1最近看到的距离，100最远看到的距离。
var camera:Camera = new Camera(0, 0.1, 100)
//移动相机，设置相机的向z轴移动3米。true代表是局部坐标，false是相对世界坐标。 
camera.transform.translate(new Vector3(0, 0, 3),false);
//加载到场景
scene.addChild(camera);
```

Réglage de la rotation d 'une caméra


```java

//欧拉角旋转相机。局部坐标，弧度制（false为角度制）。
camera.transform.rotate(new Vector3(0, 0, 3), true, true);
```




###Orthogonal Projection and perspective projection

En regardant le monde, on voit le monde avec un effet de vision "proche et lointain", dans un moteur 3D, pour mieux simuler le monde vu par les yeux, des caméras par défaut avec un effet de "projection visuelle".

![图片1](img/1.png)< br > projection à perspective par défaut

Mais il y a beaucoup de jeux, en particulier les jeux mixtes 2D et 3D avec un angle de 45 degrés oblique, dont l 'image ne peut pas avoir un effet de vision, alors il faut mettre en place une caméra en quadrature pour qu' elle ne produise pas d 'effet de vision de grande portée.


```javascript

	//正交投影属性设置
	camera.orthographicProjection = true;
	//正交垂直矩阵距离,控制3D物体远近与显示大小
	camera.orthographicVerticalSize = 7;
	//移动摄像机位置
	camera.transform.translate(new Vector3(0, 26.5, 45));
	//旋转摄像机角度
	camera.transform.rotate(new Vector3( -30, 0, 0), true, false);
```


![图片2](img/2.png)< br > projection orthogonale



###Camera cutting and Vision

**Coupe à distance**

La caméra peut également configurer des coupes à distance, ne visualiser que des modèles de scène entre des distances distantes, et les modèles extérieurs ne sont pas affichés par rendu.Son avantage le plus important est d 'améliorer les performances du jeu.

Lors de la création de la caméra, la fonction de configuration de la caméra est coupée par défaut à une distance de 0,3 m et à une distance de 1 000 m (fig. 1).L 'développeur peut paramétrer les paramètres dans la fonction de construction ou par l' intermédiaire des propriétés de la caméra.

![图片3](img/3.png)<br>（图3）




```javascript

	//创建摄像机时初始化裁剪(横纵比，近距裁剪，远距裁剪)
	var camera:Camera = new Camera( 0, 0.1, 100);
	//近距裁剪
	camera.nearPlane=0;
	//远距裁剪
	camera.farPlane=100;
```


En général, dans le jeu, nous utilisons le brouillard en même temps que la caméra de coupe, l 'effet du brouillard est quasiment invisible à distance, ce qui permet d' installer le cisaillement à distance et d 'améliorer les performances du jeu.

**Champ de vision**

Le champ de vision de la caméra est semblable à la distance de focalisation, et l 'ajustement des paramètres de visualisation permet de voir des variations de la portée de la scène et de la vision de près ou de loin.



  
```java

//设置相机的视野范围90度
camera.fieldOfView = 90;
  ```




###La caméra a capturé la cible.

Lors de la création d 'une caméra, il est souvent nécessaire d' ajuster la position de la caméra pour l 'aligner sur un objet tridimensionnel ou pour afficher une zone.Pour les débutants, la pensée spatiale n 'a pas encore pris l' habitude et il faudra beaucoup de temps pour ajuster la position.

La conversion 3D du moteur layaair - 3D fournit un procédé de lookat () permettant de capturer une cible et d 'ajuster automatiquement le point d' alignement de l 'objet 3D.La caméra peut également être utilisée pour ajuster nos vues.Code suivant

Lookat (Target observe le vecteur cible, up vers le vecteur supérieur, si islocal est local)


```java

//添加场景
var scene:Scene = new Scene();
Laya.stage.addChild(scene);
//添加自定义模型
var box:MeshSprite3D = scene.addChild(new MeshSprite3D(new BoxMesh(1,1,1))) as MeshSprite3D;
scene.addChild(box);
//添加摄像机
var camera:Camera = (scene.addChild(new Camera())) as Camera;
//摄像机捕捉模型目标
camera.transform.lookAt	(box.transform.position,new Vector3(0,-1,0));
```

Nous avons positionné le Haut de l 'appareil photo dans la direction 0,1,0, et le y de la caméra est négatif, et l' image est inversée sur l 'axe Y.Les débutants peuvent essayer davantage.

![图片4](img/4.png)<br > (fig. 4) capturer la cible



###Camera Background color and sky box

**Couleur de fond**

Dans une scène 3D, la couleur d 'arrière - plan est contrôlée par une caméra qui modifie la couleur d' arrière - plan de l 'espace 3D en définissant les attributs Clear de la caméra, les couleurs sont ajustées en fonction de la valeur du vecteur 3D vector Vector vector Vector vector Vector 3 (rouge, vert, Bleu), le moteur étant par défaut noir pur.


```java

	//设置背景颜色
	camera.clearColor = new Vector3(0.5,0.5,0.6);
```


**Sky box**

Dans la plupart des cas, il est nécessaire d 'exprimer des visions du ciel, telles que nuages bleus, crépuscules, étoiles, etc., dans le moteur layaair 3D, par l' ajout d 'une boîte à ciel (skybox) aux propriétés de la caméra.

Cependant, si la caméra utilise une projection orthogonale, la boîte ne sera pas à la hauteur de l 'effet recherché et les développeurs pourront essayer.

La cartouche du ciel est constituée d 'un modèle cubique et de six maquettes de matériaux qui peuvent être jointes sans soudure, un peu semblables à la carte panoramique 360.

Dans le code "skycube.ltc", le chemin de six cartes d 'autocollage est stocké en json.


```java

	//创建天空盒
	var skyBox:SkyBox=new SkyBox();
	//清除标记，使用天空（必须设置，否者无法显示天空）
	camera.clearFlag=BaseCamera.CLEARFLAG_SKY;
	//绑定天空盒对象到摄像机
	camera.sky=skyBox;
	//为天空盒加载贴图文件
	skyBox.textureCube=TextureCube.load("skyBox/skyCube.ltc");
```


![图片5](img/5.png)<br > (Figure 5) Utiliser des boîtes dans le ciel



###Utilisation de caméras multiples

Dans la même scène, plusieurs caméras peuvent être utilisées et, lorsqu 'elles sont chargées dans la scène, elles produisent leurs propres images de jeu.Dans les jeux que nous avons rencontrés auparavant, comme deux caméras 3D, l 'un à l' écran gauche et l 'autre à l' écran droit, le jeu est très riche.

Toutefois, les inconvénients de plusieurs caméras sont une forte consommation, le nombre de triangles de modèles et le nombre de drawcall vont doubler, et plusieurs caméras supplémentaires risquent de doubler leurs pertes de performances, ce qui oblige les concepteurs à prendre en considération, le cas échéant.

L 'affichage d' une scène 3D n 'est pas très différent de celui d' un jeu 2D, il est contrôlé principalement par l 'entrée de la caméra (viewport) qui permet la segmentation de l' écran.

Dans l 'exemple suivant, nous chargeons une scène 3D et séparons la vue gauche et droite par viewport, comme suit:


```java

package
{
	import laya.d3.core.Camera;
	import laya.d3.core.scene.Scene;
	import laya.d3.math.Vector3;
	import laya.d3.math.Viewport;
	import laya.display.Stage;
	import laya.utils.Handler;
	import laya.utils.Stat;

	public class LayaAir3D_MultiCamera
	{
		public function LayaAir3D_MultiCamera()
		{
			//初始化引擎
			Laya3D.init(1280, 720,true);
			//适配模式
			Laya.stage.scaleMode = Stage.SCALE_EXACTFIT;
			Laya.stage.screenMode = Stage.SCREEN_NONE;
			//开启统计信息
			Stat.show();			
			//加载3D资源
			Laya.loader.create("LayaScene_loveScene/loveScene.ls",Handler.create(this,on3DComplete));
		}
		
		private function on3DComplete():void
		{
			//创建场景
			var scene:Scene=Scene.load("LayaScene_loveScene/loveScene.ls");
			Laya.stage.addChild(scene);
			
			//创建摄像机1添加到场景
			var camera1:Camera=new Camera();
			scene.addChild(camera1);
			//摄像机1添加控制脚本
			camera1.addComponent(CameraMoveScript);
			//修改摄像机1位置及角度
			camera1.transform.translate(new Vector3(0,2,8),true);
			camera1.transform.rotate(new Vector3(-23,0,0),true,false);
			//设置视口为左半屏
			camera1.viewport=new Viewport(0,0,640,720);
			
			//创建摄像机2添加到场景
			var camera2:Camera=new Camera();
			scene.addChild(camera2);
			//修改摄像机2位置及角度
			camera2.transform.rotate(new Vector3(-45,0,0),false,false);
			camera2.transform.translate(new Vector3(0,0,25),true);
			//设置视口为右半屏
			camera2.viewport=new Viewport(640,0,640,720);
		}
	}
}
```


Compiler et exécuter le code ci - dessus, l 'effet de fonctionnement est le graphique 6.Les développeurs peuvent également tester le nombre de drawcall et de triangles sous une seule caméra.

![图片6](img/6.png)<br > (fig. 6)

