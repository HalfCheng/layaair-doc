#Script de souris

###### *version :2.1.1   Update:2019-8-2*

L 'événement de la souris dans le script de souris script script 3D de layaair2.0 a été fourni pour faciliter le développement.Si vous voulez utiliser un script de souris, un collisionneur physique est nécessaire sur le script owner.

**Tips**Le script de la souris dépend de la détection des rayons, mais n 'a pas besoin d' ouvrir les rayons manuellement.

**Contenu du script de la souris**

> Method

`onMouseClick():void`Exécution en cliquant sur la souris

`onMouseDown():void`Cliquez sur la souris pour exécuter

`onMouseDrag():void`Glisser la souris

`onMouseEnter():void`Exécution à l 'entrée de la souris

`onMouseOut():void`Exécution lorsque la souris s' en va

`onMouseOver():void`Exécution au passage de la souris

`onMouseUp():void`Exécution lorsque la souris explose

Tous les procédés ci - dessus sont virtuels et il suffit de réécrire la couverture au moment de l 'utilisation.

Sur la base de l 'exemple ci - dessus, nous avons créé un script de souris et ajouté un script à chacun des quatre singes.

]**Script**- Oui.


```typescript

export default class MouseScript extends Laya.Script3D{
		constructor(){super();}
		//物体必须拥有碰撞组件（Collider）
		//当被鼠标点击
		onMouseDown(e){
			//console.log("点击到了我box",owner.name);
			//从父容器销毁我自己
			this.owner.removeSelf();
		}
	}
```


]**Catégorie principale**- Oui.


```typescript

//给四个猴子添加脚本
this.staticLayaMonkey.addComponent(MouseScript);
this.layaMonkey_clone1.addComponent(MouseScript);
this.layaMonkey_clone2.addComponent(MouseScript);
this.layaMonkey_clone3.addComponent(MouseScript);
```


Comme le montre la figure 1:

[] (IMG / 1.gif) <br > (Figure 1)
