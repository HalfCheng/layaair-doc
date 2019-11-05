#Sprite 3 DのLayer

###### *version :2.0.1beta   Update:2019-4-13*

Sprite 3 DにはLayerマスク（レイヤー）の属性もあります。この属性設定は、カメラのフード層、放射線検出などを用いて機能することができる。以下の抜粋のカメラカバー層の例（[demo地址](https://layaair.ldc.layabox.com/demo2/?language=ch&category=3d&group=Camera&name=CameraLayer))


```typescript

//添加显示图层(为相机添加一个蒙版)
camera.addLayer(5);
//设置蒙版(所属图层)
layaMonkey_clone1.layer = 2;
layaMonkey_clone2.layer = 3;
layaMonkey_clone3.layer = 4;

......
//切换图层按钮事件监听
changeActionButton.on(Event.CLICK, this, function():void {
    camera.removeAllLayers();
    layerIndex ++;
    camera.addLayer(layerIndex%4 +1);
    camera.addLayer(5);
});
```


！[](img/1.gif)<br/>(図1)
