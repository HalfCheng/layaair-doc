#拡張スクリプトの使用

プロジェクトの開発では、しばしばこのような状況があります。公式に提供されたコンポーネントは、Buttonのコンポーネント機能を拡張したい、新しい属性を追加したい、IDEの中で新しい属性を設定したいなどの需要を満たすことができません。LayaAirIDE 1.4.0バージョンの後に、拡張スクリプトと追加スクリプトの2つの方法が提供されます。

​**拡張スクリプト:**引き継ぎの方式で、あるコンポーネントの機能を拡張して、自分の論理を実現して、さらには新しい属性を追加して、IDEの中に新しい属性を表示して、可視化の設定は新しい属性を設定します。

​**追加スクリプト:**非継承方式で、追加的に、あるコンポーネントにいくつかの挙動を追加し、新たな属性を追加し、IDEに新しい属性を表示し、可視化の設定に新しい属性を追加します。

上記の拡張方法があると、開発者はコンポーネントの挙動を任意に修正し、新しい属性を追加し、可視化されたUIシーンとコードを結合させることができ、同じシーンで複数の拡張スクリプトを追加することができる。

​**拡張スクリプトと追加スクリプトの違い**：拡張スクリプトはコンポーネント自体から継承され、追加スクリプトはコンポーネント自体にコントロールスクリプトを追加し、現在のコンポーネントの任意の属性を変更できます。引き継ぎません。

​**この記事では、複数のコンポーネントに同じスクリプトを追加し、それらの移動速度と名前を例にして、拡張スクリプトの使い方を詳しく紹介します。最終効果は下図の通りです。**

![0](img\0.gif)(图0)



###一、UIページを作る

ExpandPageというUIページを新規に作成します。UIページにBoxコンポーネントを入れて、Boxコンポーネントに画像とテキストコンポーネントを入れます。テキストコンポーネントのnameをuserNと名付けて、サイズ、配置を設定して保存します。図1に示すように、

![1](img\1.png)（図1）



###二、拡張スクリプトを作成し、コンポーネントに追加して値を付ける

UIパネル管理では、右キー→新規スクリプトを選択し、拡張スクリプト（UI作成ページでスクリプトを選択することができます）を選択し、スクリプトに対応する論理クラス、すなわち以下の実行クラス名を選択します。図2に示すように、

![2](img\2.png)（図2）

確定ボタンをクリックするとプロジェクトパネルに自動的に1つのファイルが作成されます。このファイルには通常の属性がいくつか付いています。属性が追加された時はこれらの属性テンプレートを参照してください。

![3](img\3.png)（図3）

MonkeyPropタグに私たちが必要とする属性を追加すると、図4に示すようになります。

![4](img\4.png)（図4）

拡張スクリプト編集が完了したらUIインターフェースを開き、開発者が変化をより直感的に見るために、ここでBoxをUIインターフェースに複写する。

![5](img\5.png)（図5）

次に作成したMonkeyProp.prop拡張スクリプトを、図6に示すように、Boxにドラッグして配置します。

![6](img\6.gif)（図6）

コンポーネントにドラッグした後、階層リストとUIインターフェースでは変化は見られませんが、Boxコンポーネントの右側の属性欄で追加の属性が見られます。図7に示すように、

![7](img\7.png)（図7）

三つのコンポーネントのspeedとuserNameに値を割り当てて、速度は順に増加して、それぞれ1.2.3に設定して、名前は小さいa、小さいb、cです。つまり、この三つの同じコンポーネントのオブジェクト属性に対して異なる割当を行いました。保存後、ショートカットキーF 12（Ctrl+F 12）を押してUIを導き、コードを作成します。



###三、コード論理編纂

プロジェクトをFlashBuiderに導入した後、ExpandPageUIファイルを開くと、エラーが発生し、game.MonkeyPropが見つかりません。図8に示すように、

![8](img\8.png)（図8）

このエラーは心配しないでください。プロジェクトの中でMonkeyPropスクリプトに対応するロジック類は開発者が自分で作成したものです。まだ作成していないので、エディタが見つからず、エラーが発生しました。

次に、srcディレクトリの下でゲームパッケージを作成し、ゲームパッケージの中でMonkeyPropクラスを作成します。追加すると、図9に示すように、ExpandPageUIファイルのエラーが消えています。

![9](img\9.png)（図9）

MonkeyPropで拡張スクリプトを作成したときに追加された属性は、以下の通りです。


```typescript

package game
{
	import laya.ui.Box;
	import laya.ui.Label;
	/**
	 * 扩展脚本对应的逻辑类
	 * @author mengjia
	 * 
	 */
	public class MonkeyProp extends Box
	{
		/**攻击速度（也可以不用定义该变量，在这里定义是为了打开该类的时候能够一目了然的看到对应的脚本中添加了哪些属性）**/
		public var speed:Number = 0;
		/**人物名称（也可以不用定义该变量，在这里定义是为了打开该类的时候能够一目了然的看到对应的脚本中添加了哪些属性）**/
		public var userName:String = "";
		/**记录状态**/		
		private var boo:Boolean = false;
		public function MonkeyProp()
		{
			//自定义的脚本会有时序问题，所以在此添加一个延时
			this.frameOnce(2,this,onFrame);
		}
		
		private function onFrame():void
		{
			//通过子元素的name值获取该对象
			var userN:Label = this.getChildByName("userN") as Label;
			//设置文本内容为属性栏中给的值
			userN.text = userName;
			this.frameLoop(1,this,onLoop);
		}
		/**
		 *设置帧循环，实现左右移动 
		 * 
		 */		
		private function onLoop():void
		{
			if(x<=0){
				boo = false;
				x+=speed;
			}
			else if(x<Laya.stage.width-this.width && boo == false){
				x+=speed;
			}
			else if(x>=Laya.stage.width-this.width || boo == true){
				x-=speed;
				boo = true;
			}
		}
	}
}
```


最後に、インポートクラスでExpandPageUIページを実行します。**注意：UIインターフェースを実装する前に必要なリソースを事前にロードしなければなりません。**）コードは以下の通りです。


```typescript

package {
	import laya.utils.Handler;
	
	import ui.ExpandPageUI;

	public class LayaSample {
		
		public function LayaSample() {
			//初始化引擎
			Laya.init(600, 700);
			//设置背景色
			Laya.stage.bgColor = "#ffcccc";
			//预加载资源
			Laya.loader.load("res/atlas/test.atlas",Handler.create(this,onLoaded));
			
		}		
		
		private function onLoaded():void
		{
			//实例化UI界面
			var ExpandPage:ExpandPageUI = new ExpandPageUI();
			//添加到stage上
			Laya.stage.addChild(ExpandPage);
			
		}
	}
}
```


最終表示結果は文章の先頭図0に示す通りです。



