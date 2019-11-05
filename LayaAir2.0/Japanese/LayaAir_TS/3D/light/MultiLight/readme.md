# 多光源渲染

###### *version :2.3.0   Update:2019-9-27*

最新の2.3.0では多光源レンダリングがサポートされています。

###どうやってマルチ光源を起動しますか？

Config 3 Dの中の修正が必要です。**enbaleMulti Light**マルチ光源と**maxLight Count**最大光源数パラメータ。2.3.0ではデフォルトで多光源レンダリングが開始されました。標準の最大光源数は32です。

>マルチ光源レンダリングパラメータをオンし、変更します。


```typescript

var config = new Laya.Config3D();
//开启多光源
config.enbaleMultiLight = true;
//设置最大光源数
config.maxLightCount = 16;
Laya3D.init(750, 1334, config);
```


>マルチ光源の描画をオフにする


```typescript

var config = new Laya.Config3D();
//开启多光源
config.enbaleMultiLight = false;

Laya3D.init(750, 1334, config);
```


###マルチ光源レンダリング使用

設定が完了すると、多光源レンダリングが開始されました。以下の公式例（[demo地址](http://layaair2.ldc2.layabox.com/demo2/?language=ch&category=3d&group=Lighting&name=MultiLight)）のコードを説明します。

>ライトスクリプトを移動


```typescript

export default class LightMoveScript extends Laya.Script3D {
    //需要操作的光源数组
	public lights = [];
	//光源对应的位置偏移数组
	public offsets = [];
	//光源对应的移动半径数组
	public moveRanges = [];

	public onUpdate(){
		var seed = Laya.timer.currTimer * 0.002;
		for (var i = 0, n = this.lights.length; i < n; i++) {
			var transform: Transform3D = this.lights[i].transform;
			var pos: Vector3 = transform.localPosition;
			var off: Vector3 = this.offsets[i];
			var ran: Vector3 = this.moveRanges[i];
			pos.x = off.x + Math.sin(seed) * ran.x;
			pos.y = off.y + Math.sin(seed) * ran.y;
			pos.z = off.z + Math.sin(seed) * ran.z;
			transform.localPosition = pos;
		}
	}
}
```


>シーンを読み込み、光源を追加します。


```typescript

Laya.Scene3D.load("res/threeDimen/scene/MultiLightScene/InventoryScene_Forest.ls", Laya.Handler.create(this, function (scene) {
    //添加到场景
    Laya.stage.addChild(scene);
    var camera = scene.getChildByName("Main Camera");
    camera.addComponent(CameraMoveScript);
    camera.transform.localPosition = new Laya.Vector3(8.937199060699333, 61.364798067809126, -66.77836086472654);
	//将灯光移动脚本添加到摄像机上
    var moveScript = camera.addComponent(LightMoveScript);
    //获取操作的灯光数组
    var moverLights = moveScript.lights;
    var offsets = moveScript.offsets;
    var moveRanges = moveScript.moveRanges;
    moverLights.length = 15;
    //添加15个点光
    for (var i = 0; i < 15; i++) {
        //创建一个点光源
        var pointLight = scene.addChild(new Laya.PointLight());
        pointLight.range = 2.0 + Math.random() * 8.0;
        pointLight.color.setValue(Math.random(), Math.random(), Math.random());
        pointLight.intensity = 6.0 + Math.random() * 8;
        moverLights[i] = pointLight;
        //设置对应的位置偏移
        offsets[i] = new Laya.Vector3((Math.random() - 0.5) * 10, pointLight.range * 0.75, (Math.random() - 0.5) * 10);
        //设置对应的移动半径
        moveRanges[i] = new Laya.Vector3((Math.random() - 0.5) * 40, 0, (Math.random() - 0.5) * 40);
    }
	//创建一个聚光灯
    var spotLight = scene.addChild(new Laya.SpotLight());
    spotLight.transform.localPosition = new Laya.Vector3(0.0, 9.0, -35.0);
    spotLight.transform.localRotationEuler = new Laya.Vector3(-15.0, 180.0, 0.0);
    spotLight.color.setValue(Math.random(), Math.random(), Math.random());
    spotLight.range = 50;
    spotLight.intensity = 15;
    spotLight.spotAngle = 60;
}));
```


！[](img/1.gif)<br/>(図1)