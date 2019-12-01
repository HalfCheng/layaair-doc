#Utilisation de HTML divelement en texte riche

**Dans ce document, nous énumérons les problèmes communs aux concepteurs.**

**Comment placer bold, font, fontsize, color et soulignement dans le même texte?**

**Remarque: pour l 'instant, il n' y a pas de support pour l 'intégration des étiquettes italiques, de bordure ou span.**

Voici quelques exemples:


```typescript

var div:HTMLDivElement=new HTMLDivElement();
div.innerHTML="<span style='font-weight:bold;" +
  "font:24px Arial' " +
  "color='red' " +
  "href='https://ask.layabox.com/www.baidu.com'>" +
  "LayaBox</span><span>欢迎你的加入</span>"
Laya.stage.addChild(div);
```


**Comment les polices et les couleurs sont - elles différentes dans le même texte?**

Voici quelques exemples:


```typescript

var htmlD:HTMLDivElement = new HTMLDivElement();
Laya.stage.addChild(htmlD);
htmlD.innerHTML = "<font style='fontSize:30' color='#67fc2c'>测试<br/></font><font style='fontSize:20'>html组件<br/></font>";
```


**Comment obtenir le contenu concret du texte html?(contextwidth, contextheight)?**

Voici quelques exemples:


```typescript

var htmlDiv:HTMLDivElement=new HTMLDivElement();
			var html:String = "<span color='#e3d26a'>使用</span>";
			html += "<span style='color:#FFFFFF;font-weight:bold'>HTMLDivElement</span>";
			html += "<span color='#6ad2e3'>创建的</span><br/>";
			html += "<span color='#d26ae3'>HTML文本</span>";
			htmlDiv.innerHTML=html;
			htmlDiv.pos(50,200);
			var txt:String = "";
			var tTxt:String;
			var tHTMLElement:HTMLElement;
			for(var i:int = 0,n:int = htmlDiv._childs.length;i < n;i++)
			{
				tHTMLElement = htmlDiv.getChildAt(i) as HTMLElement;
				if(tHTMLElement)
				{
					tTxt= tHTMLElement.text;
					if(tTxt)
					{
						txt += tTxt;
					}
				}
			}
			trace("文本内容为"+txt);
			trace("文本的实际宽度为"+htmlDiv.contextWidth,"文本的实际高度为"+htmlDiv.contextHeight)
			Laya.stage.addChild(htmlDiv);
```


* * 4.******
****
**Remarque: l 'alignement vertical central du texte n' est pas actuellement supporté, l 'développeur peut attribuer la valeur (hauteur de l' image - hauteur du texte) / 2 à la valeur y du texte et effectuer un réglage de remplacement de l 'alignement vertical central.******


示例如下：


```typescript

var html3:HTMLDivElement=new HTMLDivElement();
html3.style.lineHeight=30;
html3.style.width=300;
html3.style.align="center";
html3.innerHTML="<br><span>  测试水平居中对齐</span>";
Laya.stage.addChild(html3);
```
****

**Comment établir des hyperliens?******

Voici quelques exemples:


```typescript

var div:HTMLDivElement=new HTMLDivElement();
div.innerHTML="<span href="http://ask.layabox.com/">LayaBox欢迎你的加入！</span>";
div.on(Event.LINK,this,onLink);
Laya.stage.addChild(div);
private function onLink(data:*):void
{
  // TODO Auto Generated method stub
  Browser.window.location.href=data;
}
```
****

**Comment les images peuvent - elles être présentées?******

Voici quelques exemples:


```typescript

var imageHtml:HTMLDivElement=new HTMLDivElement();
imageHtml.innerHTML="<img src="res/boy.png">";
Laya.stage.addChild(imageHtml);
```
****

**Comment réaliser le saut de page HTML?******

Voici quelques exemples:


```typescript

var iHtml:HTMLIframeElement=new HTMLIframeElement();
Laya.stage.addChild(iHtml);
iHtml.href="res/html/test.html";
```
****

**Comment ajouter le texte appendhtml?******

Voici quelques exemples:


```typescript

var appendHtml:HTMLDivElement=new HTMLDivElement();
appendHtml.innerHTML="<span>AAAAAA</span>";
Laya.stage.addChild(appendHtml);
appendHtml.appendHTML("<br>  BBBBBBBBBB");
appendHtml.layout();
```
****

**Espacement des lignes pour htmldivelement, attributs de Leading, attention à la nécessité de définir la valeur = 'Middle'.******

Voici quelques exemples:


```typescript

var t:HTMLDivElement = new HTMLDivElement ;
Laya.stage.addChild(t);
t.style.valign = "middle";
t.size(60, 120);
t.style.wordWrap = true;
t.style.leading = 10;
t.innerHTML = "akshfkjashfkjhakshjdfhkasjdfhsaf";
```
****

**Résolution du problème de la déviation de l'Alphabet anglais sur les téléphones cellulaires de l'IOS (alignement vertical vers le haut des caractéristiques de top dans le style Style) * *

Voici quelques exemples:


```typescript

var html:HTMLDivElement=new HTMLDivElement();
html.innerHTML = "<span style='color:#ffffff;valign:top;'>朋友abc11''31ABC朋友</span><span href='http://www.baidu.com' target='_blank'>百度</span>";
Laya.stage.addChild(html);
```


