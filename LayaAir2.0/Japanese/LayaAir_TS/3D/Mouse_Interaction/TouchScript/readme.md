#マウススクリプト

###### *version :2.1.1   Update:2019-8-2*

LayaAir 2.0では、開発者の便宜を図るために、マウススクリプト――Script 3 Dにおけるマウスイベントを提供しています。マウススクリプトを使用するには、スクリプトowner上にまだ物理的な衝突器が必要です。

**Tips**：マウススクリプトは放射線検査に依存しますが、開放者自身で手動で放射線を作成する必要はありません。

**マウスのスクリプトの内容:**

>Method

`onMouseClick():void`マウスをクリックした時に実行します。

`onMouseDown():void`マウスを押した時に実行します。

`onMouseDrag():void`マウスをドラッグしたときに実行します。

`onMouseEnter():void`マウスが入ると実行します。

`onMouseOut():void`マウスが離れる時に実行します。

`onMouseOver():void`マウスを通過した時に実行します。

`onMouseUp():void`マウスが跳ね上がった時に実行します。

上の方法は全部虚の方法です。上書きします。

上記の例に基づいてマウススクリプトを作成し、4つの猿にスクリプトを追加しました。

>**スクリプトクラス**:


```typescript

export default class MouseScript extends Laya.Script3D{
    	private meshsp:Laya.MeshSprite3D;
		constructor(){super();}
        /**
		* 覆写3D对象组件被激活后执行，此时所有节点和组件均已创建完毕，此方法只执行一次
		*/
        onAwake(){
			this.meshsp = this.owner as Laya.MeshSprite3D();
        }
		//物体必须拥有碰撞组件（Collider）
		//当被鼠标点击
		onMouseDown(e){
			//console.log("点击到了我box",owner.name);
			//从父容器销毁我自己
			this.owner.removeSelf();
		}
	}
```


>**メインクラス**:


```typescript

//给四个猴子添加脚本
this.staticLayaMonkey.addComponent(MouseScript);
this.layaMonkey_clone1.addComponent(MouseScript);
this.layaMonkey_clone2.addComponent(MouseScript);
this.layaMonkey_clone3.addComponent(MouseScript);
```


マウススクリプトの効果を図1に示します。

！[](img/1.gif)<br/>(図1)
