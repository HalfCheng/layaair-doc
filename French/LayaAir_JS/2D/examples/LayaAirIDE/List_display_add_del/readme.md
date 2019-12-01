#Exemple List: affichage, ajout, suppression

List (liste) est une fonction plus courante.This paper Combined layaair Engine and IDE to LIST DISPLAY, increase, delete, etc.(créer des opérations de base telles que les projets, voir d 'autres documents, cet article saute)

###I. Établissement ui avec layaairide

#####1.1 Création d 'une page ui du type View



​        ![1](img/1.png)
(Figure 1) Création d 'une page ui de type view, appelée listpage

Tout d 'abord, nous avons créé une page ui de type View dans le gestionnaire de projet de layaairide, d' une largeur de 640 * 1136.La page s' appelle listpage.

#####1.2 importation de ressources UI

Les ressources d 'une page ui de création artistique sont importées dans le gestionnaire de ressources.(pour les importations spécifiques, voir les documents importés par les ressources layaairide.



​        ![2](img/2.png)
(Figure 2)

#####1.3 présentation de l'historique List à l'aide de la grille de Jiujiang

**1.3.1 tirer List sur la scène**



​      ![3](img/3.png)
(Figure 3) glisser l 'arrière - plan de l' image BG \ \ u list.png sur la scène

​**1.3.2 l 'attribut image est défini par l' attribut sizegrid.**



​      ![4](img/4.png)
Diagramme 4. Cliquez sur le bouton droit de l 'attribut sizegrid pour ouvrir le panneau de configuration de la grille

​**1.3.3 paramètres dans les propriétés avec une longueur d 'écran totale de 640**

​![5](img/5.png)
(Figure 5)





 #####1.4 fabrication de récipients List

**1.4.1 tirer le checkbox.png sur la scène et définir le nom de l 'attribut comme Check.**

​![6](img/6.png)
(Figure 6)

​**1.4.2 faites glisser un label.png sur la scène et définissez le nom de l 'attribut listnumber, les autres attributs renvoyant à la figure 7.**

​![7](img/7.png)
Figure 7 ensemble label pour numéros de série

​**1.4.3 faites glisser un autre label.png sur la scène et modifiez le texte en "texte illustré List", les attributs étant définis par référence à la figure 8:**

​![8](img/8.png)
Figure 8 ensemble label pour le texte List

​**1.4.4 sélectionnez la carte de base List, le numéro de série label, le texte label et le checkbox pour créer un box avec le raccourci Ctrl + B.Sélectionnez ensuite le box et définissez les propriétés de Box rendertype comme render.Voir fig. 9, fig. 10.**

​![9](img/9.png)
(Figure 9)



​     ![10](img/10.png)
(Figure 10)

​**1.4.5 cliquez sur Box pour créer un récipient List à nouveau par Ctrl + B, comme la figure 11.Notez que tous les récipients List doivent être basés sur des Box et que la hiérarchie de la figure 12 ci - dessous sera plus claire, list étant produit à partir du cycle box.**

​![11](img/11.png)
(Figure 11)

​![12](img/12.png)
(Figure 12)

#####1.5 paramètres des propriétés List

Sélectionnez le récipient List, définissez les attributs lis var comme étant \ \ list (par cette variable tous les attributs de l 'ensemble peuvent être appelés), puis définissez d' autres attributs en fonction des besoins réels, repeatx est le nombre de listes de l 'axe X, repeaty est le nombre de listes de l' Axe Y, SpaceX est l 'espacement de listes de l' axe X et Spacey l 'espacement de listes de l' axe Y.Comme le montre la figure 13:

​![13](img/13.png)
(Figure 13)

#####1.6 ajout de boutons

Ici, nous utilisons directement le modèle buttontab, nous le remorquons sur la scène, puis cliquez sur le Sous - noeud d 'entrée, réglez les propriétés var, label et la grille de neuvième palais.Figure 14, figure 15



​        ![14](img/14.png)
(Figure 14)

​![15](img/15.png)
(Figure 15)

Ajuster les détails de l'emplacement de l'ui, comme indiqué à la figure 16.F12 publie l'ui, et on entre dans la phase de code.

​![16](img/16.png)
(Figure 16)



###Mise en œuvre de la logique de code list dans la langue JavaScript

#####2.1 affichage des pages ui produites

2.1.1 créer un fichier listdemo.js et définir les JS correspondants comme un fichier d 'initialisation à l' entrée index.html.

​![17](img/17.png)
(Figure 17)

2.1.2 Éditer le Code pour afficher l'ui.

Nous introduisons d 'abord le chargement et les catégories ui, puis le chargement des ressources d' Atlas pour afficher l 'ui, et enfin l' interface ui et l 'ajout à la scène.Ces trois éléments sont réalisés par codage:


```typescript

(function()
{
    var Stage= Laya.Stage;
    var Handler= Laya.Handler;
    var Loader= Laya.Loader;
    var WebGL = Laya.WebGL;
     
     var ListDemoView;
    (function()
    {
         Laya.init(640,1136,WebGL);
         Laya.stage.bgColor = "#ffffff";
          
         Laya.stage.scaleMode = Stage.SCALE_SHOWALL;
         //预加载资源文件后执行回调
         Laya.loader.load(["res/atlas/ListPage.atlas","res/atlas/template/ButtonTab.atlas"], Handler.create(this, onLoaded));
       
    })();
  
    function onLoaded(){
        ListDemoView = new ListPageUI();
        Laya.stage.addChild(ListDemoView);
    }
     
})();
```


​*Le trajet de l 'Atlas dans le code doit être ajusté avec souplesse en fonction de la réalité du projet.*



2.1.3 lorsque le codage est terminé, l'édition du Code logique commence avec l'exécution F5, comme le montre la figure 18, lorsque l'affichage des pages et la production de l'IDE sont compatibles.



​        ![18](img/18.png)
(Figure 18)

#####2.2 Élaboration de la logique de code

​**2.2.1 mise en œuvre de la logique List**

Pour obtenir l 'ajout de données du numéro de série List, il faut utiliser la source de données list dans l' API "laya.ui.list", le processeur de rendu de cellule renderhandler et le procédé permettant d 'obtenir des objets du sous - noeud par le nom du sous - noeud dans l' API "laya.display.node".Nous passons d'abord à la note API: voir les figures 19, 20 et 21.

​![19](img/19.png)
(Figure 19)

​![20](img/20.png)
(Figure 20)

​![21](img/21.png)
(Figure 21)


 **List, ajouter le code suivant:**


```typescript

(function()
{
    var Stage= Laya.Stage;
    var Handler= Laya.Handler;
    var Loader= Laya.Loader;
    var WebGL = Laya.WebGL;
     
     var ListDemoView;
     var arr;
    (function()
    {
         Laya.init(640,1136,WebGL);
         Laya.stage.bgColor = "#ffffff";
          
         Laya.stage.scaleMode = Stage.SCALE_SHOWALL;

          //预加载资源文件后执行回调
         Laya.loader.load(["res/atlas/ListPage.atlas","res/atlas/template/ButtonTab.atlas"], Handler.create(this, onLoaded));
       
    })();
  
    function onLoaded(){
        ListDemoView = new ListPageUI();
        Laya.stage.addChild(ListDemoView);
        //获得List模拟数据，并渲染
         getListData(); 
    }
  
    function getListData(){
        //添加list数据
        arr = [];
        for (var i  = 1; i <= 30; i++) {
            arr.push({listNumber: {text:i}});
           }
      
        ListDemoView._list.vScrollBarSkin='';//添加list滚动条功能（UI不可显示）
        ListDemoView._list.array = arr;//数据赋值
         //list渲染函数
          ListDemoView._list.renderHandler = new Handler(this, onRender);
    }
  
    function onRender(cell,index){
         //如果索引不再可索引范围，则终止该函数
        if(index > arr.length)return;
        //获取当前渲染条目的数据
        var data = arr[index];
        //根据子节点的名字listNumber，获取子节点对象。         
        var listNumber = cell.getChildByName("listNumber") ;
        //label渲染列表文本（序号）
        listNumber.text=data.listNumber.text;
    }

})();
```


Le résultat de l 'exécution du Code, tel qu' il ressort de la figure 22, a permis d 'obtenir l' entrée de données de numéro de série.Realization of the Logic and Code Description to direct Visual Code and annotations.

​![22](img/22.png)
(Figure 22)

2.2.2 réalisation de défilement List
Trente données analogiques ne peuvent être vues que 16 fois que l 'exemple ci - dessus a été exécuté.Il faut donc ajouter un effet de défilement.Vsscrollbarskin dans l 'API de laya.ui.list peut répondre à nos besoins, comme le montre la figure 23:

​![23](img/23.png)
(Figure 23)
L 'ajout de cette fonction n' exige qu 'une seule ligne de code pour ne pas coller tous les codes, et le code suivant est placé avant la source de données de la liste d' attribution.


```typescript

//添加list滚动条功能
listView._list.vScrollBarSkin='';
```


Les effets de la réactivation sont indiqués à la figure 24:



​        ![24](img/24.png)
(Figure 24)

2.2.3 Mise en œuvre de la fonction List

L 'augmentation du nombre de List nécessite l' utilisation de la méthode d 'écoute d' événements dans le moteur layaair laya.display.sprite () pour l 'interception d' événements Click de souris et l 'adjonction de sources de données de cellules dans laya.ui.list.api ();

​![25](img/25.png)
(Figure 25)

​![26](img/26.png)
(Figure 26)



 
```typescript

(function()
{
    var Handler= Laya.Handler;
    var Loader= Laya.Loader;
    var WebGL = Laya.WebGL;
    var Event   = Laya.Event;
    var Stage = Laya.Stage;
     
     var ListDemoView;
     var arr;
    (function()
    {
         Laya.init(640,1136,WebGL);
         Laya.stage.bgColor = "#ffffff";
          
         Laya.stage.scaleMode = Stage.SCALE_SHOWALL;

          //预加载资源文件后执行回调
         Laya.loader.load(["res/atlas/ListPage.atlas","res/atlas/template/ButtonTab.atlas"], Handler.create(this, onLoaded));
       
    })();
  
    function onLoaded(){
        ListDemoView = new ListPageUI();
        Laya.stage.addChild(ListDemoView);
        //获得List模拟数据，并渲染
         getListData(); 
         //侦听增加按钮点击事件
         ListDemoView.add.on(Event.CLICK,this,onAddClick);
    }
  
    function getListData(){
        //添加list数据
        arr = [];
        for (var i  = 1; i <= 30; i++) {
            arr.push({listNumber: {text:i}});
           }
      
        ListDemoView._list.vScrollBarSkin='';//添加list滚动条功能（UI不可显示）
        ListDemoView._list.array = arr;//数据赋值
         //list渲染函数
          ListDemoView._list.renderHandler = new Handler(this, onRender);
    }
  
    function onRender(cell,index){
         //如果索引不再可索引范围，则终止该函数
        if(index > arr.length)return;
        //获取当前渲染条目的数据
        var data = arr[index];
        //根据子节点的名字listNumber，获取子节点对象。         
        var listNumber = cell.getChildByName("listNumber") ;
        //label渲染列表文本（序号）
        listNumber.text=data.listNumber.text;
    }
      function onAddClick(){
         //添加单元格数据源
         ListDemoView._list.addItem({listNumber: {text:arr.length+1}});
    }

})();
 ```


Pour plus de détails, consulter directement le Code et les notes.


Les effets de l'opération du Code sont illustrés à la figure 27:

​![27](img/27.png)
Figure 27 accroissement de l 'effet de la Liste



2.2.3 réalisation des fonctions List ajoutées et supprimées

Pour réaliser la fonction List de suppression, il faut réaliser la fonction checkbox de la case à cocher, la détection de la souris du bouton Supprimer et le rendu des données après l 'opération de suppression.Pour plus de détails, consulter directement le Code et les notes:



 
```typescript

(function()
{
    var Handler= Laya.Handler;
    var Loader= Laya.Loader;
    var WebGL = Laya.WebGL;
    var Event   = Laya.Event;
     var Stage = Laya.Stage;
     var CheckBox = Laya.CheckBox;
     
     var ListDemoView;
     var arr;
    (function()
    {
         Laya.init(640,1136,WebGL);
         Laya.stage.bgColor = "#ffffff";
          
         Laya.stage.scaleMode = Stage.SCALE_SHOWALL;

          //预加载资源文件后执行回调
         Laya.loader.load(["res/atlas/ListPage.atlas","res/atlas/template/ButtonTab.atlas"], Handler.create(this, onLoaded));
       
    })();
  
    function onLoaded(){
        ListDemoView = new ListPageUI();
        Laya.stage.addChild(ListDemoView);
        //获得List模拟数据，并渲染
         getListData(); 
         //侦听增加按钮点击事件
         ListDemoView.add.on(Event.CLICK,this,onAddClick);
         //侦听删除按钮点击事件
         ListDemoView.del.on(Event.CLICK,this,onRemoveClick);
    }
  
    function getListData(){
        //添加list数据
        arr = [];
        for (var i  = 1; i <= 30; i++) {
            arr.push({listNumber: {text:i}});
           }
      
        ListDemoView._list.vScrollBarSkin='';//添加list滚动条功能（UI不可显示）
        ListDemoView._list.array = arr;//数据赋值
         //list渲染函数
          ListDemoView._list.renderHandler = new Handler(this, onRender);
          //mouseHandler: list单元格鼠标事件处理器
          ListDemoView._list.mouseHandler = new Handler(this,onMouse);
    }
  
    function onRender(cell,index){
         //如果索引不再可索引范围，则终止该函数
        if(index > arr.length)return;
        //获取当前渲染条目的数据
        var data = arr[index];
        //根据子节点的名字listNumber，获取子节点对象。         
        var listNumber = cell.getChildByName("listNumber") ;
        //label渲染列表文本（序号）
        listNumber.text=data.listNumber.text;
        //获取当前渲染条目的check组件
        var check=cell.getChildByName("check");
        //根据isCheck的值，确定当前check组件是否为勾选状态（可以避免出现其他多余的选中状态）
        if(data.isCheck)
        {
                check.selected=true;
   
        }
        else
        {
                check.selected=false;
        }
    }
  
      function onRemoveClick(){
      //创建一个新的数组，存放移除条目后的数据
      var temp= [];
      for(var i=0;i<arr.length;i++)
      {
      //将非选中状态的条目数据存储起来
            if(!arr[i].isCheck)
            {
                  temp.push(arr[i]);
            }
      }
      arr = temp;
      //将新的数组赋值给list
      ListDemoView._list.array = arr;
    }
  
      function onAddClick(){
         //添加单元格数据源
         ListDemoView._list.addItem({listNumber: {text:arr.length+1}});
    }

      function onMouse(e,index)
      {
         //鼠标单击事件触发
         if(e.type == Event.CLICK)
         {
              //判断点击事件类型,如果点中的是checkBox组件执行
            if((e.target) instanceof CheckBox)
            {
                 //记录当前条目所包含组件的数据信息(避免后续删除条目后数据结构显示错误)
                var tempObj = arr[index];
                 //根据check的选中状态，设置条目的数据信息
                 if((e.target).selected)
                 {
             ListDemoView._list.setItem(index,{listNumber:{text:tempObj.listNumber.text} ,isCheck:true});
                 }
                 else
                 {
             ListDemoView._list.setItem(index,{listNumber:{text:tempObj.listNumber.text},isCheck:false});
                 }
            }
         }
      }
  
})();
 ```


Les effets opérationnels sont indiqués à la figure 28:

​![28](img/28.png)
(Figure 28) effet de la suppression des articles 2, 3 et 4
​

Ainsi, nous avons terminé la production de l 'ui de la liste, ainsi que l' affichage, l 'ajout et la suppression de la logique de code.S'il y a des questions, veuillez les adresser à la communauté: ask.layabox.com.



