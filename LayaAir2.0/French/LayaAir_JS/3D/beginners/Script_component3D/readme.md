#Ensemble de script layaair3d

###Ensemble parent Component 3D

Dans le moteur layaair 3D, afin de faciliter l 'affichage du contrôle d' objet et la maintenance du Code, une classe de composants puissante Component 3D est fournie.Nos composants de commande d 'animation, les collisions, les scripts, les points d' accrochage osseux et ainsi de suite sont basés sur l 'extension de la fonction du composant, appartenant à sa sous - catégorie.En outre, le moteur layaair 3D supporte l 'ajout d' une pluralité de composants sur un objet 3D, ce qui permet une commande plus souple du composant.

Dans l 'ancien profil technique, nous avons présenté les fonctions de base de l' ensemble commande d 'animation et de l' ensemble collisionneur.Aux fins du présent chapitre,**Nous avons donné l'exemple de l'ensemble script.**Dans la mesure où il hérite de la catégorie des composants, mais il n 'a pratiquement pas sa propre fonction d' extension, principalement à l 'aide des attributs de Component 3D de la catégorie paternelle et des méthodes, la fonction script sera mise à jour ultérieurement, s' il vous plaît s' il vous plaît!



###Principales propriétés et procédés d 'assemblage

**Owner**: objet sprite3d associé à l 'ensemble lié.

* ***Enable:**Si le composant est activé ou non, il est activé par défaut au moment de son chargement, et s' il est modifié en false, l 'événement de modification d' activation est d 'abord transmis, puis le procédé de mise à jour du composant \ \ Update () interrompt l' exécution.

**Onawake ()**Le composant n 'est exécuté qu' une seule fois après sa création, sans Code par défaut.Elle peut être recouverte dans la catégorie de succession, dans laquelle le Code logique doit être initialisé.

**Onsart ()**Une fois que l 'objet 3D de l' ensemble de chargement a été mis à jour, il n 'y a pas de code par défaut.Il peut être recouvert dans la catégorie d 'héritage, dans laquelle le Code logique est inséré une fois que l' objet 3D a été chargé.

Par exemple, le clonage d 'un objet 3D avec un script, s' il y a plus de sous - objets dans l' objet 3D, le clonage de script est effectué en premier, et si la logique du script n 'est pas insérée dans le procédé onstart (), un objet vide se produit lors de l' acquisition du sous - objet.

**Onupdate ()**Procédé de mise à jour de composants correspondant à un cycle de trames.Elle peut être recouverte dans la catégorie d 'héritage et un code logique nécessitant une mise à jour de chaque trame peut être inséré dans le procédé.



###événement associé à un composant

* * Component \ \ added:**Le composant est chargé d 'un événement d' exécution, envoyé par le propriétaire du composant sprite3d et transmis comme paramètre.****
****
**Component \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \



###Ensemble script

Le script est hérité de l 'ensemble, ce qui permet d' ajouter le script à l 'objet d' affichage 3D en utilisant le procédé addcomponent () de l 'objet d' affichage.

Dans l 'exemple des moteurs 3D du réseau officiel, de nombreux exemples de caméras ont été utilisés dans le script mobile camera movescript, auquel la caméra a ajouté un script qui permet de commander sa rotation et son déplacement vers et vers le bas par le clavier à l' aide de la souris.Ajouter le code suivant pour le procédé d 'assemblage de scripts:


```typescript

//添加摄像机脚本组件
camera.addComponent(CameraMoveScript);
```


Bien entendu, le script peut également être supprimé de l 'objet si certaines nécessités logiques l' exigent, et le script peut être supprimé par la méthode removecomponentbytype () de l 'objet affiché en 3D.


```typescript

//根据类型移除脚本组件
camera.removeComponentByType(CameraMoveScript);
//移除所有组件(包括动画、脚本、碰撞器等，注意，此方法不能移除子对象节点上的组件)
camera.removeAllComponent();
```




###Créer son propre module de script

Les développeurs peuvent consulter le script de caméra et créer leurs propres composants de script pour commander les objets dans la scène.

Dans le développement du jeu layaair 3D, on crée des scènes, des personnages, des animations dans l 'unité, on exporte les scènes et on les charge dans le Code, ce qui permet d' ajouter des éléments de script de commande à différents objets de la scène.

Par exemple, un script de commande d 'acteur principal, un script de commande de NPC, un script de commande d' objet de scène, etc. un niveau de jeu est ainsi créé, et lorsque le jeu charge la scène de niveau suivant, le script peut être multiplexé, l 'entretien du projet est facilité et rapide et la commande et l' affichage sont séparés.

Dans l 'exemple suivant, nous modifions le code "voyage à démarrage rapide en 3D" dans le document technique pour créer un script de commande à ajouter sur Box et supprimer l' ensemble script dans quatre secondes.

Un script personnalisé boxcontrolscript est d 'abord créé pour modifier le matériau et la rotation de l' objet Box auquel appartient le script.


```typescript

export default class BoxControlScript extends Laya.Script3D{
  constructor(){super();}
  /**
	 * 覆写3D对象组件被激活后执行，此时所有节点和组件均已创建完毕，此方法只执行一次
	 */
  onAwake(){
    // this.owner
  }
  /*覆写组件所属3D对象实例化完成后，第一次更新时的执行方法*/
  onStart(){
    //得到3D对象的材质
    var material = this.owner.meshRenderer.material;
    //更改3D对象的材质反射率 （偏红）
    material.albedoColor = new Laya.Vector4(1,0,0,1);
  }
  /**
	 * 覆写组件更新方法（相当于帧循环）
	 */
  onUpdate(){
    this.owner.transform.rotate(new Laya.Vector3(0,0.5,0),false,false);
  }
}
```


Le type de script ci - dessus est ensuite ajouté à Box dans le code "démarrer rapidement le voyage en 3D" et le script est supprimé quatre secondes plus tard.


```typescript

import BoxControlScript from "./BoxControlScript";
var Main = (function () {
  function Main() {

    //初始化引擎
    Laya3D.init(0, 0);

    //适配模式
    Laya.stage.scaleMode = Laya.Stage.SCALE_FULL;
    Laya.stage.screenMode = Laya.Stage.SCREEN_NONE;

    //开启统计信息
    Laya.Stat.show();

    //添加3D场景
    var scene = Laya.stage.addChild(new Laya.Scene3D());

    //添加照相机
    var camera = (scene.addChild(new Laya.Camera( 0, 0.1, 100)));
    //移动摄影机位置
    camera.transform.translate(new Laya.Vector3(0, 3, 3));
    //旋转摄影机方向
    camera.transform.rotate(new Laya.Vector3( -30, 0, 0), true, false);
    //设置背景颜色
    camera.clearColor = null;

    //添加方向光
    var directionLight = scene.addChild(new Laya.DirectionLight());
    //设置灯光漫反射颜色
    directionLight.color = new Laya.Vector3(0.6, 0.6, 0.6);
    //设置灯光的方向（弧度）
    directionLight.transform.worldMatrix.setForward(new Laya.Vector3(1, -1, 0));

    //添加自定义模型
    var box= scene.addChild(new Laya.MeshSprite3D(new Laya.BoxMesh(1,1,1),"MOs"));
    //设置模型的旋转
    box.transform.rotate(new Laya.Vector3(0,45,0),false,false);
    //创建材质
    var material = new Laya.PBRSpecularMaterial();
    //加载模型的材质贴图
    Laya.Texture2D.load("res/layabox.png",Laya.Handler.create(this,function(text){
      material.albedoTexture = text;
      //给模型添加材质
      box.meshRenderer.material = material;

      //给box添加自定义脚本组件
      box.addComponent(BoxControlScript);
    }))
    //4秒后删除自定义组件
    Laya.timer.once(4000,this,this.onLoop,[box]);
  }
  var _proto = Main.prototype;
  _proto.onLoop = function(box){
    // 获取到组件
    var boxContro = box.getComponent(BoxControlScript);
    // 移除组件
    // boxContro.destroy();
    //如不想移除组件，可设置为不启用能达到同样效果（组件_update方法将不会被更新）
    boxContro.enabled = false;
  }
  return Main;
} ());
new Main();

```


Dans le Code précédent, si les développeurs ne veulent pas enlever le composant dans quatre secondes, mais cessent simplement d 'utiliser le script, la propriété d' activation du script peut être définie comme False.

La compilation et l 'exécution de ce code permettent d' obtenir l 'effet ci - après (fig. 1) après élimination du composant, le modèle cesse de tourner.

![1](img/1.gif)(Figure 1) < / BR >

