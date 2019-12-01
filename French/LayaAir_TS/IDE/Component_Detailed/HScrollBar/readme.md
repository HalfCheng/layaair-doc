# HScrollBar 组件参考



##Création d 'un ensemble hscrollbar par layaairide

###1.1 Création de hsrollbar

L 'ensemble hscrollbar est un ensemble barre de défilement horizontale.
L 'utilisateur final peut utiliser l' ensemble hscrollbar pour commander la partie de données affichée lorsque les données sont trop nombreuses pour que la zone d 'affichage ne soit pas acceptable.
La barre de défilement comprend quatre éléments: un diagramme d 'orbite, un bouton de glissière et deux boutons de flèche.
Cliquez sur l 'ensemble hscrollbar dans le panneau de ressource de sélection et faites glisser - le dans la zone d' édition de page pour ajouter l 'ensemble hscrollbar à la page.
Interface de script pour composants hscrollbar[HScrollBar API](http://layaair.ldc.layabox.com/api/index.html?category=Core&class=laya.ui.HScrollBar)".
Exemples de ressources d 'images d' ensembles hscrollbar:

​![图片0.png](img/1.png)< br / >
(Figure 1)

​![图片0.png](img/2.png)< br / >
(Figure 2)

​![图片0.png](img/3.png)< br / >
(Figure 3)

​![图片0.png](img/4.png)< br / >
(Figure 4)

Après avoir glissé le hscrollbar dans la zone d 'édition, affiche l' effet:

​![图片0.png](img/5.png)< br / >
(Figure 5)

Définit la valeur max de l 'attribut hscrollbar, la valeur min de l' attribut, la valeur valeur valeur valeur valeur valeur valeur valeur valeur valeur valeur valeur de l 'attribut

​![图片0.png](img/6.png)<br/>

(Figure 6)

Pendant l 'exécution du programme, vous pouvez glisser le curseur ou cliquer sur le bouton de la flèche pour commander la valeur de la barre de progression:

​![图片0.gif](gif/1.gif)< br / >
(Figure 7)

Définit l 'effet d' affichage des propriétés de hscrollbar lorsque la valeur de showbuttons est égale à false:

​![图片0.png](img/7.png)< br / >
(Figure 8)

Effets de l'exécution dans le programme:

​![图片0.gif](gif/2.gif)< br / >
(Figure 9)



 



###1.2 caractéristiques usuelles des composants hscrollbar

​![图片0.png](img/8.png)< br / >
(Figure 10)

- 124.**Attribut**- 124.**Description fonctionnelle**- 124.
124 -------------------------------------------------------------------------------------------------------------------
L 'adresse de ressource d' image de la barre de roulement.- 124.
Les données de la grille (données de la grille de la neuvième maison) sont mises à l 'échelle de manière efficace.- 124.
Le nombre de la position de défilement actuelle est exprimé par ‧ renvoie ‧.- 124.
Le nombre correspondant à la position de défilement minimale.- 124.
Le nombre correspondant à la position de défilement maximale.- 124.
$124 scrollsize \ \ \ \ \ \ \ \ \ \ \ \ \ \ \- 124.
124%- 124.
‧ renvoie une valeur booléenne indiquant si une touche est activée, la valeur par défaut étant vraie.- 124.
$124hide \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \- 124.
, cliquez sur le bouton vers le haut et vers le bas pour indiquer si la valeur par défaut est vrai.- 124.



 



##Création d 'un ensemble hscrollbar par Code

Quand on écrit un code d 'écriture, on ne peut pas forcément contrôler l' ui par le Code, créer`UI_ScrollBar`Classe, les attributs associés à hscrollbar sont définis par Code.

**Exécution de l 'exemple:**

​	![5](gif/4.gif)< br / >
(Figure 11) Création de hscrollbar par Code

D 'autres attributs de hscrollbar peuvent également être définis par un code, l' exemple suivant illustre comment le hscrollbar peut être créé par le Code, et les lecteurs intéressés peuvent définir eux - mêmes le hscrollbar par le Code pour créer un hscrollbar correspondant à leurs besoins.

**Exemple:**


```javascript

module laya {
    import Stage = Laya.Stage;
    import Text = Laya.Text;
    import HScrollBar = Laya.HScrollBar;
    import ScrollBar = Laya.ScrollBar;
    import VScrollBar = Laya.VScrollBar;
    import Handler = Laya.Handler;
    import WebGL = Laya.WebGL;
    export class UI_ScrollBar {
        /***水平滚动条资源**/
        private skins:Array<string>=["res/ui/hscroll.png", 
                                  "res/ui/hscroll$bar.png", 
                                 "res/ui/hscroll$down.png",
                                 "res/ui/hscroll$up.png"];
        /***提示信息文本框**/
        private promptText:Text;        
        /****水平滚动条****/
        private hScrollBar:HScrollBar;  
        constructor() {
            // 不支持WebGL时自动切换至Canvas
            Laya.init(800, 600, WebGL);
            //画布垂直居中对齐
            Laya.stage.alignV = Stage.ALIGN_MIDDLE;
            //画布水平居中对齐
            Laya.stage.alignH = Stage.ALIGN_CENTER;
            //等比缩放
            Laya.stage.scaleMode = Stage.SCALE_SHOWALL;
            //背景颜色
            Laya.stage.bgColor = "#232628";
            //加载资源
            Laya.loader.load(this.skins, Handler.create(this, this.onSkinLoadComplete));
        }
         /***加载资源完成***/
        private onSkinLoadComplete(e:any=null):void
        {
            //创建水平滚动条
            this.createHScroller();
        }
        /***创建水平滚动条***/
        private createHScroller():void 
        {
            //实例化垂直滚动条
            this.hScrollBar= new HScrollBar();
            //加载皮肤资源（其他资源根据规范命名后，会自动加载）
            this.hScrollBar.skin = "res/ui/hscroll.png";
            //设置宽度
            this.hScrollBar.width = 400;
            //设置位置
            this.hScrollBar.pos(150, 170);
            //最低滚动位置数字
            this.hScrollBar.min = 0;
            //最高滚动位置数字
            this.hScrollBar.max = 100;
            //滚动变化事件回调
            this.hScrollBar.changeHandler = new Handler(this, this.onChange);
            //加载到舞台
            Laya.stage.addChild(this.hScrollBar);
            //创建提示信息
            this.createPromptText(this.hScrollBar)
        }        
        /***创建提示信息***/
        private createPromptText(scrollBar:ScrollBar):void
        {
            //实例化提示信息
            this.promptText=new Text();
            //提示框字体
            this.promptText.font="黑体";
            //提示框字体大小
            this.promptText.fontSize=26;
            //提示框字体颜色
            this.promptText.color="#FFFFFF";
            //提示框初始文本
            this.promptText.text="您的选择是： ";
            //加载到舞台
            Laya.stage.addChild(this.promptText);
            //设置提示框位置
            this.promptText.pos(scrollBar.x,scrollBar.y-50);
        }
        /***滚动条位置变化回调***/
        private onChange(value:Number):void 
        {
            this.promptText.text= "滚动条的位置： value=" + value;
        }
    }
}
new laya.UI_ScrollBar();
```


