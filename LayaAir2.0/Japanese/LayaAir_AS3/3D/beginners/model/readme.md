##LayaAir 3 Dモデル

###モデルの概要

3 Dモデルとは、3 Dソフトウェアによって物体の構造モデル化によって形成される3 D立体オブジェクトをいう。現在LayaAir 3 Dエンジンには2つのモデル表示タイプが含まれています。1つは普通のモデルです。**Mesh Sprite 3 D**を選択します。**SkinedMesh Sprite 3 D**。

違いは、モンピのアニメーションモデルとは、制作時にマスクと骨格アニメーションの模型を入れて、アニメのキャラクターがよく使われています。普通の模型はアニメがないシーン景観モデルなどを指します。

これらはいずれも模型メッシュと材質の二つの部分を含んでいます。

**モデルグリッド(Mesh):**

モデルメッシュは点、線、面からなる三次元データで、LayaAirエンジンには専門的なMeshメッシュデータ類があり、3 Dモデルの表示対象Mesh Sprite 3 DまたはSkinedMesh Sprite 3 Dを付与して、シーンに表示することができます。

現在は3 D制作ソフトが多く、3 d maxとmayaソフトが主流です。3 Dモデルのデータフォーマットも多く、例えばFBX、3 DS、OBJなどです。

LayaAirエンジンは、モデル導出ツールFBXToolsとunity 3 D導出プラグインを提供し、layaAirを生成するために必要な3 Dデータフォーマットを提供する。unityエクスポートプラグインを使用すると、FBXToolsツールは今後更新されません。

**材質(Material)：**

材質説明は独立した章で紹介します。この章では説明を見合わせます。



###エンジン独自の基礎モデルを作る

高速で3 Dの旅を開くコースでは、BoxMeshケースモデルを使用しました。今回はLayaAirエンジンが提供する他のSphere Mesh、CylindersMesh基礎モデルデータを紹介します。順番にそれらを作成し、トレーンform属性を通じて位置を移動します。具体的なコードは以下の通りです。

作成時に注意したいのは、シーンにロードされたエンジンはモデルの真ん中にあります。したがって、モデルの中心点を参考に移動、回転、スケーリングを行います。シーンにロードする場合、モデルはデフォルトでシーンの世界座標の原点に置かれます。2 Dと似ています。


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


上のコードでは、カメラと照明を作成し、基本的な幾何学モデルを3つ追加しました。効果は図1のように表示されます。

![图片1](img/1.png)<br/>(図1)



###三次元ソフトウェアの生成モデルを作成します。

上記の3つの基本モデルは主に開発者がテストを学ぶために使用されます。ゲームのモデルは三次元ソフトで作られた後、unityエディタでスティッチングを導入し、layaAirでエクスポートしたツールを転化して、3 Dシーンやモデル表示クラスを通してロードして使用します。

ここではまた、導出したリソースの種類とファイルの使い方を説明します。

エクスポートされたフォルダには、シーン、3 Dモデルの容器、3 Dモデル、3 D材質などの解析ファイル、フォトスタンプ、材質スタンプなどのデータファイルが含まれています。

![图片2](img/2.png)<br/>(図2)

**love Sceneフォルダ**untiyでフォトスタンプを作成したフォルダです。untiyで作成したシーン名と同じで、フォトスタンプはシーンScene編で紹介されています。

**Materialsフォルダ**unityでFBXモデルを導入した時に素材ボールを作成するフォルダです。エクスポートしたリソースは対応するLayaAir材質データ解析ファイルです。ファイルには材質のレンダリングモード、リソースパス、材質の各種光色属性などが格納されています。

**Textureフォルダ**unityで作成した保存スタンプのフォルダです。ここでは資源は材質のスタンプファイルで、一連の画像ファイルです。LayaAirエンジンでjpgまたはpng形式の画像を使って、他のフォーマットの写真を自動的にjpgまたはpngに変換します。



####*.lsフォーマットScheneデータファイル

エクスポートされたシーンSceneタイプのデータファイルは、前のコースで説明されました。ここでは多く説明しません。




####*.lh形式Sprite 3 Dデータファイル

導出された3 D表示対象容器Spirte 3 Dタイプデータファイルは、JSONフォーマット符号化で、unity 3 DにおけるlayaAir導出プラグイン選択導出「Sprite 3 D」カテゴリ生成であり、内部記憶は*.lsフォーマットよりも少なくなり、その他はすべて同じです。

「*.lh」フォーマットのロードはシーンローディング方法と似ています。Sprite 3 D.load()を非同期でロードするか、Laya.loader.createをプリロードするか、コードを参照してください。


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




####*.lmフォーマットデータファイル

「Scene」ファイルまたは「Sprited 3 D」ファイルタイプにかかわらず、エクスポートされたリソースフォルダにはシリーズ＊lmフォーマットファイルが含まれており、本プロジェクトではmodelフォルダはunityで開発者が構築したFBXモデルを格納するフォルダで、図2のように、エクスポート時に対応するフォルダと.lmリソースファイルが生成されています。

![图片3](img/3.png)<br/>(図3)

「*.lm」ファイルはモデルデータファイルであり、Mesh Sprite 3 DまたはSkinedMesh Sprite 3 Dタイプ表示オブジェクトのグリッドデータMeshを生成し、モデルグリッドの頂点位置、法線、頂点色、頂点UVなどの情報を含んでいます。

Mesh Sprite.load（）を非同期でロードするか、Laya.loader.create（）をプリロードする方法でロードします。参照コードは以下の通りです。


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


上記の2つの方法でゲーム画面にモデルを表示することができます。材質スタンプエンジンも自動的にモデルにロードされます。プロジェクトでは、状況に応じて上記の2つの方法を使ってもいいです。固定シーンは使用できます。ls形式でロードします。活動するアイテムはlsまたはlm方式でロードできます。



###サブオブジェクトモデルとグリッドを取得

3 Dモデルは、場合によっては複数のサブモデルのオブジェクトから構成されることがあります。例えばシーンモデル.lsは、基本的には複数の物体モデルと材質から構成されています。外層はSprite 3 D容器で、内部は本物のモデルMesh Sprite 3 DまたはSkinedMesh Sprite3 Dです。また、複数の階層がネストされている場合があります。

####サブオブジェクトモデルを取得

ゲームロジックを作成する時、モデルを修正したり、モデルを切り替えたり、削除したり、モデルにコンポーネントを追加したり、モデル上のアニメーションコンポーネントを取得したり、モデルの材質を修正したりする必要があります。これは全部ロードされたモデルからサブオブジェクトを取得する必要があります。**get ChildAt()、get ChildByName()**方法はサブオブジェクトを取得します。これは2 Dエンジン取得サブオブジェクト法と同じです。

次はトラックモデルのlhファイルをロードして、そのサブオブジェクトを取得します。サブオブジェクトを取得する前に、開くことを提案します。lhファイルはモデルの親子レベルの関係を調べます。モデルを作る時に、モデルがいくつのサブオブジェクトモデルから構成されているかということと、その命名規則が確定できないからです。

tips：3 ds maxでモデル化する場合、モデルのサブオブジェクトに名前を付けることを提案し、プロジェクトのリソース命名規則を制定し、デフォルトのモデル名を使用しないでください。

次の例ではunityから導いたトラックtruck.lhをロードし、開いた後にJSON構造で見ることができます。外層はスピリット3 D容器（unityのシーンに相当）で、内部はSprtie 3 D容器（unityシーンに相当するトラック）で、トラック容器の中は2つのサブモデルMesh Sprited 3 D（車体モデルと車体モデル）です。したがって、モデルMesh Sprite 3 Dを得るためには、2回のget ChildAt（）方式が必要です。

サブオブジェクトを取得する際には、モデルと材質がロードされていないとサブオブジェクトを取得できないので、リソースのプリロードが必要です。


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


上記のコードをコンパイルすると、モデルが表示されます（図4）。ブラウザでF 12でコンソールを開くと、出力されたモデルの名前が「body」となり、モデルの取得に成功したことが分かります。

![图片4](img/4.png)<br/>(図4)



####モデルグリッドMeshを取得

ゲームの中で、私達はいつもキャラクターの入れ替えシステムを作って、時にはモデルを変えて、時にはスタンプを交換して、時には両方とも交換します。材質の貼り付け部分は後の章で説明しますので、モデルのグリッドを交換する方法だけを紹介します。

モデルMesh Sprite 3 DまたはSkinedMesh Sprite 3 Dにあります。**meshFilter**属性は、グリッドフィルタクラスの例です。この属性の中の**sharedMesh**モデルのメッシュです。交換や廃棄ができます。

次の例を参照してください。トラックモデルをロードしてから2秒後に、既存の車体メッシュに新しいヘッドメッシュオブジェクトを作成します。


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


![图片5](img/5.gif)<br/>(図5)