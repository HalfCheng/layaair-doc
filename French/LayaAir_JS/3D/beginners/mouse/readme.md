# LayaAir3D之鼠标交互

###Aperçu de l 'interaction de la souris

Dans le moteur layaair2d, l 'objet 2D affiche des événements de souris pour nous permettre d' élaborer une logique simple et pratique.Cette fonction n 'a pas été réalisée dans le moteur layaair 3D, l' espace 3D est plus complexe et montre que l 'objet a une relation lointaine, superposée, découpée, paternelle, etc., et que l' espace continue de changer.Par conséquent, les moteurs 3D ont utilisé des impacts, des couches et des rayons physiques pour la détection et l 'information sur les impacts de souris.



###Collider

Le collisionneur est un ensemble physique qui peut être ajouté à un objet d 'affichage 3D, principalement pour la détection de collision d' un objet dans l 'espace 3D, selon la forme de l' objet affiché 3D et selon différents types.

Il y a trois types de collisionneurs que le moteur layaair3d appuie actuellement.**Spherecollider**Oui.**Boxcollider**Oui.**Dispositif de collision de grille**".De...**Précision de détection de collision**Et**Consommation**De bas en haut, spherecollider - boxcollider - meshcollider; un collimateur approprié peut être sélectionné en fonction des besoins de développement du jeu.

Le procédé d 'affichage 3D d' un code d 'objet pour ajouter un composant de collision est le suivant (version 1.7.12 du moteur) et il est conseillé à l' concepteur de ne pas essayer l 'ajout de code, ce qui est plus difficile, en ajoutant directement l' ensemble collision à l 'unité.

Le collisionneur doit être ajouté à l 'objet d' affichage du type meshsprite3d et ne peut pas être ajouté à l 'objet sprite3d, faute de quoi il ne fonctionne pas.


```typescript

/**
* 给3D精灵添加碰撞器组件
* BoxCollider    : 盒型碰撞器
* SphereCollider : 球型碰撞器
* MeshCollider   : 网格碰撞器
*/
//添加Mesh碰撞器组件并获取
var meshCollider=meshSprite3d1.addComponent(MeshCollider);
//设置mesh碰撞器网格属性（否则无法被检测）
meshCollider.mesh=meshSprite3d1.meshFilter.sharedMesh;

//添加球形碰撞器组件并获取
var sphereCollider = meshSprite3d2.addComponent(SphereCollider);
//设置球形碰撞器中心位置
sphereCollider.center = meshSprite3d2.meshFilter.sharedMesh.boundingSphere.center.clone();
//设置球形碰撞器半径
sphereCollider.radius = meshSprite3d2.meshFilter.sharedMesh.boundingSphere.radius;

//添加盒形碰撞器
var boxCollider =meshSprite3d3.addComponent(BoxCollider);
boxCollider.setFromBoundBox(meshSprite3d3.meshFilter.sharedMesh.boundingBox);
```


A partir de la version 1.7.12 du moteur et de la version 1.7.0 du module d 'Export, Collider ajouté au modèle 3D dans l' Unity peut être exporté et le moteur peut charger automatiquement la création.Toutefois, pour le moment, l 'exportation de meshcollider n' est pas appuyée et cette fonction sera affinée dans une version ultérieure.

Après l 'ajout de boxcollider et spherecollider au modèle dans l' Unity, la taille de la boîte de collision ou de la boule de collision peut également être réglée en fonction de la demande, la boîte de collision peut être plus petite ou plus grande que le modèle réel et la position peut être modifiée pour faciliter le traitement logique des développeurs.

Tips: dans l 'éditeur Unity, un objet 3D peut supporter une pluralité de collisions, mais le connecteur d' exportation layaair (version 1.7.0) ne prend actuellement en charge que l 'exportation du premier collisionneur, qui est porté à l' attention des concepteurs.Si l 'on souhaite ajouter des impacts multiples sur le modèle, on peut les décomposer en plusieurs sous - modèles de grille lors de la fabrication du modèle et ajouter un collisionneur respectif au sous - modèle de grille pour la détection.Dans la version suivante 1.7.13, nous exporterons une pluralité de collimateurs pour les objets 3D sans sous - grille.

###Layer

Il y a 32 étages dans la scène par défaut, vous pouvez choisir de laisser les elfes 3D dans n 'importe quelle couche.Sur la caméra, la caméra peut être découpée en fonction du niveau;**Pour la détection de collision, il est possible de contrôler la couche de collision et non la couche de collision.**".

Le procédé de désignation d 'une couche elfe 3D est le suivant:


```typescript

//指定3D精灵的层
meshSprite3d1.layer = Layer.getLayerByNumber(10);
meshSprite3d2.layer = Layer.getLayerByNumber(13);
```




###Ray.

Le rayon est un type de données qui n 'est pas un objet d' affichage, il a des propriétés originelles et directionnelles.

Dans le jeu, comme l 'espace de visualisation change souvent, et pour simuler la position de la souris dans l' espace 3D, le moteur layaair3d propose un procédé de création de rayons par caméra camera qui produit un rayon perpendiculaire à l 'écran.

Les procédés de création de rayons par caméra sont les suivants:


```typescript

//射线初始化（必须初始化）
var ray = new Laya.Ray(Laya.Vector3.ZERO,Laya.Vector3.ZERO);
//获取鼠标在屏幕空间位置
var point = new Laya.Vector2();
point.elements[0] = Laya.stage.mouseX;
point.elements[1] = Laya.stage.mouseY;
//详设计产生射线方法，通过2D坐标获取与屏幕垂直的一条射线
camera.viewportPointToRay(point, ray);
```




###Détection de rayonnement physique

Une fois que nous avons créé un collisionneur pour les objets d 'affichage 3D dans la scène, que nous avons installé une couche (par défaut au niveau 0) et que nous avons créé un rayon, la détection de l' intersection peut être effectuée au moyen d 'une collision avec un rayon physique, et l' développeur peut faire ses propres jugements logiques en fonction de la demande, tels que la capture, la sélection, la création, etc.

La détection physique des rayons ionisants nous fournit deux méthodes: la détection de l 'acquisition d' informations sur le premier collisionneur qui a eu lieu et la détection de l 'acquisition d' informations sur tous les collisions qui se sont produites.

![1](img/1.png)(Figure 1) < / BR >



###Informations sur les collisions

Les informations de collision détectées par les rayons doivent être initialisées avant la détection et, si les rayons sont en contact avec l 'objet d' affichage 3D, on peut obtenir des informations telles que les propriétés raycasthit des informations de collision, la position spatiale de l 'intersection, le Sommet triangulaire de l' intersection, etc.

Le sprite3d est un objet d 'affichage tridimensionnel entrelacé et, s' il n' y a pas d 'objet intersection, null.

Position is Space Position of the intersection between the Ray and the model.

Les propriétés triangles sont des ensembles de positions de sommets triangulaires croisés, à condition, bien entendu, que le type de collimateur soit Mesh Collider, faute de quoi la propriété de position de sommet est zéro.



###Capture de souris

Sur la base des concepts et méthodes ci - dessus, nous allons produire un exemple de la capture de la souris en procédant comme suit:

Créer plusieurs objets 3D dans une scène d 'Unity, par exemple trois voitures, à l' aide d 'une fiche d' exportation.

Des exemples d 'établissement de scènes scènes, et des types de commande de scripts scénographiques, et l' ajout de scripts à l 'aide d' addscript () lors du chargement de scènes.

Script script`_start()`Procédé, couche de réglage, création de rayons, informations de collision, etc., et ajout d 'un collisionneur pour un article 3D dans une scène.

Procédé de post - traitement de rendu de script réécrit`_postRenderUpdate()`Dans ce procédé, une ligne de référence vectorielle peut être observée sur la base d 'un point d' origine de rayons et l 'on peut déterminer si le rayon est intersection avec l' article 3D.

Mise à jour de script`_update()`, mais la ligne de référence de la peinture ne voit pas la ligne de référence de la position du clic de la souris après le modèle.`_postRenderUpdate()`Après avoir rendu la scène, puis dessiner la ligne de référence vectorielle.

Ajoutez l 'événement de clic de la souris et si vous cliquez sur la souris et entrez avec l' article 3D, nous laisserons l 'article 3D disparaître et suggérerons d' obtenir des informations.

Les codes principaux sont les suivants:


```typescript

var LayaAir3D = (function () 
{
    function LayaAir3D() 
    {

        //初始化引擎
        Laya3D.init(1000, 500,true);
        
        //适配模式
        Laya.stage.scaleMode = Laya.Stage.SCALE_FULL;
        Laya.stage.screenMode = Laya.Stage.SCREEN_NONE;
        
        //加载3D资源
        Laya.loader.create(["LayaScene_collider3D/collider3D.ls",
                        "LayaScene_truck/truck.lh",
                        "LayaScene_box/box.lh"],Laya.Handler.create(this,onComplete));
        
        //创建信息提示框
        LayaAir3D.txt=new Laya.Text();
        LayaAir3D.txt.text="还未获得汽车！！";
        LayaAir3D.txt.color="#ff0000";
        LayaAir3D.txt.bold=true;
        LayaAir3D.txt.fontSize=30;
        LayaAir3D.txt.pos(100,50);
        Laya.stage.addChild(LayaAir3D.txt);			
		
		function onComplete()
		{
			//添加3D场景
			var scene= Laya.loader.getRes("LayaScene_collider3D/collider3D.ls");
			Laya.stage.addChild(scene);
			//为场景添加控制脚本			
			scene.addScript(SceneScript);
		}
    }
    return LayaAir3D;
} ());

LayaAir3D();
```


Les codes scénescript de la catégorie de scripts sont les suivants:

* * Tips: après la version 1.7.10, des procédés tels que la mise à jour de la scène elle - même et le traitement post - rendu de laterender () ont été supprimés, mais la scène a accru la fonction de commande de l 'ensemble script, ce qui permet d' obtenir un dessin de la ligne de référence de la souris par un procédé d 'exécution finale de rendu dans l' ensemble script.* *.


```typescript

var SceneScript = (function(_super)
{
    function SceneScript()
    {
        SceneScript.super(this);
    }
    Laya.class(SceneScript,"SceneScript",_super);

    var scene;
    /**3D摄像机**/
    var camera;
    /**用于鼠标检测的射线**/
    var ray;
    /**画矢量线的3D显示对象**/
    var phasorSprite3D;
    /**碰撞信息**/
    var rayCastHit;	
    
    /**鼠标点击创建的3D对象**/
    var box;
    /***获得的物品***/
    var nameArray=[];

     /**
     * 覆写3D对象加载组件时执行的方法
     * @param owner 加载此组件的3D对象
     */	
     SceneScript.prototype._load=function(owner)
    {
        //获取脚本所属对象
        scene=owner;
    }

    /*初始化场景（摄像机、碰撞相关对象、添加碰撞器等）*/
    SceneScript.prototype._start = function(state)
    {
        //创建摄像机（纵横比，近距裁剪，远距裁剪）
        camera = new Laya.Camera(0,0.1,1000);
        camera.transform.position = new Laya.Vector3(1,7,10);
        camera.transform.rotate(new Laya.Vector3(-30,0,0),false,false);
        //加载到场景
        scene.addChild(camera);
        //加入摄像机移动控制脚本
        // this.camera.addComponent(CameraMoveScript);

        //创建一条射线
        ray = new Laya.Ray(new Laya.Vector3(),new Laya.Vector3());
        //创建矢量3D精灵
        phasorSprite3D = new Laya.PhasorSpriter3D();
        //创建碰撞信息
        rayCastHit = new Laya.RaycastHit();
        //为场景中3D对象添加组件
        for(var i = scene.numChildren-1;i>-1;i--)
        {
            //添加网格型碰撞器组件
            var boxCollider=meshSprite3D.addComponent(Laya.BoxCollider);
            //为盒形碰撞器设置盒子大小（否则没有尺寸，无法被射线检测）
            boxCollider.setFromBoundBox(meshSprite3D.meshFilter.sharedMesh.boundingBox);
        }            
        //鼠标点击事件回调
        Laya.stage.on(Laya.Event.MOUSE_DOWN,this,onMouseDown);
    }

    /****覆盖场景渲染后更新方法（相当于场景渲染完成后的帧循环）****/
    SceneScript.prototype._postRenderUpdate = function(state)
    {
        //画参考线、时行碰撞检测
        //根据鼠标屏幕2D座标修改生成射线数据 
        camera.viewportPointToRay(new Laya.Vector2(Laya.stage.mouseX,Laya.stage.mouseY),ray);        
        //射线检测，最近物体碰撞器信息，最大检测距离为300米，默认检测第0层
        Laya.Physics.rayCast(ray,rayCastHit,300);            
        //摄像机位置
        var position = new Laya.Vector3(camera.position.x, 0, camera.position.z);
        //开始绘制矢量3D精灵，类型为线型
        phasorSprite3D.begin(Laya.WebGLContext.LINES, camera);
        //根据射线的原点绘制参考直线（为了观察方便而绘制，但矢量线并不是射线真正的路径）
        phasorSprite3D.line(ray.origin, new Laya.Vector4(1,0,0,1), position , new Laya.Vector4(1,0,0,1));    
        //结束绘制
        phasorSprite3D.end();
    }

    /**
     * 鼠标点击拾取
     */
    function onMouseDown()
    {
        //如果碰撞信息中的模型不为空,删除模型
        if(rayCastHit.sprite3D)
        {
            //从场景中移除模型
            scene.removeChild(rayCastHit.sprite3D);
            //将模型名字存入数组
            nameArray.push(rayCastHit.sprite3D.name);
            //文件提示信息
            LayaAir3D.txt.text = "你获得了汽车"+rayCastHit.sprite3D.name+"!，现有的汽车为："+nameArray;
            //销毁物体(如不销毁还能被检测)
            rayCastHit.sprite3D.destroy();
        }
    }
    return SceneScript;
})(Laya.Script);
```


Le Code de présentation compilé permet d 'obtenir l' effet suivant (fig. 2), de cliquer sur la souris pour obtenir le véhicule et d 'enlever le modèle du véhicule de la scène.

![2](img/2.gif)(Figure 2) < / BR >



###Créer un objet

Dans les jeux, nous utilisons aussi souvent la souris pour contrôler la mise en place d 'objets de jeu, tels que des jeux de type construction au sol, rôle, accessoires, etc.

La souris pose un objet de la même manière que le procédé de capture d 'un objet, ce qui nécessite également l' utilisation d 'éléments et de procédés 3D tels que des collisions, des rayons, la détection des rayons, des informations de collision, etc.

Lors de la création de l 'article, après avoir cliqué sur les rayons du modèle, nous pouvons obtenir la position de l' objet par l 'information de collision raycasthit.position, puis placer l' article créé ici.En outre, nous utilisons le clonage pour créer des objets, et les développeurs en font attention.

Dans l 'exemple, nous avons utilisé un boxcollider de type boxcollider, dans l' exemple de la création d 'un collimateur de grille meshcollider, qui est plus précis et permet d' obtenir des sommets triangulaires d 'interconnexion sur le modèle en utilisant raycasthit.trianglepositions, que nous pouvons dessiner pour l' observation en fonction de la position de pointe!

Modifier le Code principal comme suit:

Créer un modèle de camion et ajouter un ensemble Collisionneur de grille à la carrosserie du camion.


```typescript

var LayaAir3D = (function () 
{
    function LayaAir3D() 
    {
        //初始化引擎
        Laya3D.init(1000, 500,true);
        
        //适配模式
        Laya.stage.scaleMode = Laya.Stage.SCALE_FULL;
        Laya.stage.screenMode = Laya.Stage.SCREEN_NONE;
        
        //加载3D资源
        Laya.loader.create(["LayaScene_collider3D/collider3D.ls",
                        "LayaScene_truck/truck.lh",
                        "LayaScene_box/box.lh"],Laya.Handler.create(this,onComplete));
        
        //创建信息提示框
        LayaAir3D.txt=new Laya.Text();
        LayaAir3D.txt.text="还未获得汽车！！";
        LayaAir3D.txt.color="#ff0000";
        LayaAir3D.txt.bold=true;
        LayaAir3D.txt.fontSize=30;
        LayaAir3D.txt.pos(100,50);
        Laya.stage.addChild(LayaAir3D.txt);			
		
		function onComplete()
		{
			//添加3D场景
			var scene= new Laya.Scene();
			Laya.stage.addChild(scene);
			//为场景添加控制脚本			
			scene.addScript(SceneScript);

            //创建货车模型，加载到场景中
            var truck3D = Laya.loader.getRes("LayaScene_truck/truck.lh");
            scene.addChild(truck3D);
          
			//获取货车的车身（车头不进行装货）
			var meshSprite3D=truck3D.getChildAt(0).getChildByName("body");
          	//添加网格型碰撞器组件
          	var meshCollider=meshSprite3D.addComponent(Laya.MeshCollider);
          	//为Mesh碰撞器mesh网格（否则没有尺寸，无法被射线检测）
         	boxCollider.mesh=meshSprite3D.meshFilter.sharedMesh;
		}

    }
    return LayaAir3D;
} ());

LayaAir3D();
```


Modifier le Sous - Code de scène comme suit:


```typescript

var SceneScript = (function(_super)
{
    function SceneScript()
    {
        SceneScript.super(this);
    }
    Laya.class(SceneScript,"SceneScript",_super);
  
    var scene;
    /**3D摄像机**/
    var camera;
    /**用于鼠标检测的射线**/
    var ray;
    /**画矢量线的3D显示对象**/
    var phasorSprite3D;
    /**碰撞信息**/
    var rayCastHit;	    
    /**鼠标点击创建的3D对象**/
    var box;
    /***获得的物品***/
    var nameArray=[];


     /**
     * 覆写3D对象加载组件时执行的方法
     * @param owner 加载此组件的3D对象
     */	

     SceneScript.prototype._load=function(owner)
    {
        //获取脚本所属对象
        scene=owner;
    }

    /*初始化场景（摄像机、碰撞相关对象、添加碰撞器等）*/
    SceneScript.prototype._start = function(state)
    {
        //创建摄像机（纵横比，近距裁剪，远距裁剪）
        camera = new Laya.Camera(0,0.1,1000);
        camera.transform.position = new Laya.Vector3(1,7,10);
        camera.transform.rotate(new Laya.Vector3(-30,0,0),false,false);
        //加载到场景
        scene.addChild(camera);
        //加入摄像机移动控制脚本
        // this.camera.addComponent(CameraMoveScript);

        //创建一条射线
        ray = new Laya.Ray(new Laya.Vector3(),new Laya.Vector3());
        //创建矢量3D精灵
        phasorSprite3D = new Laya.PhasorSpriter3D();
        //创建碰撞信息
        rayCastHit = new Laya.RaycastHit();
 
        //鼠标点击需要创建的物品，用于克隆使用（火车上的货物）
        box = Laya.loader.getRes("LayaScene_box/box.lh");           
        //鼠标点击事件回调
        Laya.stage.on(Laya.Event.MOUSE_DOWN,this,onMouseDown);
    }

    /****覆盖场景渲染后更新方法（相当于场景渲染完成后的帧循环）****/
    SceneScript.prototype._postRenderUpdate = function(state)
    {
        //画参考线、时行碰撞检测
        //根据鼠标屏幕2D座标修改生成射线数据 
        camera.viewportPointToRay(new Laya.Vector2(Laya.stage.mouseX,Laya.stage.mouseY),ray);        
        //射线检测，最近物体碰撞器信息，最大检测距离为300米，默认检测第0层
        Laya.Physics.rayCast(ray,rayCastHit,300);            
        //摄像机位置
        var position = new Laya.Vector3(camera.position.x, 0, camera.position.z);
        //开始绘制矢量3D精灵，类型为线型
        phasorSprite3D.begin(Laya.WebGLContext.LINES, camera);

        //如果与物品相交，画三面边线
        if(rayCastHit.sprite3D){
            //从碰撞信息中获取碰撞处的三角面定点
            var trianglePositions = rayCastHit.trianglePositions;
            //矢量绘制三角面边线
            phasorSprite3D.line(trianglePositions[0], new Laya.Vector4(1,0,0,1),
                                trianglePositions[1], new Laya.Vector4(1,0,0,1));
            phasorSprite3D.line(trianglePositions[1], new Laya.Vector4(1,0,0,1),
                                trianglePositions[2], new Laya.Vector4(1,0,0,1));
            phasorSprite3D.line(trianglePositions[2], new Laya.Vector4(1,0,0,1),
                                trianglePositions[0], new Laya.Vector4(1,0,0,1));
        } 


        //根据射线的原点绘制参考直线（为了观察方便而绘制，但矢量线并不是射线真正的路径）
        phasorSprite3D.line(ray.origin, new Laya.Vector4(1,0,0,1), position , new Laya.Vector4(1,0,0,1));    
        //结束绘制
        phasorSprite3D.end();
    }

    /**
     * 鼠标点击拾取
     */
    function onMouseDown()
    {
        //如果碰撞信息中的模型不为空,删除模型
        if(rayCastHit.sprite3D)
        {
            //克隆一个货物模型 
            var cloneBox=Laya.Sprite3D.instantiate(box).getChildAt(0);

            //添加网格型碰撞器组件
            var meshCollider=meshSprite3D.addComponent(Laya.MeshCollider);
            //为Mesh碰撞器mesh网格（否则没有尺寸，无法被射线检测）
            meshCollider.mesh=meshSprite3D.meshFilter.sharedMesh;                   
            scene.addChild(cloneBox);
            //修改位置到碰撞点处
            cloneBox.transform.position = rayCastHit.position;
            //更新提示信息
            nameArray.push(cloneBox.name);
            LayaAir3D.txt.text = "您在货车上装载了 "+nameArray.length+" 件货物!";
        }
    }
    return SceneScript;
})(Laya.Script);
```


Compiler le code d 'affichage ci - dessus permet de voir que l' objet peut être créé par clic de souris (fig. 3) et que les rayons se croisent avec le modèle pour afficher un triangle à l 'intersection du modèle.

![3](img/3.gif)(图3)</br>