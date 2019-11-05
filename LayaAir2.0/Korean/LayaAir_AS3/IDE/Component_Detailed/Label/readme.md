#Label 구성 참조



##하나, LayairierIDE 로 Label 구성 요소 만들기

###1.1 Label 생성

Label 구성 요소는 텍스트를 표시하고, 텍스트는 시스템 글꼴이나 BMFont 글꼴로 사용할 수 있습니다.

자원 패널에 있는 Label 구성 요소를 누르고 페이지 편집 영역으로 끌면 Label 구성 요소를 페이지에 추가할 수 있습니다.
Label 구성된 스크립트 인터페이스 참고해주세요.[Label API](http://layaair.ldc.layabox.com/api/index.html?category=Core&class=laya.ui.Label).

Label 구성 요소의 자원 예제:



​        ![图片0.png](img/1.png)< br >>
(그림 1)

Label 구성 요소를 편집구에 끌어 놓고 text 속성값을 설정하는 것은 Layaiair IDE 를 다음과 같습니다:

​![图片0.png](img/2.png)< br >>
(2)



 

 



###1.2 Label 속성

​![图片0.png](img/3.png)< br >>
(도)

124대**속성**기능 설명
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
124테xt  124테오 텍스트 내용 문자열.124대
124테로르의 텍스트 색상 값기본 검은색입니다.124대
텍스트가 굵은 글자에 표시되지 않느냐를 124테입니다.124대
124테오  텍스트 글꼴 이름124대
124테오  fontsize 텍스트 크기124대
1244 align  124테의 수평 정렬 방식.left, center, right.124대
124대 overflow
124테르untime
124사이클



 



##둘째, 코드 로 Label 구성 요소 만들기

우리가 쓴 코드를 쓸 때, 코드 제어 UI, 생성할 수 없다`UI_Label`종류, 코드 중 가져오기`laya.ui.Label`가방은 코드 설정을 통해 Label 관련 속성을 설정합니다.

**실행 실례 효과:**
​![5](img/4.png)< br >>
(그림 5) 코드 로 Label 만들기

Label 의 다른 속성도 코드 를 통해 설정할 수 있으며, 다음은 코드 코드 를 통해 다른 피부(스타일)를 만드는 Label, 흥미를 가진 독자들은 코드 설정을 통해 Label, 자신에게 필요한 텍스트 효과를 생성할 수 있다.

더 많은 텍스트 효과는 2D 기초 편에 있는 텍스트 부분을 살펴볼 수 있다.

**예시 코드:**


```javascript

package 
{
	import laya.display.Stage;
	import laya.ui.Label;
	import laya.webgl.WebGL;
	
	public class UI_Label
	{
		public function UI_Label()
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

			//创建多个label文本
			createMoreLabel();			
		}

		/***创建多个label文本***/
		private function createMoreLabel():void
		{
			//创建各种文本效果并设置位置
			createLabel("#FFFFFF", null).pos(30, 50);
			createLabel("#00FFFF", null).pos(290, 50);
			createLabel("#FFFF00", "#FFFFFF").pos(30, 100);
			createLabel("#000000", "#FFFFFF").pos(290, 100);
			createLabel("#FFFFFF", "#00FFFF").pos(30, 150);
			createLabel("#0080FF", "#00FFFF").pos(290, 150);
		}
		
		/**
		 * 创建Label文本
		 * @param color 	         文字颜色
		 * @param strokeColor  文字描边颜色
		 */		
		private function createLabel(color:String, strokeColor:String):Label
		{
			//实例化label文本
			var label:Label = new Label();
			//设置文本字体
			label.font = "Microsoft YaHei";
			//设置文本内容
			label.text = "SAMPLE DEMO";
			//设置文本字体大小
			label.fontSize = 30;
			//设置文本字体颜色
			label.color = color;
			
			//如果有描边颜色参数
			if (strokeColor)
			{
				//文本描边宽度为4
				label.stroke = 4;
				//设置文本描边颜色
				label.strokeColor = strokeColor;
			}
			//加载到舞台
			Laya.stage.addChild(label);
			
			return label;
		}
	}
}
```








 	