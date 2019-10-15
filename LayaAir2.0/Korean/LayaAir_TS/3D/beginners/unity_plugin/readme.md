##유닛 플러그인 사용

>>###중요한 힌트:

###Layaiair 1.x 버전의 엔진, 유닛 5.6.x.그래서 개발자가 유닛 5.6.x 버전을 다운로드해 주세요.다른 버전에 대해서는 일부 호환되지 않는 경우가 있을 수 있다.



###Layair3D 내보내기 도구 다운로드

주소 https://ldc2.layabox.com/layadownload/? type= layaide-Layaiaiaiaiaiaiaiaiaiaia% 202.0%202.0%20beta3

또는 ide 아래 메뉴-------3D 변환 도구

다운로드가 완료된 후 두 가지 도구를 볼 수 있습니다. FBXX 형식의 FBXTools 도구를 바꿀 수 있습니다. 이 도구는 당분간 갱신을 하지 않습니다. 개발자들이 다른 유닛을 내보내는 플러그인 도구를 사용하여 개발자들의 게임 세계를 만들기 편리합니다.

![图片1](img/1.png)< br > (그림 1)



###내보내기 플러그인 설치

유닛, 새 프로젝트를 시작하고, 게임에 필요한 자원과 재질, 스티커 등을 도입하고 항목 명칭은 자신의 필요에 따라 명명할 수 있다.crl + s 가 우리의 장면을 저장합니다. 우리는 이름을 truck 이라고 합니다.

자원 관리 인터페이스에서 Layair3D 변환 도구를 가져옵니다.플러그인 버전은 레이야아 엔진 기능이 증가함에 따라 업데이트되지만 도입하는 방법은 완전히 일치한다.

도구를 가져오는 데 성공하면 자원 관리 인터페이스에서 Layaplugin 폴더가 나타날 수 있으며 유닛 메뉴 표시줄에도 플러그인 메뉴를 내보내기 위한 Layaplugin.그림 2:

![动图2](img/2.gif)< br > (그림 2)

​

###자원 설정 내보내기

우리는 유닛에서 자동차 모형을 만들어 낸 다음 Layair의 플러그인을 사용합니다.메뉴 표시줄 Layaplugin을 누르면 패널 내보내기 위해 자세하게 설명할 것입니다.

![动图3](img/3.gif)< br > (그림 3)



####자원 분류 내보내기

**Sce 분류**장면 전체의 장면, 장면 속 모형, 소재, 스티커, 애니메이션, 광샷 모두 내보내며, 주로 세트제작에 사용되고, 파일 확장자는 Scene 종류나 상속류로 가재되어야 한다.

**Sprite3D 분류**장면보다 스냅샷의 내보내기 때문에 캐릭터나 게임에서 활동하는 단독 자원 내보내기, 파일 확장명은 Lh, Spite3D 로 가재합니다.

그것들의 가재는 ‘ 3D 기술 문서 — Layair3D의 모형 편에 소개될 것이다.

####메시 Setting

체크 데이터의 내보내기 설정을 선택하면 두 가지 정보 (그림 4) 가 나타납니다. 모형 격자 lm 파일 크기를 압축할 수 있는 역할을 할 수 있습니다. 항목 중 절선 (법선 플랫폼) 과 정점 색상을 사용하지 않으시면 20% 정도 모형 자원 크기를 절약할 수 있습니다.

Ignore Verticess Tangent 정점 절선 정보 무시
Ignore Vertices Color 정점 색상 정보 무시

![图片4](img/4.png)< br > (그림 4)

####Terrain Setting

unity G형 내보내기 설정 (그림 5)

Conver Terrain To Mersh
만약 장면에 지형이 있다면, 변환지 모양으로 모형.
untiy 의 지형 제작은 매우 편리하며 지형 높이, 산천, 하천 등 다양한 디테일 스티커를 그릴 수 있으며, 여러 가지 스티커에 사용되는 지표로 제작할 수 있다.Layair 내보내기 플러그인을 Mersh로 바꾸어 개발자 사용을 편리하게 합니다.소재와 기본 소재와 달리 디테일 스티커가 포함되어 있습니다.

Resolution
내보내는 모형 격자면 최적화 설정은 일반적으로 기본 Medium 중등이다.이하 설정의 최적화 등급은 1단계당 4의 면수를 제외하는 정교량에 해당한다.
Very Height 최적화 후 면수가 가장 높습니다.
Hight 최적화 후 면 수가 상대적으로 높다
미디엄 최적화 후 면 중
Low 최적화 후 면 수가 낮다
Very Low 최적화 후 면 최저

![图片7](img/7.png)< br > (그림 5)



####GameObject Setting

게임 노드 설정 (그림 6)

Ignore Null Game Objects
내보내는 동안 노드를 무시하고, Layair 엔진이 지원되지 않는 노드를 적어, 불빛의 노드를 누르면 요정수를 줄일 수 있다.
주: 1.5.0판은 카메라를 지원해 내보내기 때문에 빈 노드를 무시하면 카메라에 영향을 주지 않는다.

Ignore Not Active Game Objects
유닛 장면에서 활성화되지 않은 노드를 내보내는 데 무시합니다.

Optimize Game Objects
내보내는 동안 유닛 장면에서 1급 노드를 찍기 시작하여 평수형 구조를 삭제하고 모든 노드 를 삭제하면 요정수를 최대한 줄일 수 있다.

Batch Make The First Level Game Objects
대량 내보내기 (sprite3d 를 선택해야) 대량 내보내기 (모든 노드) 를 선택해야 합니다.



 ![图片8](img/8.png)< br > (그림 6)



####Other Setting

다른 설정 (7)

Cover Original Export Files
내보내는 중

Customize Export Root Directory Name
폴더 이름 내보내기 위해 기본값의 폴더는 'layaSce + 장면' 입니다.

Automatically Save The Configuration
현재 설정을 자동으로 내보내기



 ![图片9](img/9.png)< br > (7)



####내보내기 설정

borower 저장 파일 경로
Clear Config 현재 설정 비우기
Revert Config 프로필 목록에서 읽기 설정
Save Config 이 현재 설정을 저장하고 저장한 후 다음 열면 바로 사용하기 전에 설정하고 개발자들을 운영하기 편리합니다.
Layaiair Run 은 Layair 엔진을 사용하여 직접 이 장면을 실행할 수 있습니다.
LayairRun 사용 안내 사항:
1. Node 환경을 설치해야 합니다. express 모듈 (도구 안에 express, 제대로 사용하지 못하면 설치하십시오);
2. 장면에서 카메라가 하나 있는데, 자체 조정, 각도, 최종적으로 레이아라 실행 효과는 유닛 운행 결과와 일치한다.
Layaiair Export 현재 자원을 내보내며, 클릭을 누르면 현재 장면이나 모형 데이터를 지정 경로로 내보낼 것입니다.



 ![图片10](img/10.png)<br>（图8）











###내보내는 자원 단순 소개

출력 장면을 설정한 후, Laya Export 단추를 누르고, 내보내는 기본 LayaSceu truck 폴더 (그림 10) 를 생성했습니다.



 ![图片11](img/11.png)<br>（图9）


그림 파일 자원을 보고, 내보내면 ls, lm, lmat 데이터 자원, 스티커 png, tga 자원이 생성된다.

Scene 분류를 내보내기 위한 다양한 데이터와 모형, 조명 스티커, 위치 등을 포함하고 있으며, 수요 Scene 종류를 사용합니다.

lh 는 모형 파일로 Sprite3D 분류를 내보낼 때 생성되고, 사진 파일 정보가 부족하고, 다른 ls 와 동일한다.

lm 은 모형 데이터 파일로 FBX 형식의 변환으로 메시Sprite3D 종류를 사용할 수 있습니다.

lmat 는 소재 데이터 파일로 유닛에서 모형 설정을 위한 소재 정보로 ls 나 lh 파일을 가재할 때 자동으로 lmat에 재질이 발생할 수 있습니다.일부 속성을 수동적으로 수정할 수 있습니다.

애니메이션 데이터 파일 (그림 9중 모형이 애니메이션이 없기 때문에 내보내지 않았기 때문에 모형에 애니메이션 을 내보내면 애니메이션 설정 파일을 생성시켜 뼈나 프레임 정보를 포함한다.

그것들의 구체적인 용법은 후속 과정 문서에서 상세하게 소개할 것이다.



###단순 가재 실례

LayaSceneu truck 폴더 내용을 항목의 루트 디렉터리에 복사한 bin/ h5 / 아래.

Tips: 이 장절에는 간단한 적용만 소개하고 내보내면 다양한 형식을 생성할 수 있습니다. 이 내용은 3D 기술 문서에서 'Layair3D 장면 Scene'과' 레이이아르 3D 모형 '소개합니다.

씬을 가재하다.


```typescript


class LayaAir3D
{
      constructor() 
      {
        //初始化引擎
        Laya3D.init(0, 0,true);
        Laya.stage.scaleMode = Laya.Stage.SCALE_FULL;
        Laya.stage.screenMode = Laya.Stage.SCREEN_NONE;
        Laya.Stat.show();

        //添加3D场景
     	Laya.Scene3D.load("LayaScene_truck/truck.ls",Laya.Handler.create(this,function(s:Laya.Scene3D):void{
           	var scene = s;
        	Laya.stage.addChild(scene);
          	//创建摄像机(横纵比，近距裁剪，远距裁剪)
            var camera= new Laya.Camera( 0, 0.1, 1000);
            //加载到场景
            scene.addChild(camera);
            //移动摄像机位置
            camera.transform.position=new Laya.Vector3(-8, 4, 15);
            //旋转摄像机角度
            camera.transform.rotate(new Laya.Vector3( -8, -25, 0), true, false);
     	}));
	}		
}
new LayaAir3D();
```


이 같은 간단한 코드를 번역할 때, 우리는 장면의 가재에 성공한 장면을 발견하고, 장면의 모형도 3D 보기 (그림 10) 에 나타났습니다.



 ![图片12](img/12.png)<br>（图10）



