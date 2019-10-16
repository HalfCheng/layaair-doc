#スクリーンショットはどうしますか

プロジェクト開発の過程でスクリーンショットの需要がよくあります。例えば、スクリーン上のコンテンツを切り取って表示したり、共有したり、二次描画したりします。

LayaAirの中のSpriteクラスでは、スクリーンショットの需要を実現するためにドラフトTonvas方法を提供しています。APIは図1に示すように、

![1](img\1.png)（図1）

APIからは、drawTonvasは全部で二つの使い方があることが分かります。一つは**描画した画像を画像ソースとして、他のSpriteに描画します。**一つは**オリジナルの画像データを取得し、インターネットに共有し、スクリーンショット効果を実現します。**。次にコードの例を用いてこの2つの機能を実現する。

###1、他のSpriteにカットした写真を描きます。

2つのSpriteをstageに実装し、元の画像を表示するために使用し、切り取り用の画像を表示します。すべてのコードは以下の通りです。


```typescript

// 程序入口
class GameMain {
    private sp: Laya.Sprite;
    constructor() {
        Laya.init(800, 600, Laya.WebGL);
        //实例化一个sprite，用来显示原始图片
        this.sp = new Laya.Sprite();
        this.sp.loadImage("res/a.png");
        Laya.stage.addChild(this.sp);

        //给stage添加一个点击事件，点击之后截取原始图片中的一部分
        Laya.stage.on(Laya.Event.CLICK, this, this.onClick);
    }
    private onClick(): void {
        //定义一个HTMLCanvas来接收截屏返回的HTMLCanvas对象；截取原始图片中从0,0坐标开始的100*100部分图片
        var htmlC: Laya.HTMLCanvas = this.sp.drawToCanvas(100, 100, 0, 0);
        //获取截屏区域的texture
        var interceptT: Laya.Texture = new Laya.Texture(htmlC);
        var spDeposit: Laya.Sprite = new Laya.Sprite();
        //绘制截取的纹理
        spDeposit.graphics.drawTexture(interceptT, 0, 0, 100, 100);
        //设置显示容器的坐标
        spDeposit.x = 300;
        Laya.stage.addChild(spDeposit);
    }
}
new GameMain();
```


動作効果は図2に示すようになります。

![2](img\2.gif)（図2）



###2、共有スクリーンデータを保存する

画像データを保存してサーバに送信します。すべてのコードは以下の通りです。


```typescript

// 程序入口
class GameMain {
    private sp: Laya.Sprite;
    constructor() {
        Laya.init(800, 600, Laya.WebGL);
        //实例化一个sprite，用来显示原始图片
        this.sp = new Laya.Sprite();
        this.sp.loadImage("res/a.png");
        Laya.stage.addChild(this.sp);

        //给stage添加一个点击事件，点击之后截取原始图片中的一部分
        Laya.stage.on(Laya.Event.CLICK, this, this.onClick);
    }
    private onClick(): void {
        //定义一个HTMLCanvas来接收截屏返回的HTMLCanvas对象；截取原始图片中从0,0坐标开始的100*100部分图片
        var htmlC: Laya.HTMLCanvas = this.sp.drawToCanvas(100, 100, 0, 0);
        //获取原生的canvas对象
        var canvas:any = htmlC.getCanvas();
        //打印图片base64信息，可以发给服务器或者保存为图片
        console.log(canvas.toDataURL("image/png"));
    }
}
new GameMain();
```


stageをクリックしてから、出力されたbase 64の情報を見ることができます。図3に示すように、

![3](img\3.gif)（図3）



LayaNativeでスクリーンショットを実現したらジャンプしてください。[这里](https://ldc.layabox.com/doc/?nav=zh-ts-7-2-7)