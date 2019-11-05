# DirectionLight介绍

###### *version :2.0.1beta   Update:2019-3-30*

Direction Light (평행광) 과 약간의 빛의 구별이 비교적 커서 고정된 방향이 있으며, 호도치를 통과할 수 있으며, 감축과 광조 범위도 없이 전 장면의 모든 모형을 밝게 할 수 있다.3D 세계에서 항상 고정 방향을 모의 태양광에 쓰인다.


```typescript

//创建方向光
this.directionLight = this.scene.addChild(new Laya.DirectionLight());
//设置灯光颜色
this.directionLight.color = new Laya.Vector3(1, 1, 1);
//设置灯光方向
var mat = this.directionLight.transform.worldMatrix;
mat.setForward(new Laya.Vector3(-1.0, -1.0, -1.0));
this.directionLight.transform.worldMatrix = mat;
```


​**setForward**평행광의 방향은 각각 x, y, z 축의 방향을 대표하고, 음수는 족축, 정수는 족축, 치의 범위는-1-0-1, 범위를 넘은 후 - 1, 1, 1, 초학자들은 이 범위 안에 치수를 관찰 방향의 변화를 가질 수 있다.

[] (img/1.png)<br>(1)

