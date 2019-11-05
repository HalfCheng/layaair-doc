#Démarrage rapide d 'un projet layaair3d

Nous allons démarrer rapidement un projet en 3D avec le moteur layaair et suivre un cours en langue as, avec une démonstration simple du Code moteur pour une application en 3D fondamentale.

##IDE création d 'un exemple 3D

Téléchargement de layaairide et lancement du nouveau projet de sélection 3D, comme le montre la figure.

![图](img/1.png)(Figure 1)

C'est notre choix.**JavaScript**Langue.Créer un modèle 3D pour terminer ce que nous avons trouvé.Les concepteurs de la présentation de la structure du projet peuvent se référer au Programme d'enseignement de la deuxième génération.Il n 'y a pas de répétition.

##Affichage rapide de scénarios 3D

Nous pouvons voir la scène 3D dans laquelle le projet d 'exemple fonctionne.

![图](img/2.png)(Figure 2)

Dans la catégorie Runtime de la page d 'ouverture, nous avons construit un monde en 3D et ajouté quelques éléments nécessaires à un monde en 3D simple (scènes, caméras, lumières, modèles, matériaux).Le code suivant est extrait de gameu.as.


```typescript

//添加3D场景
var scene:Scene3D = Laya.stage.addChild(new Scene3D()) as Scene3D;

//添加照相机
var camera:Camera = (scene.addChild(new Camera(0, 0.1, 100))) as Camera;
camera.transform.translate(new Vector3(0, 3, 3));
camera.transform.rotate(new Vector3(-30, 0, 0), true, false);

//添加方向光
var directionLight:DirectionLight = scene.addChild(new DirectionLight()) as DirectionLight;
directionLight.color = new Vector3(0.6, 0.6, 0.6);
directionLight.transform.worldMatrix.setForward(new Vector3(1, -1, 0));

//添加自定义模型
var box:MeshSprite3D = scene.addChild(new MeshSprite3D(PrimitiveMesh.createBox(1, 1, 1))) as MeshSprite3D;
box.transform.rotate(new Vector3(0, 45, 0), false, false);
var material:BlinnPhongMaterial = new BlinnPhongMaterial();
Texture2D.load("res/layabox.png", Handler.create(null, function(tex:Texture2D):void {
    material.albedoTexture = tex;
}));
box.meshRenderer.material = material;
```


##### 	