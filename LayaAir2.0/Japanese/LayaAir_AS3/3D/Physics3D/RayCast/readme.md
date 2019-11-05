#物理放射線検査

###### *version :2.1.1   Update:2019-7-19*

前の方にあります**グラフィックシステムの概念**「放射線」という数学的なツールがあります。**カメラ**ここで放射線の使用について詳しく説明します。

LayaAir 3 Dで放射線検出を実現するコアはScenee 3 Dシーン属性を使用している。**Physics Simulation物理シミュレータ**。詳細は表示できます[Api地址](https://layaair.ldc.layabox.com/api2/Chinese/index.html?category=3D&class=laya.d3.physics.PhysicsSimulation)。放射線検出用のインターフェースは4つあり、2つに分類されます。`raycastFromTo`を選択します`raycastAllFromTo`クラス`rayCast`を選択します`rayCastAll`クラスです。前の2つをA類、後はB類にします。この2つの方法のアプリを見てみます。

！[](img/1.png)<br/>(図1)

！[](img/2 png)<br/>(図2)

A類はパラメータとして2つの点を使用していますが、B類では作成済みの放射線を使用していますが、放射線の長さを設定する必要があります。ベルト`All`の方法はすべての物体を検査するかどうか、つまり物体を突き抜けるかどうかだけです。この方法の`out:Vector.<hitresult>`—衝突結果[配列要素は回収されます]。</hitresult>

まずA類を展示します。`raycastFromTo`を選択します`raycastAllFromTo`このコードの使用は、公式の例に由来しています。[demo地址](https://layaair.ldc.layabox.com/demo2/?language=ch&category=3d&group=Physics3D&name=PhysicsWorld_RayShapeCast)）0


```typescript

var hitResult:HitResult = new HitResult();
var hitResults:Vector.<HitResult> = new Vector.<HitResult>();
//是否穿透
if (castAll) {
    //进行射线检测,检测所有碰撞的物体
    scene.physicsSimulation.raycastAllFromTo(from, to, hitResults);
    //遍历射线检测的结果
    for (i = 0, n = hitResults.length; i < n; i++)
        //将射线碰撞到的物体设置为红色
        ((hitResults[i].collider.owner as MeshSprite3D).meshRenderer.sharedMaterial as BlinnPhongMaterial).albedoColor = new Vector4(1.0, 0.0, 0.0, 1.0);
} else {
    //进行射线检测,检测第一个碰撞物体
    scene.physicsSimulation.raycastFromTo(from, to, hitResult);
    //将检测到的物体设置为红色
    ((hitResult.collider.owner as MeshSprite3D).meshRenderer.sharedMaterial as BlinnPhongMaterial).albedoColor = new Vector4(1.0, 0.0, 0.0, 1.0);
}
```


！（℃）<br/>（図3）貫通しない放射線

！（℃）<br/>（図4）貫通する放射線

Bクラス`rayCast`を選択します`rayCastAll`このコードは、公式の例から使用される。([demo地址](https://layaair.ldc.layabox.com/demo2/?language=ch&category=3d&group=Camera&name=CameraRay))

例はスクリーン空間の一点（マウスで押す点）に従って、近裁断面から遠裁断面までの線を形成し、放射線検出を行う。効果は（図5）の通りです


```typescript

point.x = MouseManager.instance.mouseX;
point.y = MouseManager.instance.mouseY;
//产生射线
_camera.viewportPointToRay(point,_ray);
//拿到射线碰撞的物体
_scene.physicsSimulation.rayCast(_ray,outs);
//如果碰撞到物体
if (outs.length != 0) {
    for (var i:int = 0; i < outs.length; i++){
        //在射线击中的位置添加一个立方体
        addBoxXYZ(outs[i].point.x, outs[i].point.y, outs[i].point.z );
    }		
}

//在传入的x,y,z位置添加一个box
public function addBoxXYZ(x:int, y:int, z:int ):void {/**内容省略**/}
```


！[](img/5 gif)<br/>(図5)