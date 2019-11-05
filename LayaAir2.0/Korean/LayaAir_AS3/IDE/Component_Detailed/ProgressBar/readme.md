#Progressbar 구성 요소 참조



##하나, LayairID를 통해 ProgressBar 구성 요소 만들기

###1.1 프로젝트 만들기

ProgressBar 는 게임 중 한 조작의 진도를 나타낸다. 예를 들어 자원의 진도, 역할 경험 또는 혈량의 진도를 나타낸다.
자원 패널에 있는 ProgressBar 구성 요소를 누르면 페이지 편집 영역에 끌려 Progressbar 구성 요소를 페이지에 추가할 수 있습니다.
Progressbar 스크립트 인터페이스 참조[ProgressBar API](http://layaair.ldc.layabox.com/api/index.html?category=Core&class=laya.ui.ProgressBar).

ProgressBar 구성 요소의 자원 표시:

​![图片0.png](img/1.png)< br >>
(그림 1)

​![图片0.png](img/2.png)< br >>
(2)

ProgressBar 구성 속성 value 의 값을 0.3으로 표시한 후 다음과 같습니다:

​![图片0.png](img/3.png)< br >>
(그림 3)



  



###1.2 Progressbar 구성의 상용 속성

​![图片0.png](img/4.png)< br >>
(그림 4)

124대**속성**124대**기능 설명**124대
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
124테네지드 (sizegrid) 의 진도 (I) 의 그림의 효과적으로 격자 데이터를 축소합니다.124대
124사 스킨의 진도 사진자원.124대
124테르untime
124사이클



 



##둘째, 코드 를 통해 Progressbar 만들기

Google은 코드를 쓸 때, 코드 제어 UI, UI, u Progresbar 종류를 만들 수 없습니다`laya.ui.ProgressBar`패키지는 코드를 통해 Progressbar 관련 속성을 설정합니다.

**실행 실례 효과:**
​![5](gif/1.gif)< br >>
(그림 5) 코드를 통해 Progressbar 만들기

ProgressBar 의 다른 속성도 코드 를 통해 설정할 수 있다. 이러한 예례는 코드 를 통해 서로 다른 피부를 만들 수 있는 Progresbar, 흥미가 있는 독자들은 코드를 통해 ProgressBar를 설정할 수 있으며, 자신에게 필요한 프로그립스Bar를 만들 수 있다.

**예시 코드:**


```javascript

package
{
	import laya.display.Stage;
	import laya.ui.ProgressBar;
	import laya.utils.Handler;
	import laya.webgl.WebGL;
	
	public class UI_ProgressBar
	{
		private var progressBar:ProgressBar;
		
		public function UI_ProgressBar()
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
			
			//加载资源
			Laya.loader.load(["../../../../res/ui/progressBar.png", "../../../../res/ui/progressBar$bar.png"], Handler.create(this, onLoadComplete));
		}
		
		/***加载资源完成***/
		private function onLoadComplete():void
		{
			//实例化进度条
			progressBar = new ProgressBar("../../../../res/ui/progressBar.png");
			//设置宽度
			progressBar.width = 400;
			//设置显示位置，在舞台居中
			progressBar.x = (Laya.stage.width - progressBar.width ) / 2;
			progressBar.y = Laya.stage.height / 2;
			
			//设置九宫格边距，以防变形
			progressBar.sizeGrid = "5,5,5,5";
			//数据改变时回调方法
			progressBar.changeHandler = new Handler(this, onChange);
			//加载到舞台
			Laya.stage.addChild(progressBar);
			
			//时间间隔循环，每100毫秒改变一次数据
			Laya.timer.loop(100, this, changeValue);
		}
		
		/***时间间隔循环回调，更新进度条***/
		private function changeValue():void
		{
			//最大为1，每次间隔更新5%
			if (progressBar.value >= 1)
				progressBar.value = 0;
			progressBar.value += 0.05;
		}
		
		/***进度条数据改变回调***/
		private function onChange(value:Number):void
		{
			trace("进度：" + Math.floor(value * 100) + "%");
		}
	}
}
```


