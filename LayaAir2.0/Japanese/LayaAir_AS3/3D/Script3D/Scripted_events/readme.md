#スクリプトのイベント

###### *version :2.1.0beta   Update:2019-6-26*

前ではカスタムスクリプトの作成方法を紹介しましたが、具体的なシナリオを見てみましょう。具体的な説明はAPIを見ることができる（[地址](https://layaair.ldc.layabox.com/api2/Chinese/index.html?category=Core&class=laya.d3.component.Script3D)を選択します。

シナリオの追加についてはここではやったことのない解説を使っていますが、これまでの例では多かれ少なかれ脚本を使っています。スクリプトの公式例（[demo地址](https://layaair.ldc.layabox.com/demo2/?language=ch&category=3d&group=Script&name=ScriptDemo)を選択します。この例ではonAwake，onStart，onUpdate，one Dispableを引き継いだ。材質のロードが完了したら、cubeにスクリプトを追加しました。そして、4秒後に実行されるスクリプト除去動作。


```typescript

/**
 * 创建后只执行,只会执行一次，此时所有节点和组件以及创建完毕
 */
public override function onAwake():void {}

/**
 * 脚本每次启动后执行，例如被添加到一个激活的对象上或者设置脚本的enabled = true
 */
public override function onEnable():void {}

/**
 * 第一次执行update之前执行，只会执行一次
 */
public override function onStart():void {}

/**
 * 开始触发时执行
 */
public override function onTriggerEnter(other:PhysicsComponent):void {}

/**
 * 持续触发时执行
 */
public override function onTriggerStay(other:PhysicsComponent):void {}

/**
 * 结束触发时执行
 */
public override function onTriggerExit(other:PhysicsComponent):void {}

/**
 * 开始碰撞时执行
 */
public override function onCollisionEnter(collision:Collision):void {}

/**
 * 持续碰撞时执行
 */
public override function onCollisionStay(collision:Collision):void {}

/**
 * 结束碰撞时执行
 */
public override function onCollisionExit(collision:Collision):void {}

/**
 * 鼠标按下时执行
 */
public override function onMouseDown():void {}

/**
 * 鼠标拖拽时执行
 */
public override function onMouseDrag():void {

}

/**
 * 鼠标点击时执行
 */
public override function onMouseClick():void {}

/**
 * 鼠标弹起时执行
 */
public override function onMouseUp():void {}

/**
 * 鼠标进入时执行
 */
public override function onMouseEnter():void {}

/**
 * 鼠标经过时执行
 */
public override function onMouseOver():void {}

/**
 * 鼠标离开时执行
 */
public override function onMouseOut():void {}

/**
 * 键盘按下时执行
 */
public override function onKeyDown(e:Event):void {}

/**
 * 键盘产生一个字符时执行
 */
public override function onKeyPress(e:Event):void {}

/**
 * 键盘抬起时执行
 */
public override function onKeyUp(e:Event):void {}

/**
 * 每帧更新时执行
 */
public override function onUpdate():void {}

/**
 * 每帧更新时执行，在update之后执行
 */
public override function onLateUpdate():void {}

/**
 * 渲染之前执行
 */
public override function onPreRender():void {}

/**
 * 渲染之后执行
 */
public override function onPostRender():void {}

/**
 * 禁用时执行
 */
public override function onDisable():void {}

/**
 * 手动调用销毁时执行
 */
public override function onDestroy():void {}
```






