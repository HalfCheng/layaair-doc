

# LayaAir3D之物理入门

#### 快速开启3D物理之旅

以下我们将用LayaAir引擎快速开始一个3D物理项目，并且以AS语言为教程，简单演示用引擎代码实现一个基本的3D物理应用。我们先预览一下效果:



![图](img/easyPhysics.gif)

在Main的主类中，我们构建了一个3D的物理世界，并且添加了简单3D物理世界所必须的要素（刚体和碰撞器组件）关于这些概念知识后续教程我们会详细的介绍，逐步带领大家了解3D知识。



对于这个简单的Demo，我们只是创建了小球受重力自然下落与另一个小球发生碰撞的物理效果，我们可以手动敲一下代码体验一下效果，或者可以直接复制代码，后续的文档中详细的学习知识点。

主类代码如下:

```typescript
import GameConfig from "./GameConfig";
import SceneScript from "./script/SceneScript"; 
class Main {
         constructor() {
            //初始化引擎
            Laya3D.init(0, 0);
            //适配模式
            Laya.stage.scaleMode = Laya.Stage.SCALE_FULL;
            Laya.stage.screenMode = Laya.Stage.SCREEN_NONE;
            //开启统计信息
            Laya.Stat.show();
           	Laya.loader.load("res/layabox.png",Laya.Handler.create(this,this.loadComplete));
        }        
        private loadComplete():void{
            var _scene:Laya.Scene3D = new Laya.Scene3D();
            Laya.stage.addChild(_scene);
            _scene.addComponent(SceneScript);
        }
    }

//激活启动类
new Main();
```
场景脚本代码：
```typescript

export default class SceneScript extends Laya.Script3D{
        private scene:Laya.Scene3D
        constructor(){
            super();
        }

        onAwake():void{
            this.scene = this.owner as Laya.Scene3D;
        }
    
        onStart():void{
            var camera:Laya.Camera = (this.scene.addChild(new Laya.Camera( 0, 0.1, 100))) as Laya.Camera;

            camera.transform.translate(new Laya.Vector3(1, 6, 10));
            camera.transform.rotate(new Laya.Vector3( -30, 0, 0), true, false);
            camera.clearColor = null;
            
			var directionLight:Laya.DirectionLight = this.scene.addChild(new Laya.DirectionLight()) as Laya.DirectionLight;
            directionLight.diffuseColor = new Laya.Vector3(0.6, 0.6, 0.6);
            directionLight.transform.worldMatrix.setForward(new Laya.Vector3(1, -1, 0));

            //添加自定义模型
            var sphere:Laya.MeshSprite3D = this.scene.addChild(new Laya.MeshSprite3D(new Laya.SphereMesh(1,100,100))) as Laya.MeshSprite3D;
            sphere.transform.rotate(new Laya.Vector3(0,90,0),false,false);
			sphere.transform.translate(new Laya.Vector3(0,3,0));
            sphere.meshRenderer.material = new Laya.BlinnPhongMaterial;
            var material:Laya.BlinnPhongMaterial = new Laya.BlinnPhongMaterial();
            Laya.Texture2D.load("res/layabox.png", Laya.Handler.create(null, function(tex:Laya.Texture2D):void {
                material.albedoTexture = tex;
            }));
            sphere.meshRenderer.material = material;
            
			//添加物理组件
			sphere.addComponent(Laya.PhysicsCollider);
			//给球添加刚体
			var rigid:Laya.Rigidbody3D = sphere.addComponent(Laya.Rigidbody3D);
			//有刚体的shape要加在刚体上
			rigid.colliderShape = new Laya.SphereColliderShape(1);
			//添加一个地板
			var floor:Laya.MeshSprite3D = this.scene.addChild(new Laya.MeshSprite3D(new Laya.PlaneMesh(10,10))) as Laya.MeshSprite3D;
			//给地板添加物理组件
			var floorCollicar:Laya.PhysicsCollider = floor.addComponent(Laya.PhysicsCollider);
			// 添加collidershape
			floorCollicar.colliderShape = new Laya.BoxColliderShape(10,0,10);
            //克隆一个球                
            Laya.timer.once(1000,this,function():void{
              //一秒之后复制一个球
                 var cloneSphere:Laya.MeshSprite3D = Laya.Sprite3D.instantiate(sphere) as Laya.MeshSprite3D;
                //设置位置偏移
                 cloneSphere.transform.translate(new Laya.Vector3(1,4,0));
                //添加到场景
                this.scene.addChild(cloneSphere);
            });
        }
    }  
```

​	关于物体复制的Sprite.instantiate方法可以从API去了解，这个方法要比clone方法要更方便一些。具体习惯可以根据个人习惯和场景去使用。

![图](img/图1.png)		

​	

  **[ tip:在本次案例中有涉及到使用代码给物体添加PhysicsCollider与RigidBody3D。在当物体有rigidbody时shape需要添加到rigidbody的collidershape上，没有的话需要将shape添加到PhysicsCollider的collidershape。]**

​	然后我们给球添加上弹力和滚动摩擦力

```java
.......
  //添加一个重量
  rigid.mass = 10;
  //添加弹力
  rigid.restitution = 1;
  //添加滚动摩擦力
  rigid.rollingFriction = 0.5
.......
```

看下修改后的效果:

![图](img/easyPhysics2.gif)