# LayaAir和原生Dom

개발사업에서 개발자는 도m 원소에 대한 지원을 면할 수 있지만, 레이야아에서는 지원하지 않는 불완전한 지원이다.그렇다면 이번 절에는 우리가 개발에서 만난 기교를 살펴보자.

###Layair의 SVG

svg?대부분의 개발자가 이 명사를 듣거나, w3c 가 규정한 벡터 그래픽 형식이라는 것을 알 수 있다.svg 에 대한 일부 정의와 역사에 대해 우리는 더 이상 진술하지 않고 흥미를 느끼는 개발자는 참고할 수 있다[这里](https://ldc.layabox.com/doc/?nav=zh-as-3-4-1).하지만 종목에서 진정으로 쓰이는 곳은 드물다.하지만 svg 의 강함은 무시할 수 없는 간단한 도형, 몇 줄의 텍스트만 설명할 수 있으며, 인터넷의 다운로드는 필요 없다.예를 들어 풍부한 예술자, 예를 들면 기괴한 모양의 도형, 예를 들면 텍스트 의 시스루 효과 등등 프로그램이 실현된다면 어려움을 겪을 수 있다. 예를 들어 아래

![1](img/1.png)</br>


만약 너의 프로젝트 중 이런 문자가 어떻게 처리할 수 있을까?아마도 우리가 생각한 것은 미술 그림이 나올 것이다.그러면 더 쉬운 방법이 없을까요?여기에 우리는 svg 로 처리할 것을 선택합니다.우리는 dom 원소에서 div+css 스타일로 이 효과를 나타내는 것이 가장 간단한 방법인 것을 안다.그렇다면 우리는 css 스타일을 빌려 이런 효과를 보여 준다.다음은 간단한 스크립트가 어떻게 이런 효과를 이루는지 보자.


```javascript

var data = '<svg xmlns="http://www.w3.org/2000/svg" width="200" height="200">' +
           '<foreignObject width="100%" height="100%">' +
           '<div xmlns="http://www.w3.org/1999/xhtml" style="font-size:40px">' +
             '<em>I</em> like ' + 
             '<span style="color:white; text-shadow: 0px 1px 0px #999, 0px 2px 0px #888, 0px 3px 0px #777, 0px 4px 0px #666, 0px 5px 0px #555, 0px 6px 0px #444, 0px 7px 0px #333, 0px 8px 7px #001135;">' +
             'cheese</span>' +
           '</div>' +
           '</foreignObject>' +
           '</svg>';
var DOMURL = window.URL || window.webkitURL || window;
var img = new Image();
var svg = new Blob([data], {type: 'image/svg+xml'});
var url = DOMURL.createObjectURL(svg);
img.src = url;
img.style.position ="absolute";
img.style.zIndex = 99999
document.body.appendChild(img);
```


어떻게 위쪽 이 부호 를 운행합니까?구글 브라우저를 켜고, 공백 웹페이지, F12, 위쪽 코드를 컨트롤대에 붙여 돌리면 위에서 캡처 효과를 볼 수 있다.또는 html 새 코드를 붙여서 브라우저로 열기.쉽지 않은데요?그리고 우리는 임의로 표시된 문자를 수정할 수 있다.개발자는 효과를 수정해 볼 수 있다.우리는 간단하게 이 단락의 코드를 소개한다.이 중 data 는 svg 데이터 형식으로 svg 정의 및 설명을 참고할 수 있습니다.


```javascript

<span style="color:white; text-shadow: 0px 1px 0px #999, 0px 2px 0px #888, 0px 3px 0px #777, 0px 4px 0px #666, 0px 5px 0px #555, 0px 6px 0px #444, 0px 7px 0px #333, 0px 8px 7px #001135;">
//这里是重点，文字的效果我们是通过svg支持的css样式来设置 text-shadow设置的是文字的css样式效果，假如开发者想改变文字的样式，可以修改style即可。
```


위에는 자바스크립트에서 원생의 dom 원소 img 로 나타났습니다. 그러면 게임에서 우리가 쓰고 싶으면 어떻게 할까요?이것은 사실 매우 간단합니다. 현재 우리는 img 으로 페이지에 표시되어 있습니다. 그럼 다음은 어떻게 프로젝트에 적용하고 이 img을 나타낼까요?우리는 새 항목을 건설한다.코드 다음과 같이:


```typescript

class LayaUISample {
    constructor() {
        //初始化引擎
        Laya.init(600, 400);
      	Laya.stage.bgColor = "#ffcccc";
        var data: string = "data:image/svg+xml," + '<svg xmlns="http://www.w3.org/2000/svg" width="200" height="200">' +
            '<foreignObject width="100%" height="100%">' +
            '<div xmlns="http://www.w3.org/1999/xhtml" style="font-size:40px">' +
            '<em>I</em> like ' +
            '<span style="color:white; text-shadow: 0px 1px 0px #999, 0px 2px 0px #888, 0px 3px 0px #777, 0px 4px 0px #666, 0px 5px 0px #555, 0px 6px 0px #444, 0px 7px 0px #333, 0px 8px 7px #001135;">' +
            'cheese</span>' +
            '</div>' +
            '</foreignObject>' +
            '</svg>';
        var sp: Laya.Sprite = new Laya.Sprite();
        sp.loadImage(data, 0, 0, 200, 200);
        Laya.stage.addChild(sp);
    }
}
new LayaUISample;
```


data 를 통해 url 로 로dImage에 전달하는 방법의 엔진은 우리에게 호출을 해 부호화시켜 보이게 된다.loadImage 이 방법의 인자가 주소 수신만 있는 url 은 base64 와 svg 형식을 수용한다.위쪽 코드를 번역하여 우리는 다음 그림 속의 효과를 보았다.

![2](img/2.png)</br>


총괄: 위쪽 코드가 우리에게 좋은 시사와 항목에서 우리의 특수 예술자는 이런 방법으로 더 간단하게 편리할 수 있다.개발자는 자작적으로 더욱 눈부신 효과를 찾을 수 있으며, 예를 들면 3D의 시스루 효과, 그래픽, 그림자, 거꾸로 그림자 등을 찾을 수 있다.이런 방법은 인터넷의 관대를 줄일 뿐만 아니라, 우리가 시시시각각 수정을 편리하게 하는 것이다.한 가지 양식을 설치하여 항목에서 내보내면 모두 응용할 수 있다.만약 위쪽에 있는 방법으로 스티커를 대체하는 방법은 더욱 효율적이고 빠른 것이 아니다.

관련 링크:

[https://codepen.io/pen/；](https://codepen.io/pen/%EF%BC%9B)

[https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API/Drawing_DOM_objects_into_a_canvas；](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API/Drawing_DOM_objects_into_a_canvas%EF%BC%9B)



###Layair의 Dom 원소 Image

html5 에서 image 태그 기능이 강합니다. 특성을 많이 소개하고 있습니다. 간단한 격중 상용 형식을 소개합니다.

####2차원 코드

비교적 흔한 기능은 항목에서 현재 QR코드 주소를 나타내는 것이다.사용자 대장은 식별할 수 있다.여기에서 2차원 코드가 생성되어, 우리는 제3의 js 라이브러리를 빌려 2차원 코드를 생성합니다.베이스 코드 는 다시 기tHub 에 다운로드 할 수 있다. 여기 는 이것 이다[地址](https://github.com/davidshimjs/qrcodejs).

새 항목을 다운로드한 qrcode.js index.html 추가합니다.qrcode api 참조[地址](https://github.com/davidshimjs/qrcodejs).구체적인 논리 코드 다음과 같습니다:


```typescript

class Main {
    //二维码对象
    private qrcode:any;
    private qrcodeSp:Laya.Sprite;
    constructor() {
        //初始化引擎
        Laya.init(600,400);
      	Laya.stage.bgColor = "#ffcccc";
        var div:any = Laya.Browser.document.createElement("div");
        this.qrcode = new Laya.Browser.window.QRCode(div,{
            width:100,
            height:100
        });
        var url:string = "http://layabox.com/";
        this.qrcode.makeCode(url);
        Laya.stage.once("click",this,this.clickHandler);
        this.qrcodeSp = new Laya.Sprite();
        Laya.stage.addChild(this.qrcodeSp);
    }
    private clickHandler():void{
        var url:string = this.qrcode._oDrawing._elImage.src;//获取，注意这里是异步的，开发者可以加个延时在获取。
        this.qrcodeSp.loadImage(url,0,0,100,100);
    }
}
new Main;
```


위쪽 코드를 번역하고 무대에서 클릭하면 2차원 코드가 무대에 나타났는데 휴대전화로 핸드폰을 쓸어 놓고 홈페이지로 옮겼다는 것을 발견했다.**주의자: 이 때 생성된 QR코드는 위신이나 브라우저에서 아무런 반응이 없었습니다. qrcode 가 생성된 것은 canvas 꼬리표가 아니기 때문입니다.**그래서 팝업 인식을 길게 하려면 아이메이지 태그를 사용해야 합니다.이 개발자는 스스로 확장할 수 있다.



###Layair의 Dom 원소 video

####비디오 생방송

html5 시대에 비디오 플레이는 기본적으로 비디오 태그 로 방영되고, 동영상 재생 경험이 없다면 성숙한 재생 플러그인으로 이뤄진다.지금 유행하는 건...[video.js](https://github.com/videojs/video.js),[hls.js](https://github.com/video-dev/hls.js),[plyr.js](https://github.com/Selz/plyr).호환성, 체험과 성능 면에서도 우수하다.이 플러그인의 공식 측은 모두 주어진 데모입니다.예컨대[https://plyr.io/，http://video-dev.github.io/hls.js/demo/，http://codepen.io/sampotts/pen/JKEMqB。](https://plyr.io/%EF%BC%8Chttp://video-dev.github.io/hls.js/demo/%EF%BC%8Chttp://codepen.io/sampotts/pen/JKEMqB%E3%80%82)

다음은 저희가 하겠습니다.[Plyr + hls.js](http://codepen.io/sampotts/pen/JKEMqB)자, 레이야아에서 우리가 어떻게 써야 하는지 보자.

AS 의 빈 프로젝트 새로 만들기.동시에 index.html 파일에 다음과 같은 코드:

![3](img/3.png)</br>>

`<link rel="stylesheet" href="https://cdn.plyr.io/1.8.2/plyr.css">`플레이어의 양식 파일

`<video preload="none" id="player" autoplay="" controls="" crossorigin=""></video>`video 태그 추가.아이디를'player'로 이름을 지었다. 이즈음 우리는 프로그램에 사용될 것이다.

`<script src="https://cdn.plyr.io/1.8.2/plyr.js"></script>`
`<script src="https://cdn.jsdelivr.net/hls.js/latest/hls.js"></script>`

이것은 재생기에서 사용된 유고다.개발자는 생산 환경에서 자신의 프로젝트나 서버에 다운로드하는 것을 기억한다.

다음은 주류의 논리:


```typescript

class LayaUISample {
    constructor() {
        //初始化引擎
        Laya.init(0,0);
        var Hls:any = Laya.Browser.window.Hls;//获取对Hls的引用。
        var plyr:any = Laya.Browser.window.plyr;//获取对plyr的引用
        //获取video对象，就是页面上命名为“player”的标签
        var video:any = Laya.Browser.document.querySelector('#player');
        if(Hls.isSupported()){
            var hls:any = new Hls();
            //加载m3u8源
            hls.loadSource('http://content.jwplatform.com/manifests/vM7nH0Kl.m3u8');
            hls.attachMedia(video);
            hls.on(Hls.Events.MANIFEST_PARSED,function():void{
                    video.play();
            });
        }
        plyr.setup(video);
    }
}
new LayaUISample;
```


번역 코드 실행, 웹 페이지가 이미 동영상을 방영할 수 있다는 것을 발견했다.개발자는 여기에서 우리가 초기화 엔진을 시작할 때 이렇다.

`Laya.init(0,0);//初始化引擎`그리고 사이즈는 0 입니다. 왜냐하면 이곳에는 무대와 교섭하지 않았기 때문입니다.그래서 우리는 0으로 설정하고 심지어 초기화도 할 수 있다.개발자 프로젝트에는 무대와 교차하는 논리가 함유된다면 자신에게 적합한 사이즈를 설정할 수 있다.

재생 중 개발자는 구글의 컨트롤을 통해 Network 태그에서 우리의 동영상을 보시는 ts 파일입니다.

![4](img/4.png)</br>>

재생 진행에 따라 파일의 개수가 갈수록 많아지고 있다.사실 이게 기반이에요.[hls](https://developer.apple.com/streaming/)프로토콜의 재생.이 기술의 기본 원리는 비디오 파일이나 동영상을 조각으로 나누고 인덱스 파일 (m3u8) 을 건립하는 것이다.비디오 디코딩, 비디오 프레임 데이터, 개발자는 다음과 같이 참고할 수 있습니다.

[https://developer.apple.com/streaming/。](https://developer.apple.com/streaming/%E3%80%82)

[https://developer.mozilla.org/zh_CN/docs/Web/API/MediaSource。](https://developer.mozilla.org/zh_CN/docs/Web/API/MediaSource%E3%80%82)

[https://github.com/nickdesaulniers/netfix](https://github.com/nickdesaulniers/netfix)

[https://developer.mozilla.org/en-US/docs/Web/API/HTMLMediaElement](https://developer.mozilla.org/en-US/docs/Web/API/HTMLMediaElement)


上面的例子我们使用hls+plyr来进行播放。其他的方法请开发者参考本教程进行扩展。

####카메라

html5 video 카메라 브라우저 지원도 제한**https 프로토콜**구글과 신판의 위신 지지도는 여전히 괜찮다.만약 당신의 호용성이 그렇게 높지 않다면 카메라의 기능을 시도해 볼 수 있습니다.

다음은 mdn 에서 준 예를 먼저 보도록 하겠습니다.

[https://mdn.github.io/webaudio-examples/stream-source-buffer/](https://mdn.github.io/webaudio-examples/stream-source-buffer/)

개발자는 휴대폰이나 웨이보로 이 주소를 열어 휴대폰의 지지도를 테스트한다.

![5](img/5.png)</br>>

이것은 테스트의 연결이며, 프로토콜도 htps, 개발자는 카메라를 호출할 때 주의해야 한다.자신의 원격 주소는 반드시 https.

더 많은 자료를 참고할 수 있습니다:[https://github.com/mdn/webaudio-examples.。这里的链接是mdn给出的声音和视频的例子。](https://github.com/mdn/webaudio-examples.%E3%80%82%E8%BF%99%E9%87%8C%E7%9A%84%E9%93%BE%E6%8E%A5%E6%98%AFmdn%E7%BB%99%E5%87%BA%E7%9A%84%E5%A3%B0%E9%9F%B3%E5%92%8C%E8%A7%86%E9%A2%91%E7%9A%84%E4%BE%8B%E5%AD%90%E3%80%82)

Layaiair는 카메라에 대해서도 상응하는 재킷이 있는데, 다음은 사용법을 살펴보자.


```typescript

class Main {
    private video:Laya.Video;
    constructor() {
        //初始化引擎
        Laya.init(Laya.Browser.width,Laya.Browser.height);
        if(Laya.Media.supported() === false){
            alert("当前浏览器不支持");
        }
        else{
            this.showMessage();
            var options:any = {
                audio:true,
                video:{
                    facingMode: { exact: "environment" },    // 后置摄像头，默认值就是，不设至也可以。
                    width: Laya.stage.width,
                    height:Laya.stage.height
                }
            };
            Laya.Media.getMedia(options,Laya.Handler.create(this,this.onSuccess),Laya.Handler.create(this,this.onError));
        }
    }
    private showMessage():void{
        var tex:Laya.Text = new Laya.Text();
        Laya.stage.addChild(tex);
        tex.text = "单击舞台播放和暂停";
        tex.color = "#ffffff";
        tex.fontSize = 100;
        tex.valign = "middle";
        tex.align = "center";
        tex.size(Laya.stage.width,Laya.stage.height);
    }
    private onSuccess(url:string):void{
        this.video = new Laya.Video(Laya.stage.width,Laya.stage.height);
        this.video.load(url);
        Laya.stage.addChild(this.video);
        Laya.stage.on("click",this,this.onStageClick);
    }
    private onerror(error:Error):void{
        alert(error.message);
    }
    private onStageClick():void{
        //切换播放和暂停
        if(!this.video.paused){
            this.video.pause();
        }
        else{
            this.video.play();
        }
    }
}
new Main;
```


위의 예를 편집하여 실행할 수 없는 것을 발견하였다.이것은 정상적으로 이 예를 실행하려면 자신이 https 서버를 설치해야 한다.그리고 이 주소를 휴대전화로 켜는 index.html.간단한 htps 서버를 만들기도 간단하다.이곳에서 우리는 레이야의 명령줄 도구를 빌리면 된다.

##-우선 node 를 다운로드합니다.주소 다운로드[https://nodejs.org/en/，进行安装。](https://nodejs.org/en/%EF%BC%8C%E8%BF%9B%E8%A1%8C%E5%AE%89%E8%A3%85%E3%80%82)설치 후 cmd 명령줄을 열고 npm install-g layacd를 입력하십시오.
-우리가 방금 번역한 그 index.html 을 찾았다.shift + 오른쪽 키를 누르고 이 곳에서 cmd 창을 열기 layacmd open 을 입력한 후 http 및 htps 정적 서버를 시작할 것입니다. 명령 행에 따라 출력하는 주소를 누르고, Google 구글 브라우저로 이 주소를 방문합니다.[https://10.10.20.34:8001/index.html。](https://10.10.20.34:8001/index.html%E3%80%82)

###Layair의 dom 원소 File

프로젝트 개발에서 사용자가 사진을 올리게 하는 수요에 사용할 수 있습니다.이것은 html5 의 file 태그를 빌릴 필요가 있다**마이크로편지는 마이크로폰으로 제공하는 인터페이스, 뒤의 교정은 위신교정에서 전문적으로 말한다.다른 브라우저 도 호환성 문제 가 있을 수 있다**무엇다음은 우리가 쓴 간단한 예다.


```typescript

class Main {
    private video:Laya.Video;
    constructor() {
        //初始化引擎
        Laya.init(100,100);
        var file:any = Laya.Browser.document.createElement("input");
        file.type = "file";
        file.style.position = "absolute";
        file.style.zIndex = 999;
        Laya.Browser.document.body.appendChild(file);//添加到舞台
        var fileReader:any = new  Laya.Browser.window.FileReader();
        file.onchange = function(e:any):void
        {
            if(file.files.length){
                fileReader.readAsDataURL(file.files[0]);
            }
        };
        fileReader.onload = function(evt):void
        {  
            if(Laya.Browser.window.FileReader.DONE == fileReader.readyState)
            {
                var sp:Laya.Sprite = new Laya.Sprite();
                sp.loadImage(fileReader.result,0,0,300,300);
                Laya.stage.addChild(sp);
            }
        }
    }
}
new Main;
```


위의 코드 번역, 단추를 누르십시오.사진 파일이나 카메라를 선택하여 사진을 찍어 사진은 이미 무대에 나타났다는 것을 발견했다.그럼 간단한 호출 앨범이나 카메라 프로그램이 이렇게 완성되었습니다.그러나 우리는 이 버튼을 발견하는 것이 매우 추하다.그럼 이 버튼 스타일을 어떻게 바꿀까요? 이것은 cs 스타일을 빌려 처리해야 합니다.전통적인 방법은 이 버튼의 투명치를 0 으로 설정한 후에 그와 중합된 버튼을 넣고 대체하는 것이다.이러한 가상을 통해 그의 양식을 바꾸는 것은 사실 실제적으로 조회한 것은 그이다.단지 사용자가 느끼지 못할 뿐이다.그럼 우리 수정을 한번 해보자.


```typescript

//创建隐藏的file并且把它和按钮对齐。达到位置一致，这里我们默认在0点位置
var file:any = Laya.Browser.document.createElement("input");
//设置file样式
file.style="filter:alpha(opacity=0);opacity:0;width: 150px;height:60px;";
file.type ="file";//设置类型是file类型。
file.accept="image/png";//设置文件的格式为png；
file.style.position ="absolute";
file.style.zIndex = 999;
```


기본 코드:


```typescript

class Main {
    private video:Laya.Video;
    constructor() {
        //初始化引擎
        Laya.init(100,100);
        var skins:any = ["res/a.png"];
        Laya.loader.load(skins,Laya.Handler.create(this,this.onUIAssetsLoaded));
    }
    private onUIAssetsLoaded():void{
        var btn:Laya.Button = new Laya.Button("res/a.png");
        Laya.stage.addChild(btn);

        //创建隐藏的file并且把它和按钮对齐。达到位置一致，这里我们默认在0点位置
        var file:any = Laya.Browser.document.createElement("input");
        //设置file样式
        file.style="filter:alpha(opacity=0);opacity:0;width: 150px;height:60px;";
        file.type ="file";//设置类型是file类型。
        file.accept="image/png";//设置文件的格式为png；
        file.style.position ="absolute";
        file.style.zIndex = 999;
        Laya.Browser.document.body.appendChild(file);//添加到页面；
        var fileReader:any = new  Laya.Browser.window.FileReader();
        file.onchange = function(e:any):void
        {
            if(file.files.length>0)
            {
                fileReader.readAsDataURL(file.files[0]);
            }
        };
        fileReader.onload = function(evt):void
        {  
            if(Laya.Browser.window.FileReader.DONE == fileReader.readyState)
            {
                var sp:Laya.Sprite = new Laya.Sprite();
                sp.loadImage(fileReader.result,0,0,100,100);
                Laya.stage.addChild(sp);
            }
        };
    }
}
new Main;
```


번역 코드, 보기 흉한 dom 버튼이 보이지 않습니다.우리가 사용자 정의 단추를 누르면 그림을 선택해서 무대에 표시할 수 있습니다.

위의 예는 우리가 그것을 원점으로 중합해서 투명도를 0 으로 설정해 보면 알 수 없는 것으로 변장했다.개발자는 다른 위치에 두고 시험해 볼 수 있으며 이번 교정은 구체적으로 실현되지 않는다.file 에 대한 다른 api 는 mdn 과 w3c 관련 설명을 참고하십시오.무대에 올라온 것 외에도 서버를 업로드하는 작업이 있을 수 있으며, 이때 포엠다타로 사용할 수 있다.이 개발자는 시도해 볼 수 있다.

###Layair의 dom 원소 script 태그

때로는 우리 프로젝트의 js 파일이 많습니다. 한꺼번에 모두 가재되어 유량의 낭비가 아니라 페이지의 트턴을 초래할 수 있습니다.압축으로 헷갈리는 방식은 줄일 수 있지만 조금만 큰 프로젝트는 코드 수가 크다.또는 지방의 js 파일, 첫 스크린을 가재할 때 불필요한 경우 적당한 시간을 가재해야 하기 때문에 파일과 모듈을 분할 필요가 있다.문건을 철거하면 즉시 사용할 수 있다.그렇다면 이때 script 라벨은 사용장을 파견한다.

script 의 src 를 통해 원단 스크립트를 다운로드하면 이러한 기능을 실현할 수 있다.script 를 설치하는 innerHTML 을 통해 이뤄질 수도 있고, 물론 세 번째 eval 도 있다.다음 우리는 이 몇 가지 상황에서 각각 용법을 설명한다.

####설정 src 를 통해 실현

script 의 창건은 페이지에 수동적으로 추가할 수 있으며, 코드 동태의 생성도 가능하다.여기에 우리는 코드 창건을 예로 설명한다.코드부터 할게요.

코드 논리는 다음과 같다:


```typescript

class Main {
    private video:Laya.Video;
    constructor() {
        //初始化引擎
        Laya.init(500,500);
        var script:any = Laya.Browser.document.createElement("script");
        script.src = "demo1.js";
        script.onload = function():void{
            //加载完成函数，开始调用模块的功能。
            //new一个js中的对象
            var client:any = new Laya.Browser.window.Demo1();
            client.start();
        }
        script.onerror = function():void{
            //加载错误函数
        }
        Laya.Browser.document.body.appendChild(script);
    }
}
new Main;
```


그리고 js 파일을 새로 만들고 간단한 코드 다음과 같습니다:


```typescript

var Demo1 = (function () {
    function Client() {
    }
    Client.prototype.start = function () {
        // body...
        console.log("调用方法");
    };
    return Client;
})();
console.log("我被加载进来了");
```


다음으로 우리는 이 두 단락의 코드를 간단히 설명한다.

`var script:any = Laya.Browser.document.createElement("script");`script 탭을 생성합니다.

`script.src = "demo1.js";`불러올 js 경로를 설정합니다.

`script.onload = ......`과`script.onerror =....`각각 다운로드 완료와 다운로드 실패의 조정함수입니다.

`Laya.Browser.document.body.appendChild(script);`생성된 script 탭을 페이지에 추가합니다.

`var client:any = new Laya.Browser.window.Demo1();`사실화 js 성명 그 종류.

`client.start();`실례의 함수를 호출하다.

위의 부호를 편집하다.구글의 콘솔을 열면 출력을 볼 수 있습니다:

**"제가 불러왔어요".**

**'호출 방법'.**

####script innerHTML 설정

innerHTML 을 설정하는 것은 사실 js 텍스트 형식을 innerHTML에게 주는 것이다.이것은 파일의 형식을 불러오기 위해 원단으로 복사한 파일을 텍스트 내용의 가치로 바꿀 수 있습니다.다음 예를 보자.


```typescript

class Main {
    private video:Laya.Video;
    constructor() {
        //初始化引擎
        Laya.init(500,500);
        var httpreq:Laya.HttpRequest = new Laya.HttpRequest();
        httpreq.on(Laya.Event.COMPLETE,this,this.completeHandler);
        httpreq.on(Laya.Event.ERROR,this,this.errorHandler);
        httpreq.send("demo1.js");
    }
    private completeHandler(e:any):void{
        var script:any = Laya.Browser.document.createElement("script");
        Laya.Browser.document.body.appendChild(script);
        script.innerHTML = e;
        var client:any = new Laya.Browser.window.Demo1();
        client.start();
    }
    private errorHandler(e:any):void{
        
    }
}
new Main;
```


위쪽 코드를 번역하면 효과와 src 로 가재한 효과를 볼 수 있습니다.이 예는 HtttpRequest로 파일을 다운로드한 다음 추가된 내용을 script.innerHTML 에 부여합니다.탭 실행 js물론 이 예는 HtttpRequest를 사용해 가재할 수 있으며 개발자도 Laya.loader.load 방법으로 가재할 수 있습니다.

####eval 방법 불러오기


```typescript

private completeHandler(e:any):void{
  Laya.Browser.window.eval(e);
  var client:any = new Laya.Browser.window.Demo1();
  client.start();
}
```


우리 는 앞 의 가재 를 함수 로 변경하였다`Laya.Browser.window.eval(e);`그리고 컴파일을 편집하고 콘솔을 열면 효과가 똑같습니다.이것과 script 탭은 이미 아무런 관계가 없다.

총괄: 위의 세 가지 상용 방법은 모두 동적 으로 js 파일을 다운로드할 수 있다.세 가지 방법이 뭐가 달라요?

- script 태그 src 방식으로 js 파일을 다운로드할 수 있습니다. 이 js 파일은 현재 페이지와 동일한 원본으로 다운로드할 수 있습니다.

- script.innerHTML 의 방법은 js 파일의 텍스트 형식으로 XMLHttpRequest 방식으로 다운로드할 수 없으므로 파일을 다운로드할 수 없거나, 이 js 파일이 사용자 파일을 사용자로 정의할 수 있는 형식으로 암호화된 형식으로 2진제 형식을 사용한 다음 프로그램에서 진정한 js 로 호화할 수 있다.

- eval 의 방법과 script.innerHTML 의 방식은 거의 비슷하다.가재된 내용도 자유롭다.그러나 eval 같은 방식을 추천하지 않고 eval 은 곧 폐기되는 방법이다. 성능이나 안전성 면에서도 추천하지 않는 것이다.구체적인 원인은 mdn 의 해석을 보세요.


  [https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/eval。](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/eval%E3%80%82)


  **사실 가재된 방식은 Worker로 이어질 수 있다. 이렇게 하면 페이지의 과장압력과 카톤 현상을 더욱 줄일 수 있다.개발자는 worker 교정을 읽으며 발산할 수 있다.**

###Layair의 dom 원소 목소리

html5 의 목소리를 말하자면 개발자가 첫 번째로 생각하는 것은 audio 태그에 대해 개발사업에 대해 극히 닭갈비를 쓰며 오늘 우리는 다른 인터페이스, HTML5 는 Javascript 프로그래밍을 제공할 수 있는 Audio API 는 코드에서 원시적인 오디오 데이터를 직접 조작할 수 있는 능력을 갖춰 기존 오디오 데이터를 임의적으로 재조해 준다.오디오 api, w3c 나에게 충분한[接口](https://www.w3.org/TR/webaudio/)있다[mdn](https://developer.mozilla.org/zh-CN/docs/Web/API/AudioContext)위에 소개된 것도 상세합니다.비교적 완벽한 브라우저에 목소리를 지원하는 api 는 비주얼이 풍부하다.소리의 api 는 매우 풍부하기 때문에, 우리는 벽돌 인옥, 단순히 용법, 음성 주파수 합성, 음성, 오디오 데이터 분석, 오디오 데이터 분석, 오디오 필터, 음색 등 개발자, mdn 또는 관련 자료를 볼 수 있습니다.

우리는 먼저 mdn 의 예를 보았다.이 예에서 2초 완충기를 생성하고 흰소음으로 채우고[통과]`AudioBufferSourceNode`](https://developer.mozilla.org/zh-CN/docs/Web/API/AudiobufersourceNode)를 방영합니다. 주석에 그 기능을 설명합니다.


```javascript

var audioCtx = new (window.AudioContext || window.webkitAudioContext)();
// Stereo
var channels = 2;
// Create an empty two-second stereo buffer at the
// sample rate of the AudioContext
var frameCount = audioCtx.sampleRate * 2.0;
var myArrayBuffer = audioCtx.createBuffer(2, frameCount, audioCtx.sampleRate);
window.onclick = function() {
  // Fill the buffer with white noise;
  //just random values between -1.0 and 1.0
  for (var channel = 0; channel < channels; channel++) {
   // This gives us the actual ArrayBuffer that contains the data
   var nowBuffering = myArrayBuffer.getChannelData(channel);
   for (var i = 0; i < frameCount; i++) {
     // Math.random() is in [0; 1.0]
     // audio needs to be in [-1.0; 1.0]
     nowBuffering[i] = Math.random() * 2 - 1;
   }
  }
  // Get an AudioBufferSourceNode.
  // This is the AudioNode to use when we want to play an AudioBuffer
  var source = audioCtx.createBufferSource();
  // set the buffer in the AudioBufferSourceNode
  source.buffer = myArrayBuffer;
  // connect the AudioBufferSourceNode to the
  // destination so we can hear the sound
  source.connect(audioCtx.destination);
  // start the source playing
  source.start();
}
```


위에 js 코드를 실행하면 페이지를 누르면 소리가 들려요.그러면 Layair로 어떻게 쓰죠?


```typescript

class Main {
    private video: Laya.Video;
    constructor() {
        //初始化引擎
        Laya.init(500, 500);
        Laya.stage.bgColor = "#ff0000";
        var audioCtx: any = new (Laya.Browser.window.AudioContext || Laya.Browser.window.webkitAudioContext)();
        //Stereo
        var channels: number = 2;
        // Create an empty two-second stereo buffer at the
        // sample rate of the AudioContext
        var frameCount: number = audioCtx.sampleRate * 2.0;
        var myArrayBuffer: any = audioCtx.createBuffer(2, frameCount, audioCtx.sampleRate);
        Laya.stage.on(Laya.Event.CLICK, this, function (): void {
            // Fill the buffer with white noise;
            //just random values between -1.0 and 1.0
            for (var channel: number = 0; channel < channels; channel++) {
                // This gives us the actual ArrayBuffer that contains the data
                var nowBuffering: Object = myArrayBuffer.getChannelData(channel);
                for (var i: number = 0; i < frameCount; i++) {
                    // Math.random() is in [0; 1.0]
                    // audio needs to be in [-1.0; 1.0]
                    nowBuffering[i] = Math.random() * 2 - 1;
                }
            }
            // Get an AudioBufferSourceNode.
            // This is the AudioNode to use when we want to play an AudioBuffer
            var source: any = audioCtx.createBufferSource();
            // set the buffer in the AudioBufferSourceNode
            source.buffer = myArrayBuffer;
            // connect the AudioBufferSourceNode to the
            // destination so we can hear the sound
            source.connect(audioCtx.destination);
            // start the source playing
            source.start();
        });
    }
}
new Main;
```


위에 있는 예를 편집해서 무대를 누르면 소리가 들려요.이 예는 매우 간단해서 메모리 중에 소리를 하나 짓는 것이다.그럼 외부 가재 어떻게 하지?

아래에 있는 이 예는 외부에 소리 파일을 추가합니다.소리의 주파수를 우리는 그려냈다.


```typescript

class Main {
    private AudioContext:any;
    private audioContext:any;
    private analyser:any;
    private audioBufferSourceNode:any;
    constructor() {
        //初始化引擎
        Laya.init(500, 500);
        AudioContext = Laya.Browser.window.AudioContext || Laya.Browser.window.webkitAudioContext;
        this.audioContext = new AudioContext();
        this.analyser = this.audioContext.createAnalyser();
        this.analyser.fftSize = 256;
        Laya.stage.once(Laya.Event.CLICK,this,this.clickHandler);
    }
    private clickHandler(e:any):void
    {
        var http:Laya.HttpRequest = new Laya.HttpRequest();
        http.on(Laya.Event.COMPLETE,this,this.completeHandler);
        http.send("res/3.mp3","","get",Laya.Loader.BUFFER);
    }
    private completeHandler(e:any):void
    {
        this.audioContext.decodeAudioData(e,this.decodeAudioData.bind(this));
    }
    private decodeAudioData(buffer:any):void
    {
        this.audioBufferSourceNode = this.audioContext.createBufferSource();
        this.audioBufferSourceNode.connect(this.analyser);
        this.analyser.connect(this.audioContext.destination);
        this.audioBufferSourceNode.buffer = buffer;
        this.audioBufferSourceNode.start(0);
        Laya.timer.loop(1,this,this.drawHandler);
    }
    private drawHandler():void
    {
        Laya.stage.graphics.clear();
        var dataArray:Uint8Array = new Uint8Array(this.analyser.frequencyBinCount);
        this.analyser.getByteFrequencyData(dataArray);
        var step:number = Math.round(dataArray.length / 60);
        for (var i:number = 0; i < 40; i++) {
            var energy:number = (dataArray[step * i] / 256.0) * 50;
            for (var j:number = 0; j < energy; j++) {
                Laya.stage.graphics.drawLine(20 * i + 2, 200 + 4 * j,20 * (i + 1) - 2, 200 + 4 * j,"#ff0000",1);
                Laya.stage.graphics.drawLine(20 * i + 2, 200 - 4 * j,20 * (i + 1) - 2, 200 - 4 * j,"#ffff00",1);
            }
            Laya.stage.graphics.drawLine(20 * i + 2, 200,20 * (i + 1) - 2, 200,"#ff0000",1);
        }
    }
}
new Main;
```


위에 있는 프로젝트를 번역하여 무대를 누르면 보이고, 소리의 주파수가 나타난다.그림 아래에 제시한 것처럼:

![6](img/6.gif)</br>>

총괄: 웹의 목소리가 점점 강해지는 것을 볼 수 있습니다. 만약 어떤 저단기의 호환성을 고려하지 않으면 완전히 웹의 플레이어를 만들 수 있습니다.여기에는 단지 한 주파수 효과를 냈을 뿐, 개발자는 혼음에 필터 등의 기능을 시험해 볼 수 있다.관련 api 는 mdn 을 검열할 수 있습니다.

###Layair의 dom 원소 iframe

세 개의 사이트에 삽입할 때 우리는 일반적으로 아이프라미까지 사용한다. 심지어 세 쪽의 루트는 기본적으로 아이프라미로 끼워넣는다.우리 프로젝트에서도 아이프라미의 경우.다음 예는 항목에서 iframe 를 적용하는 것이다.

코드 다음과 같이:


```typescript

class Main {
    constructor() {
        //初始化引擎
        Laya.init(500, 500);
        Laya.stage.once(Laya.Event.CLICK,this,this.clickHandler);
    }
    private clickHandler():void{
        var iframe:any = Laya.Browser.document.createElement("iframe");
        iframe.style.position ="absolute";//设置布局定位。这个不能少。
        iframe.style.zIndex = 100;//设置层级
        iframe.style.left ="100px";
        iframe.style.top ="100px";
        iframe.src = "http://ask.layabox.com/";
        Laya.Browser.document.body.appendChild(iframe);
    }
}
new Main;
```


이 안에는 개발자를 일깨워야 하는 것이 바로 위치와 계급을 기억해야 한다.많은 개발자들이 아이프라임이 오락층의 아래로 달려가 보이지 않았다.