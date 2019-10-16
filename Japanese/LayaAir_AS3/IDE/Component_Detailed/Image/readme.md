#Imageコンポーネント参照



##一、LayaAirIDEでImageコンポーネントを作成する

###1.1イメージの作成

ImageはUIで最も一般的な画像を表示するコンポーネントで、ビットマップ画像を表示する。Imageコンポーネントのskin属性を設定して、Imageコンポーネントの表示画像を変更することができます。Imageコンポーネントは、九宮格のデータ設定をサポートして、画像の拡大後の画像表示の歪みがない効果を実現します。

リソースパネルのImageコンポーネントをクリックして、ページ編集エリアにドラッグ＆ドロップして、Imageコンポーネントをページに追加します。選択したImageをクリックすると、プロパティパネルにImageの共通属性の値を設定できます。
Imageコンポーネントのスクリプトインターフェースを参照してください。[Image API](http://layaair.ldc.layabox.com/api/index.html?category=Core&class=laya.ui.Image)。

​**Imageコンポーネントのリソース例：**

​![图片0.png](img/1.png)<br/>
（図1）

​**Imageコンポーネントを編集エリアにドラッグして効果を表示します。**

​![图片0.png](img/2.png)<br/>
（図2）

###1.2 Imageコンポーネントの一般的な属性

​![图片0.png](img/3.png)<br/>
（図3）

𞓜**属性**𞓜**機能説明**𞓜
|---------------------------------------|
|size Grid𞓜ビットマップの有効スケーリンググリッドデータ（九宮格データ）。𞓜
|skin𞓜ビットマップのリソース。𞓜

Imageコンポーネントを追加した後、リソースパネルから画像リソースをImageのskin属性ブロックにドラッグすることにより、Imageコンポーネントの表示リソース画像を変更することができます。

##二、コードでImageコンポーネントを作成する

コードを書く時は、コード制御UIを通して作成することが避けられません。`UI_Image`クラスをコードにインポート`laya.ui.Image`のパケットをコードで設定し、Imageに関する属性を設定します。

**実行例の効果:**
​![5](img/4.png)<br/>
（図5）コードによるImageの作成

Imageの他の属性もコードで設定できます。コードによって異なる肌を創るためのImageの作り方を示します。

興味のある読者は自分でコードを通してImageを設定し、自分の必要に応じた画像を作成することができます。

**サンプルコード:**


```javascript

package
 {
	import laya.display.Stage;
	import laya.ui.Image;
	import laya.webgl.WebGL;
	
	public class UI_Image
	{
		public function UI_Image()
		{
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

			//创建图片
			createImage();			
		}

		/***创建图片***/
		private function createImage():void
		{
			//实例化图片
			var img:Image = new Image("../../../../res/ui/dialog (3).png");
			//设置位置
			img.pos(165, 62.5);
			//加载到舞台
			Laya.stage.addChild(img);
		}
	}
 }
```


