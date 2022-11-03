# Hi6 SDK 설명서

{% hint style="warning" %}
본 제품 설명서에서 제공되는 정보는 현대로보틱스의 자산입니다.

현대로보틱스의 서면에 의한 동의 없이 전부 또는 일부를 무단 전재 및 재배포할 수 없으며, 제3자에게 제공되거나 다른 목적에 사용할 수 없습니다.



본 설명서는 사전 예고 없이 변경될 수 있습니다.



**Copyright ⓒ 2020 by Hyundai Robotics**
{% endhint %}
# 1. Hi6 SDK 개요

이 설명서는 Hi6 제어기의 추가 기능 개발을 위한 플러그인 앱(plugin-app) 개발용 SDK (Software Development Kit) 사용법을 설명합니다.

SDK로 개발할 수 있는 앱의 기능은 아래와 같습니다.



로봇언어에 새로운 명령문이나 객체형을 추가할 수 있습니다.
전원On, 모터 on/off, 기동/정지 등 각종 이벤트에 수행할 동작을 추가할 수 있습니다.
주기적으로 수행할 동작을 추가할 수 있습니다.
티치펜던트를 위한 설정화면 등 사용자 정의 UI를 추가할 수 있습니다.# 1.1 필요한 사전 지식

SDK를 활용한 앱(app) 개발을 하기 위해서는 아래의 기술들에 대해 기본 이상의 숙련도를 갖추고 있어야 합니다.

만일 아래 기술들이 익숙하지 않다면, 먼저 적절한 교재를 이용해 학습하기를 권장합니다.

<table>
  <thead>
    <tr>
      <th style="text-align:left">필요한 기술</th>
      <th style="text-align:left">용도</th>
      <th style="text-align:left">교재</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Hi6 제어기의 기본 사용법</td>
      <td>
       로봇 조작의 기본 지식
      </td>
      <td>Hi6 제어기 조작설명서</td>
    </tr>
   <tr>
      <td>HRScript 로봇언어 프로그래밍</td>
      <td>
       HRScript와 app과의 연동
      </td>
      <td>Hi6 제어기 기능설명서 - HRScript</td>
    </tr>
    <tr>
      <td>python3 프로그래밍</td>
      <td>
       app 동작의 구현
      </td>
      <td>python 학습서, 혹은 온/오프라인 교육 프로그램</td>
    </tr>
    <tr>
      <td>웹 앱 프로그래밍</br>
      (HTML5/CSS/JavaScript, jQuery)</td>
      <td>
       app U/I 화면의 구현	
      </td>
      <td>웹 개발 학습서, 혹은 온/오프라인 교육 프로그램</br>
      (U/I가 없는 app 개발 시에는 필요없음.)</td>
    </tr>
    <tr>
      <td>이더넷 UDP 통신의 기본 개념</td>
      <td>
       argosx 예제의 이해
      </td>
      <td>python 학습서의 이더넷 socket 통신 챕터</td>
    </tr>

  </tbody>
</table>


		
		
		







		
# 1.2 Hi6 플러그인 앱(plugin-app)의 개념
Hi6 앱은 main module 내에서 동작하는 python 3 스크립트와, 티치펜던트 내에서 U/I 동작을 수행하는 javascript 기반 웹 소프트웨어로 구성됩니다.

만일 U/I 가 없는 앱이라면 python 스크립트만으로 구성될 수도 있습니다.

여러 개의 python 파일들과 웹 앱(webapp) 파일들은 main module 내의 하나의 폴더 밑에 설치됩니다.

## python 스크립트
로봇 제어기의 특정한 이벤트에 의해 혹은 주기적으로 호출될 수도 있습니다. 또한 지정한 python 함수가 .job 파일 내의 로봇언어 명령문으로서 호출될 수도 있습니다.

xhost라는 모듈을 통해 로봇 제어기의 소프트웨어 객체들(Objects)을 제어하거나 모니터링할 수 있습니다. xhost는 python interface 혹은 OpenAPI를 통해 소프트웨어 객체들과 상호작용합니다.

## 티치펜던트 U/I 웹 앱
PC나 모바일 환경에서 사용되는 통상적인 웹 앱과 동일합니다. HTML/CSS/JavaScript 및 각종 이미지 등 리소스 파일들로 구성됩니다.

main module 내에 저장되어 있다가 티치펜던트로 전송되어 웹 브라우저 엔진 상에서 실행됩니다.

티치펜던트 웹 앱은 OpenAPI 호출을 통해서 python 함수를 호출하고 데이터를 주고 받을 수 있습니다.

![](../_assets/image_1.png)# 1.3 Hi6 가상제어기 설치

(임시)</br>
1) 다운받은 Hi6 controller zip을 해제한다.
2) 해제한 폴더 속의 '사용 방법.txt'대로 수행하여 개발환경을 맞추어 준다.
3) Debug 폴더 안에 hi6_main.exe를 실행시킨다.
4) 같은 위치에서 포
명령인수 -layout=k를 입력한다.# 1.4 python3 개발환경 설치
## python3 설치
아래의 절차에 따라 python v3.8을 설치합니다.



1) 아래 링크는 python v3.8.0 설치 화면으로 연결됩니다. x86 32bit를 설치하십시오.

    (주의! : Hi6 가상제어기는 32bit 애플리케이션이므로 python 런타임도 이와 일치시켜야 합니다. x86-64를 설치하면 안됩니다 !) </br>
    https://www.python.org/downloads/release/python-380/

    ![](../_assets/image_2.png)

2) Add Python 3.8 to PATH 체크 후, Curtomize installation 선택합니다.
    ![](../_assets/image_3.png)

3) 모두 check 하고 Next 클릭합니다.
    ![](../_assets/image_4.png)
4) 모두 check. 설치 경로는 주어진 대로, C:\Program Files (x86)\Python38-32 로 두고 Install 클릭합니다.
    ![](../_assets/image_5.png)
5) Disable path length limit는 안 눌러도 됩니다. Close 클릭합니다.
    ![](../_assets/image_6.png)

6) 윈도우 명령 프롬프트를 엽니다. (Windows + R 누른 후 cmd 타이핑하고, enter키)

python --version을 타이핑한 후 enter 키를 눌러 아래와 같은 버전이 출력되는지 확인합니다.

Python 3.8.0

## python import 검색 경로 추가
1) .pth 라는 파일을 만들고 안에 _common/ 폴더가 위치한 경로를 지정해준다. </br>SDK 내의 .pth 파일을 열어, Hi6 가상제어기의 HOME 경로에 맞게 아래 경로를 지정해줍니다.
 

파일내용 예:    
    D:\Hi6\home_main\apps

편집한 .pth 파일을 python 설치 경로/Lib/site-packages/ 에 배치합니다.

예: C:\Program Files (x86)\Python38-32\Lib\site-packages\.pth


## 동적 라이브러리 배치
SDK 내의 ucrtbased.dll와 vcruntime140d.dll를 python 설치 경로에 배치합니다.

예: C:\Program Files (x86)\Python38-32\ 에 복사.# 1.5 VisualStudio Code의 설치

Microsot Visual Studio Code (이하 vscode로 지칭)는 무료로 사용가능한 강력한 텍스트 편집기입니다. 다양한 EXTENSION 설치를 통해, 수 많은 프로그래밍 언어의 개발환경을 제공합니다.

Atom이나 SublimeText 등 본인이 익숙한 다른 편집기를 사용해도 되지만, 이 설명서에서는 vscode를 기준으로 설명하겠습니다.



Code의 설치
1) 아래 링크에 접속한 후 윈도우용 stable build 버전을 다운로드 합니다.</br>윈도우용  https://code.visualstudio.com/</br>
![](../_assets/image_7.png)

2) 다운로드된 설치파일을 실행하고 라이선스에 동의합니다.
![](../_assets/image_8.png)

3) 설치 경로 등을 기본값 그대로 하여 <다음> 버튼을 계속 누르다가, 추가 작업 선택 화면이 나오면 아래와 같이 체크하고 <다음> 버튼을 클릭합니다.
![](../_assets/image_9.png)

4) <설치> 버튼을 클릭합니다.

5) 설치가 끝나면 vscode를 실행해 봅니다.

# EXTENSION의 설치
마켓플레이스에 있는 다양한 확장(EXTENSION)을 설치할 수 있다는 것이 vscode의 강력한 장점입니다.

vscode 화면에서 가장 왼쪽에 아이콘들이 배치된 수직막대가 Activity Bar입니다. 이 중  빨간 표시된 아이콘을 누르면 그림과 같이 EXTENSIONS: MARKETPLACE가 열립니다. 상단의 필터 창에 이름을 타이핑하면 원하는 확장을 쉽게 찾을 수 있습니다.

![](../_assets/image_10.png)

설치해야 할 확장은 아래와 같습니다. 하나씩 필터를 통해 찾아서 Install 버튼을 클릭해 설치합니다. 



같은 이름의 확장이 여러 개 있을 수도 있습니다. 저자의 이름을 확인하여 올바른 확장을 선택하십시오.
Microsoft의 Python을 설치하면 Pylance 등 부가적인 확장들이 자동으로 추가 설치됩니다. Pylance는 우리가 사용해야 할 Pyright와 충돌합니다. Python을 제외한 부가 확장들은 모두 삭제하십시오.
job 파일 편집 시, HR-BASIC이 아닌 HRScript로 인식되어야 합니다. hrbasic은 Disabled 시켜 두십시오.

![](../_assets/image_11.png)

개발과정에서 python 외에 html, css, javascript, json 확장자의 파일을 다루게 되지만, 이 형식들은 편집기능이 vscode에 기본 탑재되어 있으므로 별도의 확장 설치는 필요하지 않습니다.



이제 vscode를 사용할 준비가 끝났습니다. 상세한 사용방법은 vscode의 Help 메뉴 혹은 인터넷 강좌를 참고하여 습득하시기 바랍니다.

![](../_assets/image_12.png)
# 1.6 웹기반 U/I 개발환경 설치
티치펜던트를 위한 커스터마이즈된 U/I는 HTML5/CSS/jQuery의 웹앱 형태로 개발합니다.



## Google Chrome 웹브라우저 설치

Google Chrome 웹 브라우저는 웹 앱의 구동 및 디버깅 환경을 제공합니다. 아직 설치되어 있지 않다면 아래 링크를 클릭하여 설치하십시오.

(Microsoft Edge나 Mozilla Firefox 웹 브라우저도 거의 동일한 기능을 제공합니다. 하지만 이 설명서에서는 Chrome을 기준으로 설명하겠습니다.)

https://www.google.com/intl/ko/chrome/

# 1.7 가상제어기 환경에 SDK 설치

SDK의 apps/ 폴더를 가상제어기의 hi6 home 폴더 밑에 복사하십시오.

apps/ 폴더 밑의 _common/ 폴더는 모든 app들이 공용으로 사용하는 라이브러리를 담고 있습니다.

앞으로 개발하게 될 app들도 이와 같이 apps/의 서브 폴더들로서 배치되어야 합니다.

# 2. 초간단 프로젝트 : hello_world
Skip to end of metadata
Created by 최원혁, last modified on 2021년 12월 24일Go to start of metadata
아주 간단한 프로젝트로 app 제작을 시작해봅시다.

hello_world라는 이름의 이 app이 하는 일은, 히스토리 화면과 설정화면에 Hello, world! 라는 문자열을 출력하는 것 뿐입니다.



hello_world 프로젝트 생성 - 폴더와 메타정보
python 함수 hello( ) 구현
python 함수에 매개변수 전달하고 리턴값 전달받기
간단한 웹 기반 U/I 화면 만들기
# 2.1 hello_world 프로젝트 생성 - 폴더와 메타정보

apps/ 폴더 밑에 hello_world 라는 이름의 폴더를 생성합니다. 폴더의 이름이 곧 프로젝트명이며, apps/ 폴더에서 유일한(unique) 이름이어야 합니다.

탐색기의 hello_world/ 폴더에 마우스 우버튼을 클릭한 후, pop-up 메뉴에서 "Code(으)로 열기"를 클릭하십시오.

![](../_assets/image_13.png)

hello_world/ 폴더를 프로젝트로 하여 vscode가 열립니다. 좌상단의 New File 버튼을 클릭하여 폴더에 새 파일을 만들 수 있습니다. 파일명은 info.json으로 합니다.

![](../_assets/image_14.png)

info.json을 클릭해 열고 아래와 같이 입력합니다.

![](../_assets/image_15.png)

## info.json

``` json
{
   "author" : "Hyundai Robotics",
   "binding" : "plug-in",
   "copyright" : "All right reserved",
   "description" : "First example - hello_world",
   "entry" : "hello_world.py",
   "menu" : "ui/menu.json",
   "startup" : "manual",
   "version" : "v0.9.0"
}
```

</br>
<table>
  <thead>
    <tr>
      <th style="text-align:left">key</th>
      <th style="text-align:left">의미</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>author</td>
      <td>
       저자
      </td>
    </tr>
   <tr>
      <td>binding</td>
      <td>
       Hi6 호스트 소프트웨어와의 결합 형태</br>
       - plug-in: 결한된 형태로 실행됨.</br>
       - stand-alone: 독립된 응용 프로그램(process)으로 실행됨. 
      </td>
    </tr>
    <tr>
      <td>copyright</td>
      <td>
       저작권
      </td>
    </tr>
    <tr>
      <td>description</td>
      <td>
       설명	
      </td>
    </tr>
    <tr>
      <td>entry</td>
      <td>
       실행을 시작할 시작 위치의 파일명</br>
       apps/ 폴더에서 유일한(unique) 이름이어야 하므로, 가급적 아래의 이름들 중 하나로 정함</br>
       - {프로젝트명}.py</br>
       - {프로젝트명}_main.py
      </td>
    </tr>
    <tr>
      <td>menu</td>
      <td>
       사용자 인터페이스의 메뉴 구조	
      </td>
    </tr>
     <tr>
      <td>startup</td>
      <td>
       실행 시작 방식
       - manual: 수동으로 실행 시작
       - boot: 부팅시 자동으로 실행 시작	
      </td>
    </tr>
     <tr>
      <td>version</td>
      <td>
       버전 문자열	
      </td>
    </tr>
  </tbody>
</table># 2.2 python 함수 hello( ) 구현

New File 버튼으로 새로운 파일을 만들고 이름을 hello_world.py로 정합니다.
![](../_assets/image_16.png)

hello_world.py는 아래와 같이 작성합니다.

```python
import xhost
 
def hello():
    print("Hello, world!")
    xhost.printh("Hello, world!")
```

- python 프로그래밍을 익혔다면 이미 알고 있겠지만, def 아래 행들은 tab 문자로 들여쓰기 해야 합니다.
- xhost는 호스트(로봇제어기)의 기능을 호출하기 위한 모듈입니다. xhost.py라는 파일은 직접 작성할 필요가 없습니다. 후속 절에서 자세히 설명되므로 여기서는 이 정도만 이해하면 됩니다.

이제 Hi6 가상제어기 main과 TP를 차례로 실행합니다.

Hi6 제어기 main은 시작하면서 apps/ 폴더 밑의 모든 폴더의 info.json를 읽어들임으로써, 설치된 app들을 인식합니다.

[서비스] - 10: 앱(App)을 클릭하면 10: 앱(App) - TP라는 제목의 화면이 나타납니다. [location] 버튼을 2번 클릭하면 제목의 TP가 USB를 거쳐 MAIN으로 바뀝니다. 이 화면에서 우리가 만든 hello_world를 찾을 수 있습니다.

![](../_assets/image_17.png)

우리는 hello_world를 로봇언어에서 실행할 것이므로, ESC 키를 눌러 화면에서 빠져나갑니다.

이제 HRScript로 job 프로그램을 하나 생성합니다. 아래와 같이 교시하십시오.

```
import hello_world
hello_world.hello()
end
```

지난화면 pane을 열어놓고 모터ON을 하고, Step FWD 혹은 START 버튼으로 실행하면, 지난화면 창에 Hello, World! 가 출력됩니다.

```
15:05:20.894 ( 968) .import hello_world

15:05:20.894 ( 0)_____[STOP for CmdMode]

15:05:25.413 (4518890)_____[START:StepFwd]__(P2/S0./F1)___

15:05:25.415 Hello, world!

15:05:25.416 ( 2968) .var ret=hello_world.hello()

15:05:25.417 ( 1024)_____[STOP for CmdMode]

15:05:32.157 (6740100)_____[START:StepFwd]__(P2/S0./F2)___

15:05:32.158 ( 970) .end
```

<span style='background-color:#ffdce0'>
주의: python 프로그램을 수정하면 가상제어기 main을 다시 실행해야 수정한 내용이 반영됩니다. 
</span># 2.3 python 함수에 매개변수 전달하고 리턴값 전달받기


이제 python 함수에 name과 age라는 2개의 매개변수를 적용해봅시다. 아래와 같이 코드를 수정하여 introduce( ) 함수를 추가합니다.

```python
import xhost
 
def hello():
    print("Hello, world!")
    xhost.printh("Hello, world!")
 
 
def introduce(name: str, age: int) -> str:
    msg = f"Hello, {name}! You are {age} years old."
    return msg
```

- introduce 함수의 :str, :int, -> str 은 python 3.5부터 도입된 type hint라는 문법입니다. type hint는 생략해도 동작에는 지장이 없습니다. 스크립트 실행 시에는 아무 역할을 안 하지만, Pyright 같은 vscode extension이 문법검사를 할 수 있도록 정보를 주기 때문에, python 코딩 시 문법 에러를 범하지 않도록 도움을 줍니다. (https://docs.python.org/3.8/library/typing.html)


job 프로그램에는 introduce( )의 호출을 추가로 교시합니다.

```
import hello_world
hello_world.hello()
var msg=hello_world.introduce("Steve", 25)
print msg
end
```

티치펜던트의 안내프레임에 아래와 같은 문자열이 출력되면 정상 동작한 것입니다.
![](../_assets/image_18.png)

예제에서 본 바와 같이 HRScript의 문자열 값, 정수 값은 python의 문자열 값, 정수 값으로 자연스럽게 전달되며, 반대로 python의 문자열 리턴값도 HRScript의 문자열 값으로 자연스럽게 리턴됩니다. 두 언어의 값은 이와 같이 자동으로 상호 변환됩니다.  아래 표는 두 언어간 data type 맵입니다.

<table>
  <thead>
    <tr>
      <th style="text-align:left">HRScript</th>
      <th style="text-align:left">python</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>bool</td>
      <td>
       bool
      </td>
    </tr>
   <tr>
      <td>숫자 - 정수(intrger)</td>
      <td>
       long 
      </td>
    </tr>
    <tr>
      <td>숫자 - 실수(real)</td>
      <td>
       float
      </td>
    </tr>
    <tr>
      <td>문자열</td>
      <td>
       str
      </td>
    </tr>
    <tr>
      <td>배열</td>
      <td>tuple</td>
    </tr>
    <tr>
      <td>객체</td>
      <td>
       dictionary	
      </td>
    </tr>
     <tr>
      <td>xpy-object</td>
      <td>object</td>
    </tr>
  </tbody>
</table>

-  python에서 생성한 객체를 HRScript가 전달받은 경우, 이를 특별히 xpy-object라고 지칭합니다. xpy-객체에 대해서는 속성값을 읽거나 method를 호출할 수 있는데, 뒤에서 자세히 알아보도록 하겠습니다.
- python 문법은 함수에서 복수 개의 리턴값을 외부로 전달할 수 있으나, HRScript로 전달받을 수는 없습니다.
# 2.4 간단한 웹 기반 U/I 화면 만들기

app에 대한 설정을 티치펜던트로 하기 위해, 웹 기반의 U/I를 추가할 수 있습니다.

본 예제에서는 티치펜던트 화면에 Hello, world!를 출력하는 간단한 기능을 만들어 보겠습니다.



좌상단의 New Folder 버튼을 클릭하여 새 폴더를 만듭니다. 폴더명은 ui로 설정합니다.
![](../_assets/image_19.png)

ui/ 폴더에 새 파일을 만든 후, 이름은 setup.html로 설정합니다. 이제 아래 그림과 같은 상황이 됩니다.

![](../_assets/image_20.png)

setup.html에 아래 코드를 작성합니다.
```
<!DOCTYPE html:5>
<html>
 
<head>
    <title>hello, world - setup</title>
    <meta http-equiv=Content-Type content='text/html; charset=utf-8'>
</head>
 
<body>
   <div>
      <div id='contents'>
         <h1>Hello, world!</h1>
      </div>
   </div>
</body>
</html>
```

이제, 이 화면을 열기 위한 메뉴항목을 티치펜던트의 시스템 - 응용파라미터 메뉴 밑에 주입해 봅시다.

ui/ 폴더 밑에 menu.json 파일을 생성합니다.
![](../_assets/image_21.png)

``` json
[
    {
        "path": "system/appl/",
        "id": "hello_world",
        "icon": "hello_world/ui/lm_hello.png",
        "label": "hello, world",
        "url": "hello_world/ui/setup.html"
    }
]
```

각 항목의 의미는 아래와 같습니다.
<table>
  <thead>
    <tr>
      <th style="text-align:left">key</th>
      <th style="text-align:left">의미</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>path</td>
      <td>
       메뉴 항목을 주입할 메뉴 경로</br>(system/appl/는 시스템/4: 응용 파라미터"를 의미합니다.
      </td>
    </tr>
   <tr>
      <td>id</td>
      <td>
       메뉴 항목의 id 
      </td>
    </tr>
    <tr>
      <td>icon</td>
      <td>
       메뉴 화면에 표시할 항목 icon의 (apps/ 폴더 기준의) 상대경로파일명
      </td>
    </tr>
    <tr>
      <td>label</td>
      <td>
       메뉴 화면에 표시할 항목 이름
      </td>
    </tr>
    <tr>
      <td>url</td>
      <td>메뉴 항목 선택 시, 표시할 html 화면의 (apps/ 폴더 기준의) 상대경로파일명</td>
    </tr>
  </tbody>
</table>

icon 지정은 안하더라도 동작은 합니다. 하지만 icon까지 한번 만들어서 실습해봅시다.

icon의 형식은 104x104 pixel의 투명성(transparency) 정보를 포함한 png 파일이어야 합니다.

![](../_assets/lm_hello.png) lm_hello.png의 예 (이 그림을 다운받아 사용해도 됩니다.)

</br>
윈도우OS의 그림판(Paint)으로는 투명 배경의 png 파일을 만들 수 없습니다. 아래 몇 가지 소프트웨어들을 추천합니다.

참고로, 예제의 그림은 COOLTEXT로 1분만에 생성했습니다.

Adobe Illustrator (https://www.adobe.com/kr/products/illustrator.html) : 일러스트 소프트웨어. (상용)

GIMP (http://gimp.org) : 포토샵 수준의 이미지 편집 소프트웨어. (무료)

Medibang Paint Pro (https://medibangpaint.com/pc/) : 쉬운 사용법의 그래픽 툴. (무료)

COOLTEXT (https://cooltext.com/) : text를 로고 그림파일로 생성해주는 웹 사이트. (무료)


이제 가상 메인보드과 가상 티치펜던트를 재실행 합니다.


시스템 - 응용 파라미터 메뉴로 진입하면 아래와 같이 새로 추가된 hello, world 메뉴 항목이 보입니다.
![](../_assets/image_22.png)

메뉴를 누르면 아래와 같이 setup.html 의 화면이 티치펜던트에 나타납니다.

(처음에는 1~2초 정도의 로딩 시간이 소요될 수 있습니다. 2번째 부터는 캐시에 의해 좀 더 빠르게 로딩됩니다.)

![](../_assets/image_23.png)
# 3. 실전 프로젝트 : ArgosX# 3.1 실전 프로젝트 : ArgosX - 로봇언어


이제 좀 더 실제에 가까운 예제 프로젝트를 실습해봅시다.

로봇에 장착하는 ArgosX 라는 이름의 가상의 비전시스템을 위한 플러그인을 개발해보면서, app의 개발방법을 배워보도록 하겠습니다.


- ArgosX와 interface plug-in의 사양</br>
- ArgosX stub</br>
- argosx 프로젝트 생성</br>
- ip_addr, port attribute 생성</br>
- ArgosX의 로봇언어용 함수 생성</br>
- ArgosX의 로봇언어용 함수 구현</br>
- xhost 모듈의 method 호출</br>
- xhost 모듈의 method 참조설명서</br>
- 로봇언어 함수 blocking 문제 해결</br>
# 3.1.1 ArgosX와 interface plug-in의 사양

## ArgosX 비전시스템의 사양


### 기본 사양
<table>
  <thead>
    <tr>
      <th style="text-align:left">항목</th>
      <th style="text-align:left">설명</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>기능</td>
      <td>
       - LED 조명을 자체 내장하고 있고, 통신 요청에 의해 켜고 끌 수 있다.</br>
       - 최대 100개의 작업물의 시프트 값을 동시에 측정하며, 통신 요청에 응답해 보고할 수 있다.
      </td>
    </tr>
   <tr>
      <td>통신 iterface</td>
      <td>
       - 로봇제어기와 ArgosX 하드웨어는 이더넷 UDP 통신으로 서로 교신한다.</br>
        - ArgosX 하드웨어 측 IP주소는 192.168.1.XX이다. 끝자리 XX는 dip switch로 설정하기 때문에, 로봇 측에서 이에 맞게 UDP 요청을 보내야 한다.</br>
        - ArgosX 하드웨어 측 port 번호는 54321으로 고정되어 있다. 하지만, 차기 제품은 변경될 수도 있다.</br>
        - ArgosX 하드웨어는 UDP 요청을 받으면 송신자의 IP주소로 응답해준다.
      </td>
    </tr>
    <tr>
      <td>허용 장착 개수</td>
      <td>
       - 로봇제어기에는 ArgosX를 1개만 장착할 수 있다. 즉 ArgosX는 소프트웨어적으로 단일 instance이다.
      </td>
    </tr>
  </tbody>
</table>

### 프로토콜
<table>
  <thead>
    <tr>
      <th style="text-align:left">송신 방향</th>
      <th style="text-align:left">port#</th>
      <th style="text-align:left">문법과 예</th>
      <th style="text-align:left">의미</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>robot → ArgosX</td>
      <td>54321</td>
      <td>req {작업물#}</br>
        e.g. "req 39"</td>
      <td>#번 작업물의 시프트값 요청</br>작업물#는 1~100</td>
    </tr>
   <tr>
      <td>robot ← ArgosX</td>
      <td></td>
      <td>res ({x}, {y}, {z}, {rx}, {ry}, {rz})</br>
            측정에 실패하면 "fail"이란 문자열 전달</br>
            e.g. "res (30, 25.7, 11.9, 31.6, 12.8, -54.6)"</br>
            e.g. "fail"</td>
      <td>#번 작업물의 시프트값 응답</br>
        x~rz는 실수값. 단위는 mm, deg.</td>
    </tr>
    <tr>
      <td>robot → ArgosX</td>
      <td>54321</td>
      <td>light-on</td>
      <td>LED 조명을 켠다.</td>
    </tr>
    <tr>
      <td>robot → ArgosX</td>
      <td>54321</td>
      <td>light-off</td>
      <td>LED 조명을 끈다.</td>
    </tr>
  </tbody>
</table>

## ArgosX 인터페이스 플러그인의 사양


ArgosX를 인터페이스하기 위한 플러그인은 아래와 같은 사양으로 개발하겠습니다.



### 로봇언어
<table>
  <thead>
    <tr>
      <th style="text-align:left">항목</th>
      <th style="text-align:left">문법</th>
      <th style="text-align:left">설명</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>module</td>
      <td>argosx</td>
      <td></td>
    </tr>
   <tr>
      <td rowspan="2">attribute</td>
      <td>ip_addr</td>
      <td>ArgosX 하드웨어의 IP 주소 문자열 (설정가능하다.)</br>e.g. "192.168.1.44"</td>
    </tr>
    <tr>
      <td>port</td>
      <td>ArgosX 하드웨어의 port 번호.</br>(차기 제품에서 변경될 수도 있으므로 설정 가능해야 한다.)</br>e.g. 54321</td>
    </tr>
    <tr>
      <td rowspan="4">function</td>
      <td>init( )</td>
      <td>UDP통신을 위한 socket을 초기화한다.</td>
    </tr>
    <tr>
      <td>req({작업물#})</td>
      <td>작업물#번의 결과 시프트값 요청,</td>
    </tr>
    <tr>
      <td>res( )</td>
      <td>응답 대기하다가 요청 받기.</br>리턴값은 base 좌표계 시프트 배열 문자열.</br>e.g. "[30, 25.7, 11.9, 31.6, 12.8, -54.6, \"base\"]"</td>
    </tr>
    <tr>
      <td>close( )</td>
      <td>UDP통신을 위한 socket을 닫는다.</td>
    </tr>
  </tbody>
</table>

### 조명 기능
- 로봇이 motor ON 되면, ArgosX의 LED 조명도 같이 켜진다.
- 로봇이 motor OFF 되면, ArgosX의 LED 조명도 같이 꺼진다.

### 에러 처리
- ArgosX로부터 "fail"이 수신되면, 미리 설정해 둔 번호의 로봇제어기 범용 I/O 출력신호를 켠다.


### 모니터링
티치펜던트의 ArgosX용 모니터링 panel을 열어, 아래 정보들을 관측 할 수 있다.

- IP주소
- port번호
- 에러 입력할당 번호
- 요청 횟수
- 응답 횟수


### 사용자 막대 (user-bar)
티치펜던트의 ArgosX용 사용자 막대를 열면, 아래와 같은 U/I가 제공된다.

- light-on 버튼 : ArgosX의 LED 조명을 켠다.
- light-off 버튼 : ArgosX의 LED 조명을 끈다.# 3.1.2 ArgosX stub

ArgosX 비전 시스템은 실제로 존재하는 장치가 아닙니다. 따라서 우리가 인터페이스 플러그인을 시험하려면, ArgosX의 역할을 대신해줄 시험용 소프트웨어, 즉 stub가 필요합니다.

아래 python code는 ArgosX stub입니다. 구현 내용을 이해할 필요는 없습니다.

argosx_stub.py
``` python
"""ArgosX stub
Test double for ArgosX interface plug-in
 
 
@author:    Jane Doe, BlueOcean Robot & Automation, Ltd.
@created:   2021-12-06
@version:   v1.0
"""
 
import time
from typing import Optional, Union, Dict
import socket
 
 
# const
buf_size = 0x8000    # 32kb ; permitted packet length
port_no = 54321      # port for ArgosX command
sleep_sec = 0        # delay before response
 
# global variables
inaddr_any : str = ""
ip_port_of_req = ("", 0)
sock : Optional[socket.socket] = None
 
# test samples
test_shifts : Dict[str, str]= {
   "5" : "(30, 25.7, 11.9, 31.6, 12.8, -54.6)",
   "39" : "(9, 15.5, 10.3, 11.2, 19.2, 1.3)",
   "98" : "fail",
   "else" : "(0, 0, 0, 0, 0, 0)"
}
 
 
# functions
def init():
   """
   init server
   """
   global sock
   try:
      sock = socket.socket(family=socket.AF_INET, type=socket.SOCK_DGRAM)
      sock.bind((inaddr_any, port_no))
   except socket.error as e:
      print("socket creation or binding error :", e)
      return -1
   return 0
 
 
def close():
   """
   close server
   """
   if sock is None: return
   sock.close()
 
 
def do_service():
   """
   do service loop
   """
   state = 0
    
   while(True):
      msg = recv_msg()
      msg = msg.strip('\x00')
      ret = do_service_sub(msg)
      if ret==1:
         break
 
 
def recv_msg() -> str:
   """
   receive UDP message (blocking)
   IP address and port of sender is stored in ip_port_of_req
   Returns:
         received message     e.g. "req 39"
   """
   global ip_port_of_req
   if sock is None: return ""
   data, ip_port_of_req = sock.recvfrom(buf_size)
   bts = bytearray(data)
   msg = bts.decode()
   return msg
 
 
def do_service_sub(msg: str) -> int:
   """
   do service subroutine
   Args:
         msg   e.g. "req 39"
   Returns:
         1     quit the service
         0     continue the service
         -1    invalid command  
   """
   print('')
   print('request : ', msg)
   strs = msg.split()
       
   n_str = len(strs)
   if n_str < 1:
      return -1
    
   cmd = strs[0]
   param = ""
   if n_str >= 2:
      param = strs[1]
    
   if cmd=="req":
      time.sleep(sleep_sec)
      do_service_req(param)
   elif cmd=="light-on":
      print('LED light is ON')
   elif cmd=="light-off":
      print('LED light is OFF')
   elif cmd=="quit":
      return 1
   else:
      print('invalid command')
      return -1
   return 0
 
 
def do_service_req(param: str) -> int:
   """
   do service for req
   Args:
      param    work#    "1"~"100"
 
   Returns:
         -1    no socket
         >=0   the number of bytes sent
   """
   if sock is None: return -1
   res_value = ""
   try:
      res_value = test_shifts[param]
   except:
      res_value = test_shifts["else"]
   msg = "res " + res_value
   print('response: ', msg)
   bts = bytearray(str.encode(msg))
   return sock.sendto(bts, ip_port_of_req)
    
 
# -----------------------------------------------
# main
print('***** ArgosX stub v1.0 *****')
iret = init()
if iret < 0:
   quit()
print('inaddr_any, port_no=%d' % port_no)
print('server started...')
do_service()
print('closing...')
close()
print('...server ended')
```

위 내용을 복사하여 hello_world/ 폴더 아래에 argosx_stub.py 파일을 생성해놓고, 윈도우 PowerShell이나 명령 프롬프트에서  hello_world/ 폴더로 이동한 후 아래 명령으로 실행하십시오.
```
python argosx_stub.py
```

혹은,  vscode에 열어 놓고, F5키를 누르면 디버그 모드로 실행됩니다.

- argosx/ 프로젝트를 열어 둔 vscode에서 열지 말고, vscode를 하나 더 실행하여 여십시오.
- 처음에는 아래와 같이 debug configuration 리스트가 열릴 수도 있는데, Python File 항목을 선택하면 됩니다.

![](../../_assets/image_24.png)

출력은 하단의 TERMINAL 창으로 나옵니다. 우상단의 ![](../_assets/image_25.png) 버튼을 조작해 디버깅을 일시정지, 재실행, 정지할 수 있습니다.

![](../../_assets/image_26.png)# 3.1.3 argosx 프로젝트 생성

apps/ 폴더 밑에 argosx 라는 이름의 폴더를 생성합니다.

탐색기의 argosx/ 폴더에 마우스 우버튼을 클릭한 후, pop-up 메뉴에서 "Code(으)로 열기"를 클릭하십시오.

argosx/ 폴더를 프로젝트로 하여 vscode가 열립니다. argosx/ 밑에 info.json을 아래와 같이 만듭니다.

info.json

```json
{
   "author" : "BlueOcean Robot & Automation, Ltd.",
   "binding" : "plug-in",
   "copyright" : "All right reserved",
   "description" : "ArgosX vision system interface",
   "entry" : "main.py",
   "menu" : "ui/menu.json",
   "startup" : "manual",
   "version" : "v0.9.0"
}
```


우선 argosx/ 폴더 밑에 main.py 파일을 생성합니다. (@author에는 본인 이름을 적어도 됩니다.)

main.py
```python 
""" ArgosX Vision System interface - main
 
 
@author:    Jane Doe, BlueOcean Robot & Automation, Ltd.
@created:   2021-12-06
"""
```

그리고 아래와 같이 이를 시험할 job 파일을 하나 교시합니다.

```
import argosx
end
```
# 3.1.4 ip_addr, port attribute 생성

ArgosX와 interface plug-in의 사양을 보면 우선 ip_addr를 지정하는 문자열 속성(attribute)이 있습니다.



argosx_main.py 파일에, 아래와 같이 전역변수와 attr_names( ) 함수를 추가합니다.

python 프로그램 내에 많은 전역변수를 정의해 사용할 수 있지만, 이 중 attr_names( )이 리턴하는 튜플(tuple) 내에 열거한 이름들만 HRScript에 argosx 모듈의 속성(attribute)으로서 노출합니다. 속성으로 노출한 변수는 그 이름에 get_과 set_을 붙인 getter, setter 함수를 정의해야 합니다.

본 예제에서는 ip_attr와 port 두 개의 전역변수를 정의한 후, 이들을 attribute로 노출시켜 HRScript에서 읽기 쓰기가 가능하게 해봅시다.



argosx/ 프로젝트 폴더에 setup.py 파일을 새로 만들고 다음과 같이 구현합니다.



setup.py
``` python 
""" robot application - argosx - setup
 
 
@author:    Jane Doe, BlueOcean Robot & Automation, Ltd.
@created:   2021-12-06
"""
 
 
 
# attributes getter/setter
def get_ip_addr() -> str:
   return ip_addr
 
def set_ip_addr(addr: str):
   global ip_addr
   ip_addr = addr
 
def get_port() -> int:
   return port
 
def set_port(_port: int):
   global port
   port = _port
 
ip_addr : str = "192.168.1.100"
port : int = 54321
```

getter와 setter 함수들을 HRScript에서 호출할 수 있도록 setup module 전체를 main.py에 import 해주고, attrubite 이름들의 tuple을 리턴하는 attr_name( ) 함수를 정의해줍니다.

(host는 argosx/ 폴더를 package로 다룹니다. main module에서 setup module을 import할 때 setup 앞에 같은 폴더를 뜻하는 . (period)를 명시적으로 붙여줘야 합니다.)



main.py
```python 
""" ArgosX Vision System interface - main
 
 
@author:    Jane Doe, BlueOcean Robot & Automation, Ltd.
@created:   2021-12-06
"""
 
from .setup import *
 
 
def attr_names() -> tuple:
   """Returns the names of the attributes to be exposed."""
   return ("ip_addr", "port")
```

시험용 job 프로그램은 아래와 같이 교시한 후 실행합니다.
```
import argosx
print argosx.ip_addr
print argosx.port
end
```

아래와 같이 python 전역변수들의 값이 안내 프레임에 차례로 표시되면 정상 동작한 것입니다.

```
192.168.1.100

54321
```


argosx와 argosx_stub 간의 통신시험은 동일한 PC 내에서 수행하려고 합니다. 따라서, ip_addr 의 값을 본인의 PC의 IP주소로 변경한 후, 잘 변경됐는지 확인해봅시다.

job 프로그램은 아래와 같이 변경한 후 실행합니다.

```
import argosx
print argosx.ip_addr
print argosx.port
argosx.ip_addr="192.168.1.172" # 본인 PC의 IP주소
print argosx.ip_addr # 재확인
end
```

안내프레임에 새로 ip_addr에 대입된 값이 잘 출력되는지 확인하십시오.
```
192.168.1.172
```
# 3.1.5 ArgosX의 로봇언어용 함수 생성


다음 구현할 사양은 init( ), req( ), res( ), close( ) 함수입니다.

(ArgosX와 interface plug-in의 사양의 프로토콜과 로봇언어 function 부분을 참고하십시오.)



일단 각 함수는 간단한 print 동작만 수행하도록 해봅시다.

argosx/ 폴더 밑에 roblang.py 라는 파일을 아래와 같이 생성합니다.



roblang.py (시험용)
``` python 
""" ArgosX Vision System interface - robot language
 
 
@author:    Jane Doe, BlueOcean Robot & Automation, Ltd.
@created:   2021-12-06
"""
 
 
# functions
def init() -> int:
   print("init()")
   return 0
 
 
def close():
   print("close()")
 
 
def req(work_no: int) -> int:
   msg = "req(" + str(work_no) + ")"
   print(msg)
   return 0
 
 
def res():
   print("res()")
   return "data"
```

roblang.py의 모든 명칭을 main.py 안에 import 시킵니다.



main.py
```python

""" ArgosX Vision System interface - main
 
 
@author:    Jane Doe, BlueOcean Robot & Automation, Ltd.
@created:   2021-12-06
"""
 
 
from .roblang import *
from .setup import *
 
 
def attr_names() -> tuple:
   """Returns the names of the attributes to be exposed."""
   return ("ip_addr", "port")

```


job 파일
```
var iret
import argosx
 
print argosx.ip_addr
print argosx.port
argosx.ip_addr="192.168.1.172" # 본인의 PC명
print argosx.ip_addr # 재확인
 
iret=argosx.init() # socket 초기화
if iret<0
  print "init error"
  stop
endif
 
iret=argosx.req(39) # 요청 송신
if iret<0
  print "req error"
  stop
endif
 
var str=argosx.res() # 응답 대기
print str
 
argosx.close() # socket 닫기
end
```

가상제어기를 재부팅한 후, job 파일을 실행해봅니다. 정상적으로 만들어졌다면 가상제어기 콘솔에는 아래와 같이 출력될 것입니다. HRScript에서 python 함수가 호출된 것입니다.
```
192.168.1.172

init()

req(39)

res()

data

close()
```
# 3.1.6 ArgosX의 로봇언어용 함수 구현

이제 각 함수의 실제 동작을 구현해봅시다.

UDP 클라이언트 통신을 수행하는 동작은 추후 다른 부분에서도 사용될 것이므로 별도의 .py 파일로 모듈화하도록 하겠습니다.

프로젝트에 comm.py라는 파일을 추가하고 아래와 같은 동작을 작성합니다.

UDP socket을 초기화하고, 문자열 메시지를 송신하고, 수신하고, 닫는 간단한 동작들입니다.

xhost는 호스트(로봇제어기)의 기능을 호출하기 위한 모듈로서, main s/w가 동적으로 만들어 줍니다. (즉, xhost.py라는 파일은 존재하지 않습니다.) 후속 절에서 자세히 설명되므로 여기서는 이 정도만 이해하면 됩니다.


comm.py
``` python 
""" ArgosX Vision System interface - comm.
 
 
@author:    Jane Doe, BlueOcean Robot & Automation, Ltd.
@created:   2021-12-10
"""
 
from typing import Optional
import socket
import xhost
 
 
# global variables
raddr : tuple
sock : Optional[socket.socket] = None
buf_size = 0x8000    # 32kb ; permitted packet length
 
 
def is_open() -> bool:
   """
   Returns:
         True     socket is open
         False    socket is not open
   """
   return (sock is not None)
 
 
def open(ip_addr: str, port: int) -> int:
   """
   open socket for UDP communication
   Args:
      ip_addr     ip adddress of remote. e.g. "192.168.1.172"
      port        port# of remote. e.g. "192.168.1.172"
 
   Returns:
         0     ok
         -1    error
   """
   global raddr, sock
   if sock is not None: return -1
   try:
      raddr = (ip_addr, port)
      sock = socket.socket(family=socket.AF_INET, type=socket.SOCK_DGRAM)
   except socket.error as e:
      print("socket creation or binding error :", e)
      return -1
   logd('comm.open: ' + str(raddr))
   return 0
 
 
def close() -> None:
   global sock
   if sock is None: return
   logd('comm.close')
   sock.close()
   sock = None
 
 
def send_msg(msg: str) -> int:
   """
   send msg to sock, raddr
   Args:
      msg
 
   Returns:
         >=0   the number of bytes sent
         -1    no socket. init() should be called.
   """
   if sock is None: return -1
 
   logd('request : ' + msg)
   bts = bytearray(str.encode(msg))
   return sock.sendto(bts, raddr)
 
 
def recv_msg():
   """
   wait msg from sock
   Returns:
      received string
   """
   if sock is None: return ""
 
   try:
      data, ip_port = sock.recvfrom(buf_size)
      bts = bytearray(data)
      msg = bts.decode()
      logd('response: ' + msg)
      return msg
   except Exception as e:
      print('exception from recv_msg(): ' + str(e))
      return ""
 
 
def logd(text: str):
   print(text)
   xhost.printh(text)
```

이제 comm 모듈을 import하여 로봇언어에서 호출할 각 함수를 간단하게 구현할 수 있습니다.

ip_addr와 port 값도 참조해야 하므로 setup 모듈도 import해 줍니다.

get_base_shift_array_from_res()는 ArgosX에서 수신된 shift 문자열을 HRScript의 Shift( )함수로 해석되는 형식으로 변환하는 함수입니다.


roblang.py
``` python 
""" ArgosX Vision System interface - robot language
 
 
@author:    Jane Doe, BlueOcean Robot & Automation, Ltd.
@created:   2021-12-06
"""
 
from . import comm
from . import setup
 
 
# functions
def init() -> int:
   """
   init socket for UDP communication
 
   Returns:
         0     ok
         -1    error
   """
   return comm.open(setup.ip_addr, setup.port)
 
 
def close():
   """
   close socket
   """
   comm.close()
 
 
def req(work_no: int) -> int:
   """
   send request command to ArgosX
   e.g. "req 39"
   Args:
      work_no     work#    1~100
 
   Returns:
         >=0   the number of bytes sent
         -1    no socket. init() should be called.
   """
   msg = "req " + str(work_no)
   return comm.send_msg(msg)
 
 
def res() -> str:
   """
   wait response from ArgosX
   Returns:
      response string from ArgosX.
      "" if failed.
      e.g. "[30, 25.7, 11.9, 31.6, 12.8, -54.6]"
   """
   msg = comm.recv_msg()
   print(msg)
   msg = get_base_shift_array_from_res(msg)
   return msg
 
 
def get_base_shift_array_from_res(msg: str):
   """
   get base shift array notation string from response string
   Args:
      msg   e.g. "res (30, 25.7, 11.9, 31.6, 12.8, -54.6)"
    
   Returns:
      e.g. '[30, 25.7, 11.9, 31.6, 12.8, -54.6, "base"]'
   """
   tmp = msg.strip('res ')
   tmp = tmp.replace('(', '[')
   tmp = tmp.replace(')', ', "base"]')
   return tmp
```

job 은 아래와 같이 수정합니다.


```
Hyundai Robot Job File; { version: 1.6, mech_type: "780(YL012-0D)", total_axis: 6, aux_axis: 0 }
     var iret
     import argosx
      
     print argosx.ip_addr
     print argosx.port
     argosx.ip_addr="192.168.1.172" # 본인의 PC명
     print argosx.ip_addr # 재확인
      
     iret=argosx.init() # socket 초기화
     if iret<0
       print "init error"
       stop
     endif
      
     iret=argosx.req(39) # 요청 송신
     if iret<0
       print "req error"
       stop
     endif
      
     var str=argosx.res() # 응답 대기
     print str
     var sft=Shift(str) # shift 배열 문자열을 shift 데이터로 변환
     print sft.x, sft.y, sft.z, sft.rx, sft.ry, sft.rz
 
     argosx.close() # socket 닫기
     end
```

먼저, argosx_stub를 명령 프롬프트나 vscode에서 실행해놓습니다.

가상제어기를 재부팅한 후, job 파일을 실행해봅니다. 정상적으로 만들어졌다면 아래와 같이 동작할 것입니다.



<U>__argosx_stub측 (ArgosX 역할의 server)__ </U>

argosx.req( )가 실행되는 순간마다, 콘솔 출력에 아래와 같은 문자열이 출력됩니다.
```
request : req 39
response: res (9, 15.5, 10.3, 11.2, 19.2, 1.3)
```

<U>__argosx interface plug-in측 (client)__</U>

마지막 print 문이 실행될 때마다 티치펜던트 안내프레임에 아래와 같이 출력됩니다.
```
9.000000 15.500000 10.300000 11.200000, 19.200000 1.300000
```# 3.1.7 xhost 모듈의 method 호출

xhost는 호스트(로봇제어기)의 기능을 호출하기 위한 다양한 method를 포함하는 모듈입니다.

가상제어기 main이 xhost를 생성하여 python 런타임에 주입시켜 주기 때문에,  import xhost만 하면 사용할 수 있습니다. xhost.py라는 파일은 직접 작성할 필요가 없습니다.



<U>xhost 모듈의 method 참조설명서</U>를 참고하십시오.



<<U>ArgosX와 interface plug-in의 사양</U>에서 에러 처리 관련 아래의 항목이 있었습니다.

ArgosX로부터 "fail"이 수신되면, 미리 설정해 둔 번호의 로봇제어기 범용 I/O 출력신호를 켠다.


로봇제어기의 범용 I/O 출력신호는, 아래 method로 on/off 할 수 있습니다.
``` python 
def io_set_out_bit(sigcode: int, val: int) -> int
```

sigcode는 아래와 같이 io의 block 번호와 인덱스를 하나의 숫자로 합친 code입니다.

sigcode = block번호 x 10000 + 인덱스



가령 fb3.do72의 sigcode는 아래와 같습니다.

3 x 10000 + 72 = 30072



val는 ON일 때 1, OFF일 때 0입니다.



ArgosX 에러를 위한 출력신호 할당 번호를 sigcode_err라는 이름의 모듈 변수로 추가하고 default 값은 5 (즉, fb0.do5)로 설정합니다.

(attribute로 선언하여 HRScript에서 변경 가능하게 할 수도 있지만, 이 예제에서는 생략합니다.)



setup.py
```python 
..이전 생략
ip_addr : str = "192.168.1.100"
port : int = 54321
sigcode_err = 5
```

 res( ) 함수 내에서 수신된 msg 값을 "res fail"과 비교하고, 결과에 따라 출력신호를 내보냅니다.



roblang.py
```python
.. 이전 생략
  
  
import xhost
  
  
...생략
 
 
def res() -> str:
   """
   wait response from ArgosX
   Returns:
      response string from ArgosX
      "" if failed.
      e.g. "[30, 25.7, 11.9, 31.6, 12.8, -54.6]"
   """
   val = 0
   msg = comm.recv_msg()
   print(msg)
   if msg=="res fail":
      val = 1
      msg = ""
   else:
      msg = get_base_shift_array_from_res(msg)
   xhost.io_set_out_bit(setup.sigcode_err, val)
   return msg
```

가상제어기를 재 실행하고, 티치펜던트의 범용 출력 panel을 열어놓은 상태에서, job 프로그램을 다시 실행해 봅시다.

fail이 아니므로 이전과 동일한 동작일 것이며, fb0.do5번 출력신호도 켜지지 않을 것입니다.

argosx_stub.py는 98번 work를 요청하면 무조건 fail을 응답하게 만들어져 있습니다. 아래와 같이 req(98)을 수행하도록 job을 수정한 후 다시 실행해봅시다.



job
```
...이전 생략
 
 
     iret=argosx.req(98) # 요청 송신
     if iret<0
       print "req error"
       stop
     endif
      
     var str=argosx.res() # 응답 대기
     print str
     if str==""
       print "req error"
       stop
     else
       var sft=Shift(str) # shift 배열 문자열을 shift 데이터로 변환
       print sft.x, sft.y, sft.z, sft.rx, sft.ry, sft.rz
     endif
 
     argosx.close() # socket 닫기
     end
```

res( )를 수행하는 순간  fb0.do5번 출력신호가 켜진다면, 정상적으로 에러신호가 출력된 것입니다.

![](../../_assets/image_27.png)




5번 신호는 ArgosX의 에러를 위해서만 사용되어야 하므로, 할당 신호로 지정해 두어야 다른 응용에서 사용할 수 없을 것입니다.



아래 xhost의 method로 특정 sigcode를 할당으로 지정해둘 수 있습니다.
```python 
def io_assign_set_out_bit(sigcode: int) -> int
```

main.py에 on_app_init( )라는 함수를 정의한 후, 아래와 같이 할당을 지정하는 루틴을 넣어두면, argosx가 import 되는 순간 실행됩니다.



main.py
```python 
.. 이전 생략
 
 
import xhost
 
 
...생략
 
 
def on_app_init() -> int:
   """(callback) called just after self-diagnosis
   Returns:
      0
   """
   print('[argosx] on_app_init();')
   xhost.io_assign_set_out_bit(setup.sigcode_err)
   return 0
```

가상제어기를 재실행하고 job에서 import argosx가 실행된 후, 범용 출력 panel을 다시 여십시오.

지정한 신호가 할당(bold체)으로 표시됩니다.

![](../../_assets/image_28.png)# 3.1.8 xhost 모듈의 method 참조설명서

<html>
<body>
<h2>get(url, query)</h2>

<h3>Description:</h3>
OpenAPI GET method.<br><br>
<h3>Args:</h3>
<b>url</b>: str. OpenAPI URL.<br>
<b>query</b>: str. OpenAPI query.<br>

<h3>Returns:</h3>
str. responded value.<br>
<br>

<hr>
<h2>put(url, body)</h2>

<h3>Description:</h3>
OpenAPI PUT method.<br><br>
<h3>Args:</h3>
<b>url</b>: str. OpenAPI URL.<br>
<b>body</b>: str. body of the request.<br>

<h3>Returns:</h3>
str. body of the response.<br>
<br>

<hr>
<h2>post(url, body)</h2>

<h3>Description:</h3>
OpenAPI POST method.<br><br>
<h3>Args:</h3>
<b>url</b>: str. OpenAPI URL.<br>
<b>body</b>: str. body of the request.<br>

<h3>Returns:</h3>
str. body of the response.<br>
<br>

<hr>
<h2>hist_print(msg)</h2>

<h3>Description:</h3>
Same as printh() except user-param/hist_print_level setting is applied.<br><br>
<h3>Args:</h3>
<b>msg</b>: str. message.<br>

<h3>Returns:</h3>
	
<br>

<hr>
<h2>printh(msg)</h2>

<h3>Description:</h3>
print to history log.<br><br>
<h3>Args:</h3>
<b>msg</b>: str. message.<br>

<h3>Returns:</h3>
	
<br>

<hr>
<h2>issue_alarm(task_no, type, code)</h2>

<h3>Description:</h3>
issue error or warning event.<br><br>
<h3>Args:</h3>
<b>task_no</b>: int. task number (0~7).<br>
<b>type</b>: <br>
&nbsp&nbsp <b>'E'</b>: error<br>
&nbsp&nbsp <b>'W'</b>: warning<br>
<b>code</b>: alarm code number.<br>

<h3>Returns:</h3>
	
<br>

<hr>
<h2>issue_notice(task_no, code, msg, delay_sec)</h2>

<h3>Description:</h3>
issue notice event.<br><br>
<h3>Args:</h3>
<b>task_no</b>: int. task number (0~7).<br>
<b>code</b>: int. alarm code number.<br>
<b>msg</b>: str. notice message.<br>
<b>delay_sec</b>: float. time to delay before hide (sec).<br>

<h3>Returns:</h3>
	
<br>

<hr>
<h2>set_job_state_msg(task_no, msg)</h2>

<h3>Description:</h3>
set job state message on teach pendant.<br><br>
<h3>Args:</h3>
<b>task_no</b>: int. task number (0~7).<br>
<b>msg</b>: str. state message to show.<br>

<h3>Returns:</h3>
	
<br>

<hr>
<h2>io_set_so(sig_no, val)</h2>

<h3>Description:</h3>
set system i/o output bit.<br><br>
<h3>Args:</h3>
<b>sig_no</b>: int. signal number (0~959).<br>
<b>val</b>: int. value to set. (1 or 0)<br>

<h3>Returns:</h3>
&nbsp&nbsp <b>0</b>: ok<br>
&nbsp&nbsp <b>-1</b>: index-range exceeded.<br>

<br>

<hr>
<h2>io_get_in_bit(sigcode)</h2>

<h3>Description:</h3>
get user i/o input bit by sigcode.<br><br>
<h3>Args:</h3>
<b>sigcode</b>: int. signal-code (e.g. 30017 for fb3.di17)<br>

<h3>Returns:</h3>
signal value, 0 or 1.<br>
<br>

<hr>
<h2>io_set_out_bit(sigcode, val)</h2>

<h3>Description:</h3>
get user i/o input bit by sigcode.<br><br>
<h3>Args:</h3>
<b>sigcode</b>: int. signal-code (e.g. 30017 for fb3.do17)<br>
<b>val</b>: int. value to set. (1 or 0)<br>

<h3>Returns:</h3>
&nbsp&nbsp <b>0</b>: ok<br>
&nbsp&nbsp <b>-1</b>: index-range exceeded.<br>

<br>

<hr>
<h2>io_set_pulse_by_sigcode(sigcode, onoff, count, on_ms, off_ms, lag_ms, non_update)</h2>

<h3>Description:</h3>
make i/o pulse output<br><br>
<h3>Args:</h3>
<b>sigcode</b>: int. signal-code (e.g. 30017 for fb3.do17)<br>
<b>onoff</b>: <br>
&nbsp&nbsp <b>1</b>: on-pulse<br>
&nbsp&nbsp <b>0</b>: non-pulsed off (lagged-off)<br>
&nbsp&nbsp <b>-1</b>: off-pulse<br>
<b>count</b>: int. pulse count.<br>
<b>on_ms</b>: int. width of on (msec).<br>
<b>off_ms</b>: int. width of off (msec).<br>
<b>lag_ms</b>: int. width of lag (msec).<br>
<b>non_update</b>: <br>
&nbsp&nbsp <b>1</b>: don't update, if the pulse is already registered.<br>
&nbsp&nbsp <b>0</b>: re-register the pulse, if the pulse is already registered.<br>

<h3>Returns:</h3>
&nbsp&nbsp <b>0</b>: ok<br>
&nbsp&nbsp <b>-2</b>: already registered (when non_update==2).<br>

<br>

<hr>
<h2>io_assign_set_in_bit(sigcode)</h2>

<h3>Description:</h3>
set sigcode as assigned input i/o<br><br>
<h3>Args:</h3>
<b>sigcode</b>: int. signal-code (e.g. 30017 for fb3.di17)<br>

<h3>Returns:</h3>
0               ok -1              invalid sigcode<br>
<br>

<hr>
<h2>io_assign_set_out_bit(sigcode)</h2>

<h3>Description:</h3>
set sigcode as assigned output i/o<br><br>
<h3>Args:</h3>
<b>sigcode</b>: int. signal-code (e.g. 30017 for fb3.do17)<br>

<h3>Returns:</h3>
	0               ok -1              invalid sigcode<br>
<br>

<hr>
<h2>io_set_triggout(task_no, fbname, val, ofs, ax_no, type)</h2>

<h3>Description:</h3>
trigger-out output i/o<br><br>
<h3>Args:</h3>
<b>task_no</b>: int. task number (0~7).<br>
<b>fbname</b>: str. output variable name (e.g. fb3.do17, dob3)<br>
<b>val</b>: int. output value.<br>
<b>ofs</b>: int. offset-time (msec), or offset-distance (mm)<br>
<b>ax_no</b>: int. 0(TCP), 1~(axis-number) ; axis which observe when the type is OD<br>
<b>type</b>: <br>
&nbsp&nbsp <b>0x01</b>: OT (time-based)<br>
&nbsp&nbsp <b>0x02</b>: OD (distance-based) ; relative distance from previous position<br>
&nbsp&nbsp <b>0x04</b>: output even if it is an assigned i/o<br>
&nbsp&nbsp <b>0x10</b>: OX (absolute position of X)<br>
&nbsp&nbsp <b>0x20</b>: OY (absolute position of Y)<br>
&nbsp&nbsp <b>0x30</b>: OZ (absolute position of Z)<br>

<h3>Returns:</h3>
&nbsp&nbsp <b>1</b>: buffer full<br>
&nbsp&nbsp <b>2</b>: complete<br>
&nbsp&nbsp <b>0</b>: ok<br>
&nbsp&nbsp <b>-1</b>: robot axis is locked. (when, ax_no==0)<br>
&nbsp&nbsp <b>-2</b>: Nth robot axis is locked<br>
&nbsp&nbsp <b>-3</b>: unsupported coordinate system<br>
&nbsp&nbsp <b>-4</b>: user coordinate system number is unmatched<br>

<br>

<hr>
<h2>io_n_blocks()</h2>

<h3>Description:</h3>
	
<h3>Args:</h3>
	
<h3>Returns:</h3>
int. The number of i/o blocks<br>
<br>

<hr>
<h2>io_size_block_addr()</h2>

<h3>Description:</h3>
	
<h3>Args:</h3>
	
<h3>Returns:</h3>
int. The number of bits (address-space) in a block.<br>
<br>

<hr>
<h2>io_fbname_from_sigcode(sigcode, is_out)</h2>

<h3>Description:</h3>
get fbname from sigcode.<br><br>
<h3>Args:</h3>
<b>sigcode</b>: int. signal-code (e.g. 30017)<br>
<b>is_out</b>: 1               output 0               input<br>

<h3>Returns:</h3>
fbname of the sigcode (e.g. fb3.di17)<br>
<br>

<hr>
<h2>solve_expr_as_string(task_no, expr)</h2>

<h3>Description:</h3>
solve the expression and return the result value.<br><br>
<h3>Args:</h3>
<b>task_no</b>: int. task number (0~7).<br>
<b>expr</b>: str. expression of robot language.<br>

<h3>Returns:</h3>
str. the result value of the expression.<br>
<br>

<hr>
<h2>solve_expr_as_int(task_no, expr)</h2>

<h3>Description:</h3>
solve the expression and return the result value as integer.<br><br>
<h3>Args:</h3>
<b>task_no</b>: int. task number (0~7).<br>
<b>expr</b>: str. expression of robot language.<br>

<h3>Returns:</h3>
int. the result integer value of the expression.<br>
<br>

<hr>
<h2>exec_mode()</h2>

<h3>Description:</h3>
	
<h3>Args:</h3>
	
<h3>Returns:</h3>
&nbsp&nbsp <b>True</b>: exec-mode.<br>
&nbsp&nbsp <b>False</b>: not exec-mode.<br>

<br>

<hr>
<h2>cont_mode()</h2>
<h3>Description:</h3>	
<h3>Args:</h3>
	
<h3>Returns:</h3>
&nbsp&nbsp <b>True</b>: cont-mode.<br>
&nbsp&nbsp <b>False</b>: not cont-mode.<br>

<br>

<hr>
<h2>req_to_continue()</h2>

<h3>Description:</h3>
request host to be continue-mode
<h3>Args:</h3>
	
<h3>Returns:</h3>
	
<br>

<hr>
<h2>set_err_code(code)</h2>

<h3>Description:</h3>
set error code.<br><br>
<h3>Args:</h3>
<b>code</b>: int. error code.<br>

<h3>Returns:</h3>
	
<br>

<hr>
<h2>lang_timer()</h2>

<h3>Description:</h3>
	
<h3>Args:</h3>
	
<h3>Returns:</h3>
int. current value of language-timer.<br>
<br>

<hr>
<h2>set_lang_timer(timeout)</h2>

<h3>Description:</h3>
	set value to language-timer.<br><br>
<h3>Args:</h3>
<b>timeout</b>: int. initial value (msec)<br>

<h3>Returns:</h3>
	
<br>

<hr>
<h2>branch_to_addr(addr)</h2>

<h3>Description:</h3>
branch to the address.<br><br>
<h3>Args:</h3>
<b>addr</b>: str. address to branch.<br>

<h3>Returns:</h3>
	
<br>

<hr>
<h2>abs_path(name)</h2>

<h3>Description:</h3>
get absolute-path in the file-system.<br><br>
<h3>Args:</h3>
<b>name</b>: str. 'home', 'project', 'log', 'jobs', 'vars', 'backup', 'fbrr', 'module', 'apps_main', or 'help'<br>

<h3>Returns:</h3>
absolute-path<br>
<br>

<hr>
<h2>sci_open(port)</h2>

<h3>Description:</h3>
serial port open<br>        Args:<br>        port (int): serial port number<br><br>
<h3>Args:</h3>
	
<h3>Returns:</h3>
&nbsp&nbsp <b>0</b>: OK<br>
&nbsp&nbsp <b>'<0'</b>: Not OK<br>

<br>

<hr>
<h2>sci_close(port)</h2>

<h3>Description:</h3>
serial port close<br>        Args:<br>        port (int): serial port number<br>        <br>
<h3>Args:</h3>
	
<h3>Returns:</h3>
&nbsp&nbsp <b>0</b>: OK<br>
&nbsp&nbsp <b>-1</b>: already closed.<br>

<br>

<hr>
<h2>sci_send_bytes(port, data)</h2>

<h3>Description:</h3>
serial communication send bytes type data<br>xhost dbg not supporteds<br>
<h3>Args:</h3>
<b>port (int)</b>: serial port number<br>
<b>data (bytes)</b>: input data<br>

<h3>Returns:</h3>
&nbsp&nbsp <b>0</b>: OK<br>
&nbsp&nbsp <b>-1</b>: Not OK<br>

<br>

<hr>
<h2>sci_recv_bytes(port, len)</h2>

<h3>Description:</h3>
serial communication recv bytes type data<br>xhost dbg not supported
<h3>Args:</h3>
	
<h3>Returns:</h3>
	
<br>

<hr>
<h2>sci_clear_buf(port)</h2>

<h3>Description:</h3>
	
<h3>Args:</h3>
	
<h3>Returns:</h3>
	
<br>

<hr>
<h2>sci_send(port, data)</h2>

<h3>Description:</h3>
serial communication send string type data<br><br>
<h3>Args:</h3>
	
<h3>Returns:</h3>
&nbsp&nbsp <b>0</b>: OK<br>
&nbsp&nbsp <b>-1</b>: Not OK<br>

<br>

<hr>
<h2>sci_recv(port)</h2>

<h3>Description:</h3>
serial communication receive string type data<br><br>
<h3>Args:</h3>
<b>port (int)</b>: serial port number<br>

<h3>Returns:</h3>
&nbsp&nbsp <b>'str'</b>: receive string data<br>

<br>

<hr>
</body>
</html># 3.1.9 로봇언어 함수 blocking 문제 해결
## blocking 문제


앞 절에서 구현한 recv_msg( ) 함수는 한 가지 문제점이 있습니다.

이 함수를 호출하면 원격으로부터 응답이 오기를 기다리는데, 응답을 수신해야만 함수의 동작이 종료됩니다. 만일 응답해야 할 ArgosX 장치에 문제가 있어서 응답이 오지 않는다면, 함수는 종료하지 않고 영원히 대기합니다.



argosx_sub.py로부터 응답이 오지 않는 상황에서, job 프로그램이 어떻게 동작하는지 확인해봅시다.

시험을 위해 argosx_stub.py의 sleep_sec 이라는 변수를 찾아 값을 20으로 변경합니다. 이제 argosx_stub는 "req ~" 요청을 받으면 20초를 기다렸다가 "res ~" 응답을 하게 됩니다.

 

argosx_stub.py
``` python 
# const
buf_size = 0x8000    # 32kb ; permitted packet length
port_no = 54321      # port for ArgosX command
sleep_sec = 20        # delay before response
```


argosx_stub.py 를 재실행하고, 티치펜던트의 STEP FWD키로 job 프로그램을 한 행씩 실행해봅시다.

argosx.req를 실행하고 바로 이어서 argosx.res( )를 실행하면, 아래와 같이 req 실행 이후 20초간 커서를 움직일 수 없는 상태가 되어 버립니다. 이러한 상태를 blocking이라고 하는데, 사용자 조작성 측면에서 좋지 않습니다. 응답이 영구적으로 오지 않을 경우 로봇 제어기를 껐다 켜야 하는 상황까지 초래하기 때문에 문제가 있는 사양입니다.
</br>
![](../../_assets/image_29.png)

어떤 사양이 바람직할까요? 일반적으로, wait 문과 같이 어떤 상태를 기다리는 명령문의 경우 STEP FWD키를 누르면 대기하고 떼면 커서를 이동할 수 있습니다. 또한 timeout과 퇴피주소를 인수로 지정할 수 있어서, timeout이 되면 퇴피주소로 분기하는 예외 처리 동작이 가능합니다.
</br>
![](../../_assets/image_30.png)
di6을 10초 대기. timeout 시, *tout으로 분기.



## 실행모드와 계속모드


아래 순서도를 봅시다. Hi6 호스트(HOST)가 로봇언어 명령문을 호출할 때는 실행모드(execution-mode)와 계속모드(continue-mode)의 2가지 상태가 있습니다. 계속모드라면, 호스트는 해당 명령문을 다시 호출해줍니다.

호스트는 명령문을 일단 실행모드로 호출합니다. 대부분의 명령문들은 자신의 동작을 수행하고 즉각 종료하며, 호스트도 연속모드가 되지 않은 것을 확인하고 해당 명령문에 대한 처리를 완료시킵니다.
</br>
![](../../_assets/image_31.png)
</br>

그러나 일부 명령문들은 대기 동작을 가지고 있습니다. (IO 입력이나 이더넷 데이터 수신, 일정 시간, 로봇 동작 완료 등 어떤 상태나 사건(event)의 대기를 뜻합니다.) 호스트와 플러그인 간에는 아래의 절차가 수행됩니다.

* 플러그인의 대기 동작 명령문은 xhost.exec_mode( )로 모드를 확인합니다. True이면 실행모드, False이면 연속모드입니다. 실행모드임이 확인되면(Yes), timeout 인수로 전달받은 시간을 로봇언어용 timer에 설정한 후(set_lang_timer), Hi6 호스트에게 다음 번엔 연속모드로 호출해주기를 요청(xhost.req_to_continue)한 후 종료합니다.
* xhost.req_to_continue 실행 후 종료되면 연속모드입니다. Hi6 호스트는 해당 명령문을 다시 호출(call)해줍니다.
* 플러그인의 대기 동작 명령문은 xhost.exec_mode( )로 모드를 확인합니다. 연속모드임이 확인(No)되면 timer를 확인하여 timeout 인 경우 퇴피주소로 분기(branch_to_addr)하고 연속모드 요청 없이 종료합니다.
* timeout이 아니면, 대기조건이 완료되었는지(wait-complete condition?) 확인합니다. 완료이면 연속모드 요청 없이 그대로 종료하고, 미완료이면 Hi6 호스트에게 연속모드 요청(req_to_continue)을 하고 자신의 동작을 수행(execute command)한 후 종료합니다.
* Hi6 호스트는 이번 호출에서 연속모드 요청이 없었을 경우(continue-mode No), 해당 명령문 처리를 완료(complete)시킵니다.
 </br>
 ![](../../_assets/image_32.png)
</br>



이제 argosx.res( )도 이러한 대기동작 사양을 갖도록 개선해봅시다.



## comm 모듈을 non-blocking으로 만들기


이더넷 송수신을 위해 구현했던 comm 모듈은 내부적으로 socket 모듈을 활용합니다. socket은 기본적으로 blocking 모드입니다. 즉, UDP 수신 함수인 socket.recvfrom( ) 함수는 데이터가 수신될 때까지 리턴하지 않는다는 뜻입니다.

우선, 우리가 사용한 socket 인스턴스를 non-blocking 모드로 변경해야 합니다. comm.open( ) 함수에 아래와 같이 sock.setblocking(False) 를 삽입해줍니다.



comm.py
``` python
def open(ip_addr: str, port: int) -> int:
   """
   open socket for UDP communication
   Args:
      ip_addr     ip adddress of remote. e.g. "192.168.1.172"
      port        port# of remote. e.g. "192.168.1.172"
 
 
   Returns:
         0     ok
         -1    error
   """
   global raddr, sock
   try:
      raddr = (ip_addr, port)
      sock = socket.socket(family=socket.AF_INET, type=socket.SOCK_DGRAM)
      sock.setblocking(False)
   except socket.error as e:
      print("socket creation or binding error :", e)
      return -1
   logd('comm.open: ' + str(raddr))
   return 0
```

이제 socket.recvfrom( ) 함수는 데이터 수신이 없으면 blocking 없이 즉각 BlockingIOError 예외가 발생합니다. (데이터 수신이 있으면 당연히 즉각 정상 리턴합니다.)

comm.recv_msg( ) 함수에 BlockingIOError 예외 시 공문자열로 리턴하는 핸들링을 삽입합니다.



comm.py
``` python
def recv_msg():
   """
   wait msg from sock
   Returns:
      received string
   """
   if sock is None: return ""
 
 
   try:
      data, ip_port = sock.recvfrom(buf_size)
      bts = bytearray(data)
      msg = bts.decode()
      logd('response: ' + msg)
      return msg
   except BlockingIOError:
      return ""
   except Exception as e:
      print('exception from recv_msg(): ' + str(e))
      return ""

```


## res( ) 함수의 대기동작 구현


아래와 같이 res( ) 함수에 timeout과 addr_on_timeout(퇴피주소)의 2가지 인수를 추가합니다.

timeout을 지정하지 않으면 default값 -1이 적용되어 무한 대기가 되고, 퇴피주소를 지정하지 않으면, default값 -1이 적용되어 timeout 시 분기없이 다음 명령문으로 진행하도록 합니다.

기존 구현을 제거하고, xhost.exec_mode( ) 함수로 모드를 확인한 후, 실행모드이면 res_exec( ), 연속모드이면 res_cont( ) 함수를 호출하는 간결한 형태로 만듭니다.



roblang.py
``` python 
""" ArgosX Vision System interface - robot language
 
 
@author:    Jane Doe, BlueOcean Robot & Automation, Ltd.
@created:   2021-12-06
"""
 
from . import comm
from . import setup
 
import xhost
import typing
 
 
# type
int_or_str = typing.Union[int, str]
 
 
 
생략...
 
 
 
def res(timeout: int=-1, addr_on_timeout: int_or_str=-1) -> str:
   """
   wait response from ArgosX
    
   Args:
      timeout: (ms), Default(-1) means infinite.
      addr_on_timeout: branch address on timeout.
         (e.g. 99, "S7", "*TimeOut")
         Default(-1) means no branch.
 
 
   Returns:
      response string from ArgosX.
      "" if failed.
      e.g. "[30, 25.7, 11.9, 31.6, 12.8, -54.6]"
   """
   ret = ""
   if xhost.exec_mode():
      _res_exec(timeout)
   else:
      ret = _res_cont(addr_on_timeout)
   return ret
```



이제, res( ) 함수 바로 밑에 실행모드용 _res_exec( )함수를 구현합니다. timeout 인수값으로 로봇언어용 timer를 설정하고, 호스트에 연속모드 요청을 하고 종료합니다. 간단하죠?



roblang.py
``` python
이전 생략...
 
 
def _res_exec(timeout: int) -> None:
   """res() implementation for exec-mode"""
   xhost.set_lang_timer(timeout)
   xhost.req_to_continue()
```

실질적인 동작은 연속모드용 _res_cont( )함수에서 수행합니다. timeout이면 분기하는 동작을 _check_timeout_and_branch( ) 함수로 만들었습니다.

timeout인 경우 sigcode_err 출력신호를 켜고 즉각 종료합니다.

timeout이 아니면, 데이터 수신을 수행하는데, 수신 데이터가 없으면 호스트에 연속모드 요청을 한 후(xhost.req_to_continue) 공문자열로 리턴하게 됩니다. 



roblang.py
``` python
이전 생략...
 
 
def _res_cont(addr_on_timeout: int_or_str) -> str:
   """res() implementation for cont-mode"""
   val = 0
   msg = ""
   timeout = _check_timeout_and_branch(addr_on_timeout)
   if timeout:
      val = 1
   else:
      msg = comm.recv_msg()
      print(msg)
      if msg=="res fail":
         val = 1
         msg = ""
      elif msg=="":    # No response
         xhost.req_to_continue()
      else:    # Normal response
         msg = get_base_shift_array_from_res(msg)
   xhost.io_set_out_bit(setup.sigcode_err, val)
   return msg
 
 
 
def _check_timeout_and_branch(addr_on_timeout: int_or_str) -> bool:
   """if timeout, do branch.
 
   Returns:
      True     timeout. branched.
      False    not-timeout.
   """
   timer = xhost.lang_timer()
   if timer!=0: return False  # not-timeout
   # timeout!
   if addr_on_timeout==-1:
      return True
   # make as str, unconditionally
   str_addr = str(addr_on_timeout)
   xhost.branch_to_addr(str_addr)
   return True
```



## non-blocking 동작 시험


이제 원하는 사양이 되었는지 확인해봅시다. 가상 제어기를 재실행한 후, 티치펜던트의 STEP FWD키로 job 프로그램을 한 행씩 실행해봅시다.

argosx.req를 실행하고 바로 이어서 argosx.res( )를 실행하면, 응답이 아직 안 왔기 때문에 완료되지 않습니다. STEP FWD키를 떼면 스텝전진 표시가 꺼지면서 커서를 움직일 수 있습니다. STEP FWD를 누르면 다시 대기상태가 계속됩니다.

STEP FWD를 누르고 있는 상태에서 req( ) 수행 후 20초가 지나면 수신이 완료됩니다.
</br>
![](../../_assets/image_33.png)



이제, timeout 인수를 추가해봅시다. 3000ms를 지정하고 다시 실행합니다. STEP FWD를 누르고 3초가 지나면 다음 명령어로 넘어가 버립니다.

job
```
var str=argosx.res(3000) # 응답 대기
 
print str
```

이제, 퇴피스텝도 추가해봅시다. 아래와 같이 교시하고 res( )에서 STEP FWD를 3초 이상 누르면, 행번호 99로 분기하여 print "timeout"이 수행되는 것을 볼 수 있습니다.



job
```
... 이전 생략
      
     var str=argosx.res(3000,99) #응답 대기
     print str
     if str==""
       print "req error"
       stop
     else
       var sft=Shift(str) # shift 배열 문자열을 shift 데이터로 변환
       print sft.x, sft.y, sft.z, sft.rx, sft.ry, sft.rz
     endif
      
     argosx.close()
     end
      
  99 print "timeout"
     end
```

# 3.2 실전 프로젝트 : ArgosX - callback


Hi6 제어기의 동작에는 모드 변경, 모터ON, 리셋, 기동, 정지, Accuracy OK 등 주요 이벤트들이 있습니다. 각 plug-in 들은 이러한 이벤트에 대해 고유한 동작을 수행하도록 함수를 등록시켜 둘 수 있습니다.

이러한 함수를 callback 함수라고 합니다. python 코드 내에서 능동적으로 호출하는 것이 아니라, 누군가(Hi6 host)에 의해 호출 당한다는 뜻에서 callback 이라고 하는 것입니다.



* callback 함수 등록
* callback 함수 구현
* callback 함수 참조설명서# 3.2.1 callback 함수 등록


callback 함수를 등록하는 방법은 아주 간단합니다. 각 이벤트 별로 callback 함수명이 정해져 있으며, plug-in 코드에 이 함수명으로 함수를 정의해놓기만 하면, plug-in이 import 될 때 자동으로 등록됩니다.


다시 <U>ArgosX와 interface plug-in의 사양</U>의 기타 기능을 참고해봅시다.

로봇의 모터ON/OFF에 따라서 ArgosX의 LED조명도 함께 켜지고 꺼져야 합니다. 구체적으로는 ArgosX에 "light-on", "light-off"라는 명령을 송신해야 합니다.



callback 함수들을 별도의 파일(python 모듈)에 다음과 같이 작성합시다. 일단은 구체적인 동작 없이, 시험을 위해 문자열 출력만 합니다.



callback.py (시험용)
```python
def on_motor_on() -> int:
   """(callback) on motor-on
   Returns: 0
   """
   print('on_motor_on')
   return 0
 
 
def on_motor_off() -> int:
   """(callback) on motor-off
   Returns: 0
   """
   print('on_motor_off')
   return 0
```

callback 모듈을 entry 파일에서 import합니다.



main.py
```python
이전 생략...
 
from . import setup
from .roblang import *
from .setup import *
from .callback import *
 
import xhost
 
이후 생략...

```
이제 제어기를 재부팅한 후, 모터ON하고 StepFWD로 import argosx를 실행합니다.

이 상태에서, 모터ON, 모터OFF를 할 때마다 콘솔창에 아래와 같이 출력되면, callback 함수를 잘 정의한 것입니다.
```
on_motor_on

on_motor_off
```
# 3.2.2 callback 함수 구현
callback 함수가 잘 호출되는 것을 확인했으니, 이제 실제 동작을 구현해봅시다.

<U>ArgosX와 interface plug-in의 사양</U>을 확인해보면, ArgosX 하드웨어측으로 "light-on"과 "light-off" 메시지를 송신하기만 하면 됩니다.



comm 모듈에 이미 이더넷 문자열 송신 기능이 구현되어 있으므로, 아래와 같이 간단하게 호출만 하면 됩니다.
```
comm.send_msg("light-on")
comm.send_msg("light-off")
```

그런데 이 구현은 한가지 문제가 있습니다. 로봇언어 명령문 argosx.init를 통해 comm.open( ) 함수가 호출된 상태라면 정상적으로 문자열이 송신되지만, 호출되지 않은 상황에서는 송신이 안됩니다.

또한, comm.close( )로 통신이 닫혀버린 상황에서도 송신이 안됩니다. 

따라서, 통신이 닫혀있으면 열고 송신한 후 다시 닫아주는 동작까지 해주는 문자열 송신 함수를 정의해야 합니다.


comm.py와 같은 폴더에 comm_ex.py 파일을 아래와 같이 작성합니다.



comm_ex.py

``` python
""" ArgosX Vision System interface - main
 
 
@author:    Jane Doe, BlueOcean Robot & Automation, Ltd.
@created:   2021-12-07
"""
 
 
from . import setup
from . import comm
 
 
def send_msg_once(msg: str) -> int:
   was_closed = (comm.is_open()==False)
   if was_closed: comm.open(setup.ip_addr, setup.port)
   iret = comm.send_msg(msg)
   if was_closed: comm.close()
   return iret

```

이제 comm_ex 모듈을 import하여 callback 함수를 아래와 같이 간단하게 구현할 수 있습니다.



callback.py

``` python
""" ArgosX Vision System interface - callback functions
 
 
@author:    Jane Doe, BlueOcean Robot & Automation, Ltd.
@created:   2021-12-07
"""
 
from . import comm_ex
 
 
def on_motor_on() -> int:
   """(callback) on motor-on
   Returns: 0
   """
   print('on_motor_on')
   return comm_ex.send_msg_once("light-on")
 
 
def on_motor_off() -> int:
   """(callback) on motor-off
   Returns: 0
   """
   print('on_motor_off')
   return comm_ex.send_msg_once("light-off")
```

먼저, argosx_stub를 명령 프롬프트나 vscode에서 실행해놓습니다.

가상제어기를 재부팅한 후, job 파일을 argosx.init( )까지 실행합니다. 이 상태에서 아래와 같이 동작하면, 조명 기능의 정상 동작은 확인된 것입니다.



<U>__argosx_stub측 (ArgosX 역할의 server)__</U>

motor OFF, motor ON을 할 때마다, 콘솔 출력에 아래와 같은 문자열이 출력됩니다.
```
request : light-off
LED light is OFF

request : light-on
LED light is ON
```
# 3.2.3 callback 함수 참조설명서

<table>
  <thead>
    <tr>
      <th style="text-align:left">Python callback 함수들</th>
      <th style="text-align:left">main s/w 내에서 호출되는 시점</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>on_app_init()</td>
      <td>
       자기 진단 후
      </td>
    </tr>
   <tr>
      <td>on_before_self_diagnosis_proc()</td>
      <td>
       자기 진단 전
      </td>
    </tr>
    <tr>
      <td>on_mot_servoerror_detect()</td>
      <td>
       서보 에러 검지
      </td>
    </tr>
    <tr>
      <td>on_mot_on_ready_check_remote_auto()</td>
      <td>
       모터 on 준비 완료, 원격/자동
      </td>
    </tr>
    <tr>
      <td>on_entry_teach_mode()</td>
      <td>교시 모드 진입</td>
    </tr>
    <tr>
      <td>on_entry_auto_mode()</td>
      <td>
        자동 모드 진입
      </td>
    </tr>
     <tr>
      <td>on_system_status_chk_proc()</td>
      <td>시스템 이상 상태 여부 확인 (10ms)</td>
    </tr>
    <tr>
      <td>on_period_low()</td>
      <td>
       최저 우선순위 주기 호출(5ms)
      </td>
    </tr>
   <tr>
      <td>on_motor_on()</td>
      <td>
       모터 on
      </td>
    </tr>
    <tr>
      <td>on_motor_off()</td>
      <td>
       모터 off
      </td>
    </tr>
    <tr>
      <td>on_stop(task_no)</td>
      <td>
       정지
      </td>
    </tr>
    <tr>
      <td>on_restart(task_no)</td>
      <td>기동</td>
    </tr>
    <tr>
      <td>on_reset0()</td>
      <td>
       reset 0	
      </td>
    </tr>
     <tr>
      <td>on_cur_job_selected_by_tp(task_no)</td>
      <td>TP에 의한 job program 선택</td>
    </tr>
    <tr>
      <td>on_step_func_no_change_for_clear(task_no,..)</td>
      <td>
       프로그램 카운터 변경
      </td>
    </tr>
    <tr>
      <td>on_job_end(task_no)</td>
      <td> job end문 실행
      </td>
    </tr>
    <tr>
      <td>init_signal_output_status()</td>
      <td>응용 신호 출력 초기화</td>
    </tr>
    <tr>
      <td>set_ext_io_sig_proc()</td>
      <td>
       할당 신호 처리	
      </td>
    </tr>
    <tr>
      <td>is_assigned_input(sigcode) /</br>
      is_assigned_output(sigcode)</td>
      <td>sigcode가 할당된 입/출력 신호인지 여부 리턴</td>
    </tr>
    <tr>
      <td>update_input_assign_info() / </br>
      update_output_assign_info()</td>
      <td>host의 할당된 입/출력 신호 look-up table 설정</td>
    </tr>
  </tbody>
</table># 3.3.1 실전 프로젝트 : ArgosX - 설정화면 U/I 개발

플러그인에 각종 설정이 있다면, 사용자에게 설정값을 보여주고 새로운 값으로 변경할 수 있도록 해주는 설정화면이 필요합니다.

이번 챕터에서는 ArgosX용 설정화면 U/I를 구현하고 특정 메뉴 위치에 배치하는 과정을 실습해봅시다.
</br>


* ArgosX의 설정화면 사용자 인터페이스의 사양
* 설정화면의 레이아웃
* 설정화면의 동작
* 설명화면 메뉴의 주입
* 설정화면의 값 불러오기와 저장하기
* 설정 파일 불러오기와 저장하기
* F 버튼 동작 - default 값으로 초기화
# 3.3.1 ArgosX의 설정화면 사용자 인터페이스의 사양

아래와 같은 사양으로 설정화면 사용자 인터페이스를 만들어 봅시다.

* 설정 - 응용 파라미터 메뉴에 ArgosX 설정 화면에 진입하는 메뉴가 있다.
* ArgosX 설정화면에서 ArgosX 장치의 IP 주소와 port 번호를 설정할 수 있다.
* 설정 내용은 argosx.json 파일에 json 파일 형식으로 저장된다.
* 화면 전체를 default 값으로 만드는 F버튼 (Initialize All)을 제공한다.
* 현재 선택한 항목만 default 값으로 만드는 F버튼 (Initialize One)을 제공한다.

![](../../_assets/image_34.png)





job 프로그램에서의 import argosx 의 수행 여부와 무관하게, 설정화면을 열고 설정값을 확인/변경할 수 있게 하고자 합니다.

argosx/info.json 파일의 startup 항목을 "manual"에서 "boot"로 바꿉니다. 이제, 제어기 booting 시에 argosx가 import 되므로, job 프로그램에서 import argosx 를 수행할 필요가 없습니다.

(여전히, HRScript에서 설정할 수도 있습니다.)# 3.3.2 설정화면의 레이아웃


argosx 의 부모인 apps/ 폴더에 대해 vscode를 여십시오.

(이렇게 하는 이유는 argosx의 사용자 인터페이스가 apps/_common/의 파일을 참조해야 하기 때문입니다. Live server가 참조하는 파일들을 모두 포함하는 폴더를 workspace의 최상위 폴더로서 열어야 합니다.)
</br>
![](../../_assets/image_35.png)
</br>



New Folder 버튼을 클릭하여 ui/ 폴더를 만듭니다.
</br>
![](../../_assets/image_36.png)
</br>




ui/setup.html 파일을 생성합니다.
</br>
![](../../_assets/image_37.png)
</br>



내용은 아래와 같이 작성합니다.



setup.html
``` html
<!DOCTYPE html:5>
<!--
   @author: Jane Doe, BlueOcean Robot & Automation, Ltd.
   @brief: ArgosX Vision System interface - setup
   @create: 2021-12-06
-->
<html>
  
<head>
   <title>ArgosX Vision System - setup</title>
   <meta http-equiv=Content-Type content='text/html; charset=utf-8'>
   <link rel='stylesheet' href='../../_common/css/style.css' type=text/css rel=stylesheet>
</head>
  
<body>
   <div>
      <div id='contents'>
         <span class='col0' name='ip_addr'>IP address</span>
         <input class='col1' type='text' name='ip_addr' id='ip_addr_0' size='3'/>
         .
         <input class='col1' type='text' name='ip_addr' id='ip_addr_1' size='3'/>
         .
         <input class='col1' type='text' name='ip_addr' id='ip_addr_2' size='3'/>
         .
         <input class='col1' type='text' name='ip_addr' id='ip_addr_3' size='3'/>
         <br>
         <span class='col0' name='port'>Port#</span>
         <input class='col1' type='text' id='port' size='5'/>
         <br>
         <span class='col0' name='fail_out_sig'>Failure output signal</span>
         <input class='col1' type='text' id='fail_out_sig' size='5'/>
      </div>
   </div>
</body>
</html>
```

setup.html이 열린 상태에서 우하단의 Go Live 버튼을 클릭하여 Live server를 실행합니다.
</br>
![](../../_assets/image_38.png)
</br>




혹시 Visual Studio Code에 대한 보안 경고가 나타나면, 통신 허용을 모두 체크하고 액세스 허용 버튼을 클릭해주십시오.
</br>
![](../../_assets/image_39.png)
</br>




혹은, setup.html에 대해 우버튼으로 팝업 메뉴를 열고 "Open with Live Server"를 선택합니다.
</br>
![](../../_assets/image_40.png)
</br>




Chrome 브라우저가 열리면서, 대략적인 레이아웃을 확인할 수 있습니다.



setup.html의 대략적인 레이아웃
</br>
![](../../_assets/image_41.png)
</br>



# 3.3.3 설정화면의 동작

setup.html의 head 안에 아래와 같이 script를 추가합니다.

setup.js 파일은 이 설정화면에만 적용되는 동작들을 구현하기 위한 script 파일입니다. 아래에서 다시 설명됩니다.



ui/setup.html
``` html
<!DOCTYPE html:5>
<!--
   @author: Jane Doe, BlueOcean Robot & Automation, Ltd.
   @brief: ArgosX Vision System interface - setup
   @create: 2021-12-06
-->
<html>
  
<head>
   <title>ArgosX Vision System - setup</title>
   <meta http-equiv=Content-Type content='text/html; charset=utf-8'>
   <link rel='stylesheet' href='../../_common/css/style.css' type=text/css rel=stylesheet>
   <script src='../../_common/js/jquery-3.6.0.min.js'></script>
   <script src='../../_common/js/Parser.js'></script>
   <script src='../../_common/js/sigcode.js'></script>
   <script src='../../_common/js/dst_setup.js'></script>
   <script src='./setup.js'></script>
   <script>
      $(document).ready(init);
   </script>
</head>
  
<body>
   <div>
      <div id='contents'>
         <span class='col0' name='ip_addr'>IP address</span>
         <input class='col1' type='text' name='ip_addr' id='ip_addr_0' size='3'/>
         .
         <input class='col1' type='text' name='ip_addr' id='ip_addr_1' size='3'/>
         .
         <input class='col1' type='text' name='ip_addr' id='ip_addr_2' size='3'/>
         .
         <input class='col1' type='text' name='ip_addr' id='ip_addr_3' size='3'/>
         <br>
         <span class='col0' name='port'>Port#</span>
         <input class='col1' type='text' id='port' size='5'/>
         <br>
         <span class='col0' name='sigcode_err'>Failure output signal</span>
         <input class='col1' type='text' id='sigcode_err' size='5'/>
      </div>
      <div id='guidebar'></div>
   </div>
</body>
</html>
```



ui/ 폴더에 setup.js 파일을 추가하고 내용을 아래와 같이 작성합니다.



ui/setup.js
``` js
///@author: Jane Doe, BlueOcean Robot & Automation, Ltd.
///@brief: ArgosX Vision System interface - setup
///@create: 2021-12-06
 
 
 
function init()
{
   setDomPath("/apps/argosx/svr_setup");
   setUpdateGuideBar(updateGuideBar);
   setUpdateData(updateData);
   onReady();
}
 
 
///@return     f-button infos array
function initButtonBar()
{
   console.log('initButtonBar()'); 
   var btn_infos = [
   ]
   return btn_infos;
}
 
 
///@brief      have guidebar display message on clicking widget
function updateGuideBar()
{
   let sg = setGuideBarMsg;
   let msg_ip_addr = 'Enter the IP address of ArgosX.'
   let msg_port = 'Enter the port # of ArgosX.'
   let msg_sigcode = 'Enter the number of the signal to assign.[0 - 4096]';
    
   sg('ip_addr', msg_ip_addr);
   sg('port', msg_port);
   sg('sigcode_err', msg_sigcode);
}
 
 
///@param[in]  data
///@param[in]  to_data     true; widget->data, false; widget<-data
function updateData(data, to_data)
{
   ddx_edit_ip(data, 'ip_addr', to_data);
   ddx_edit_i(data, 'port', to_data);
   ddx_edit_sig(data, 'sigcode_err', to_data);
}
```

updateGuideBar( ) 함수는 입력 element에 커서가 위치했을 때, 안내 프레임에 어떤 메시지를 표시할 지를 setGuideBarMsg( ) 함수를 호출하여 지정합니다.



__setGuideBarMsg(element의 id 혹은 name, 표시할 message)___



가령, 위의 예에서 sg('ip_addr', msg_ip_addr);는 name이 'ip_addr'인 input element에 커서가 위치했을 때, msg_ip_addr 문자열을 안내 프레임에 표시하라는 설정입니다.



처음 설정화면이 열렸을 때, 현재 설정 값을 불러오고 [OK] 버튼을 클릭하면 저장해야 합니다.

이러한 설정값과 관련된 동작은 setDomPath("/apps/argosx/svr_setup");의 호출과 updateData( ) 함수의 정의로 구현됩니다.

자세한 내용은 이 후의 절에서 다시 설명하겠습니다.# 3.3.4 설명화면 메뉴의 주입

hello_world 예제에서 이미 메뉴 주입을 실습해 본 바 있습니다.

ArgosX 의 설정 화면도 티치펜던트의 시스템 - 응용파라미터 메뉴 밑에 주입해 봅시다.



ui/ 폴더 밑에 아래와 같은 menu.json 파일을 생성합니다.



menu.json
``` json
[
    {
        "path": "system/appl/",
        "id": "argosx",
        "icon": "argosx/ui/lm_argosx.png",
        "label": "ArgosX Vision",
        "url": "argosx/ui/setup.html"
    }
]
```


이번에는 그림 icon을 넣어봅시다. 아래 2개의 site를 이용해서 104x104 투명배경 png icon을 얻었습니다.



Bootstrap Icons (https://icons.getbootstrap.com/#icons) : open 소스 icon 라이브러리. SVG 벡터 파일 형태로 제공.

EZGIFCOM (https://ezgif.com/svg-to-png) : SVG 파일을 원하는 해상도의 PNG 파일로 온라인 변환.

</br></br>
![](../../_assets/lm_argosx.png) lm_argosx.png 의 예 (이 그림을 다운받아 사용해도 됩니다.)



이제 가상 메인보드과 가상 티치펜던트를 재실행 합니다.


시스템 - 응용 파라미터 메뉴로 진입하면 아래와 같이 새로 추가된 ArgosX Vision 메뉴 항목이 보입니다.
</br>
![](../../_assets/image_42.png)
</br>



메뉴를 선택하면 잠시 후, 우리가 작성한 레이아웃이 나타납니다.
</br>
![](../../_assets/image_43.png)
</br>






# 3.3.5 설정화면의 값 불러오기와 저장하기



설정화면을 열면, python 플러그인 내에 저장되어 있는 설정값들을 javascript의 중간 객체로 불러온 후, 화면의 HTML element로 표시합니다.

사용자가 값을 수정한 후 [OK] 버튼을 클릭하면, 화면의 HTML element가 javascript의 중간 객체로 만들어진 후, python 플러그인 내의 변수에 저장됩니다.



이러한 전달을 수행하기 위해, javascript의 updateData( ) 함수와 python의 setup 모듈의 getter, putter 함수를 구현해야 합니다.
</br>![](../../_assets/image_44.png)</br>






## element ↔ javascript object


setup.js
``` js
이전 생략...
 
 
///@param[in]  data
///@param[in]  to_data     true; element->data, false; element<-data
function updateData(data, to_data)
{
   ddx_edit_ip(data, 'ip_addr', to_data);
   ddx_edit_i(data, 'port', to_data);
   ddx_edit_sig(data, 'sigcode_err', to_data);
}
```


element와 javascript 객체간 값의 양방향 전달을 updateData( ) 함수로 정의했습니다. 매개변수 data는 javascript 객체입니다. to_data는 전달방향을 나타내는 boolean 변수인데, true이면 element → data 방향, false이면 element ← data 방향입니다.

이러한 전달은 DOM API 혹은 jquery를 활용하여 직접 구현해도 됩니다. 하지만, dst_setup.js가 제공하는 ddx (dynamic data exchange) 함수들을 사용하면 좀 더 간결하게 전달을 구현할 수 있습니다.


# 3.2.3 callback 함수 참조설명서
|함수 signature|HTML element|data type|설명|
|---|---|---|---|
|ddx_edit(data, name, to_data)|```<input type='text'>```|string||
|ddx_edit_i(data, name, to_data)|```<input type='text'>```|integer||
|ddx_edit_sig(data, name, to_data)|```<input type='text'>```|integer|범용 I/O 신호 설정용</br>element값이 sigcode이면 그대로 전달.</br>element값이 fb?.?의 형태이면 sigcode로 변환해 전달.|
|ddx_edit_ip(data, name, to_data)|```<input type='text'>``` x 4개|string|IP address 설정용.</br>name이 'ip'이면 4개의 element의 id는 각기 'ip_0', 'ip_1', 'ip_2', 'ip_3'이어야 한다.</br>data는 "xxx.xxx.xxx.xxx"의 형태로 저장된다.|
|ddx_check(data, name, to_data)|```<input type='checkbox'>```|boolean||
|ddx_radio(data, name, to_data)|```<input type='radio'>```x N개|integer|radio element들은 각자 unique한 value 속성값을 가져야 함.|

</br>

## javascript 객체 ↔ python data


setup.js
``` js
///@author: Jane Doe, BlueOcean Robot & Automation, Ltd.
///@brief: ArgosX Vision System interface - setup general
///@create: 2021-12-06
 
 
 
function init()
{
   setDomPath('/apps/argosx/svr_general');
   setUpdateGuideBar(updateGuideBar);
   setUpdateData(updateData);
   onReady();
}
 
 
...이후 생략
```

초기화 단계에서 setDomPath( ) 함수로 "/apps/argosx/svr_general" 경로를 지정했습니다.

argosx를 비롯해 모든 플러그인은 /apps/ 밑에 배치됩니다. 그리고, 경로 마지막 이름은 "svr_"에 설정 그룹명을 붙인 형태입니다.

</br>

**/apps/{app name}/svr_{setup group name}**

</br>
플러그인은 1개 혹은 여러 개의 설정 화면을 가질 수 있는데, 한 화면의 데이터는 하나의 객체로 다루어지고 이를 설정 그룹(setup group)이라고 합니다. 각 설정 그룹에는 자유롭게 이름을 부여할 수가 있는데, 이것이 설정 그룹명입니다. 이 예제에서는 설정 그룹명을 'general'로 정했습니다.

설정 그룹명에 접두어 "svr_" 대신, "get_"과 "put_"을 붙이면 각각이 getter와 putter 서비스 함수명이 됩니다. 또한, getter 함수명 끝에 '_def'를 붙이면 default값을 얻는 default getter 서비스 함수명이 됩니다.



따라서, 위의 예에서 default getter, getter, putter 서비스 함수명은 각각 get_general_def( ), get_general( ), put_general( ) 입니다.



프로젝트 폴더인 argosx/ 에 setup.py 파일을 추가합니다.
<table>
  <thead>
    <tr>
      <th style="text-align:left"></th>
      <th style="text-align:left">함수</th>
      <th style="text-align:left">호출되는 시점</th>
      <th style="text-align:left">동작</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>default getter</td>
      <td>get_general_def( )</td>
      <td>default 값이 요구될 때,</td>
      <td>default 설정값들을 하나의 객체에 속성값으로 저장하여, 전체 객체를 리턴.</td>
    </tr>
    <tr>
      <td>dgetter</td>
      <td>get_general( )</td>
      <td>	설정 화면이 열릴 때,</td>
      <td>python 플러그인이 가진 설정값들을 하나의 객체에 속성값으로 저장하여, 전체 객체를 리턴.</td>
    </tr>
    <tr>
      <td>putter</td>
      <td>put_general( )</td>
      <td>설정 화면의 값을 [ok] 혹은 [apply] 버튼으로 저장할 때,</td>
      <td>body 매개변수로 전달된 객체의 속성값들을 읽어 python 플러그인에 저장.</td>
    </tr>
  </tbody>
</table>


</br>

이제 각 함수를 구현합시다. 각 속성의 key는 ddx 함수에서 사용한 이름과 동일해야 합니다.

setup.py
``` python

""" robot application - argosx - setup
 
 
@author:    Jane Doe, BlueOcean Robot & Automation, Ltd.
@created:   2021-12-06
"""
 
 
def get_general_def() -> dict:
   """
   Returns:
      default value of setting
   """
   print('def_general_def()')
 
 
   data_def = {
      'ip_addr': '192.168.1.100',
      'port' : 54321,
      'sigcode_err': 5
   }
    
   return data_def
 
 
def get_general() -> dict:
   """
   Returns:
      setting dict.
   """
 
   print('get_general()')
    
   ret = {}
   ret["ip_addr"] = ip_addr
   ret["port"] = port
   ret["sigcode_err"] = sigcode_err
    
   return ret
 
    
def put_general(body: dict) -> int:
   """
   Args:
      body  setting dict.
    
   Returns:
      0
   """
   global ip_addr, port, sigcode_err
   print('put_general()')
 
   ip_addr = body["ip_addr"]
   port = body["port"]
   sigcode_err = body["sigcode_err"]
 
   save_to_setup_file(body) # save to file
    
   return 0
```

setup.py의 전역변수 초기화 부분도 이제 default getter 함수를 활용하는 방식으로 교체하여 중복 코드를 없애 봅시다.



setup.py
``` python
...이전 생략
 
 
gen_def = get_general_def()
ip_addr : str = gen_def['ip_addr']
port : int = gen_def['port']
sigcode_err = gen_def['sigcode_err']
```



## 동작 시험


이제, 가상제어기를 재시작하고, import argosx를 수행한 후, ArgosX의 설정화면으로 진입해봅시다. 아래와 같이 기본 설정값들이 표시됩니다.
</br> ![](../../_assets/image_45.png) </br>




IP address를 192.168.1.172로 변경하고, Failure output signal에 3.4 <enter>키를 눌러 fb3.4로 바꾼 후 [OK] 버튼을 눌러 빠져나갑니다.
</br>

다시 화면에 진입하여 새로 설정한 값들이 잘 나타나면 정상적으로 동작한 것입니다.
</br> ![](../../_assets/image_46.png) </br>






# 3.3.6 설정 파일 불러오기와 저장하기

앞 절에서는 설정 값을 python 변수에 저장하고, 다시 불러오는 실습을 했습니다.

제어기를 껐다 켜더라도 이 설정이 보관되려면 파일에 저장해야 합니다.



setup.py에 file에 저장하는 함수 save_to_setup_file( )와 file에서 불러오는 함수 load_from_setup_file()를 다음과 같이 추가합니다.



setup.py
``` python 

""" robot application - argosx - setup
 
 
@author:    Jane Doe, BlueOcean Robot & Automation, Ltd.
@created:   2021-12-06
"""
 
import xhost
import json
 
 
fname_setup = 'argosx.json'
 
 
def get_general_def() -> dict:
   """
   Returns:
      default value of setting
   """
   print('def_general_def()')
 
   data_def = {
      'ip_addr': '192.168.1.100',
      'port' : 54321,
      'sigcode_err': 5
   }
    
   return data_def
 
 
def get_general() -> dict:
   """
   Returns:
      setting dict.
   """
 
   print('get_general()')
    
   ret = {}
   ret["ip_addr"] = ip_addr
   ret["port"] = port
   ret["sigcode_err"] = sigcode_err
    
   return ret
 
    
def put_general(body: dict) -> int:
   """
   Args:
      body  setting dict.
    
   Returns:
      0
   """
   global ip_addr, port, sigcode_err
   print('put_general()')
 
   ip_addr = body["ip_addr"]
   port = body["port"]
   sigcode_err = body["sigcode_err"]
 
   save_to_setup_file(body) # save to file
    
   return 0
 
 
def load_from_setup_file() -> int:
   """
   load setup file
   Returns:
         0     ok
         -1    error
   """
   global ip_addr, port, sigcode_err
 
   pathname = xhost.abs_path('project') + fname_setup
   try:
      with open(pathname, 'r') as file:
         data = json.load(file)
         ip_addr = data['ip_addr']
         port = data['port']
         sigcode_err = data['sigcode_err']
   except:
      print('file not found: ', pathname)
      return -1
   return 0
 
 
def save_to_setup_file(body: dict) -> int:
   """
   save to setup file
   Returns:
         0     ok
         -1    error
   """
   pathname = xhost.abs_path('project') + fname_setup
   try:
      with open(pathname, 'w') as file:
         json.dump(body, file, indent='\t')
   except:
      print('failed in file writing: ', pathname)
      return -1
   return 0
 
 
 
# attributes getter/setter
def get_ip_addr() -> str:
   return ip_addr
 
 
def set_ip_addr(addr: str):
   global ip_addr
   ip_addr = addr
 
def get_port() -> int:
   return port
 
def set_port(_port: int):
   global port
   port = _port
 
gen_def = get_general_def()
ip_addr : str = gen_def['ip_addr']
port : int = gen_def['port']
sigcode_err = gen_def['sigcode_err']
```
</br>

설정화면 값을 python 변수에 저장하는 함수 put_general( )의 마지막 동작으로서 save_to_setup_file(body) 호출을 추가했습니다.

또한, 아래와 같이 main.py의 on_app_init( ) 함수 내에 setup.load_from_setup_file( ) 호출을 추가했습니다. 이제, argosx 플러그인이 import되는 순간 setup.load_from_setup_file( ) 이 호출되어 설정이 load됩니다.

</br>

main.py
```python 
""" ArgosX Vision System interface - main
 
 
@author:    Jane Doe, BlueOcean Robot & Automation, Ltd.
@created:   2021-12-06
"""
 
from . import setup
from .roblang import *
from .setup import *
from .callback import *
 
import xhost
 
 
def attr_names() -> tuple:
   """Returns the names of the attributes to be exposed."""
   return ("ip_addr", "port")
 
 
def on_app_init() -> int:
   """(callback) called just after self-diagnosis
   Returns:
      0
   """
   print('[argosx] on_app_init();')
   setup.load_from_setup_file()
   xhost.io_assign_set_out_bit(setup.sigcode_err)
   return 0
```
</br>


이제 앞 절에서 한 시험을 반복해봅시다.

ArgosX의 설정화면을 열고, IP 주소, 출력 할당 신호 등의 설정값을 바꾼 후, [OK] 키로 저장합니다.

가상제어기의 project/ 폴더에 아래와 같은 argosx.json 파일이 생성되었는지 확인합니다.

</br>
argosx.json (IP주소를 192.168.1.172로, 에러 시 출력 할당 신호를 fb3.4로 변경했을 때의 예)

```json
{
    "port": 54321,
    "ip_addr": "192.168.1.172",
    "sigcode_err": 30004
}
```


</br>
main S/W를 재실행 한 후 ArgosX 설정화면을 열어봅니다. 파일에 저장했던 설정값들이 정상적으로 로드되었는지 확인합니다.

![](../../_assets/image_46.png)

# 3.3.7 F버튼 동작 - default 값으로 초기화



<U>ArgosX의 설정화면 사용자 인터페이스의 사양</U>에서 계획한 대로, 화면의 설정을 default 값으로 만드는 F버튼들을 구현해봅시다.

setup.js에 아래와 같이 InitButtonBar( ) 함수를 추가합니다.

setup.js
``` js
...이전 생략
 
 
function init()
{
   setDomPath(domPath);
   setUpdateGuideBar(updateGuideBar);
   setUpdateData(updateData);
   onReady();
}
 
 
///@return     f-button infos array
function initButtonBar()
{
   console.log('initButtonBar()'); 
   var btn_infos = [
      {
         label: 'Initialize All',
         script: 'setAllValueAsDef();'
      },
      {
         label: 'Initialize One',
         script: 'setSelectedValueAsDef();'
      }
   ]
   return btn_infos;
}
 
 
..이후 생략
```

InitButtonBar( ) 함수는 F버튼 인터페이스를 정의한 객체들의 배열을 리턴합니다. 각 객체 항목은 버튼 레이블을 지정한 속성 label과, 버튼 클릭 시 수행될 javascript를 지정한 속성 script로 구성됩니다.

</br>

InitButtonBar( ) 함수의 리턴값

``` js
[
   {
      label: {label of button-F1},
      script: {script to execute on button-F1 clicked}
   },
   {
      label: {label of button-F2},
      script: {script to execute on button-F2 clicked}
   },
   ....
]
```

위 코드에서 지정한 script는 각각 setAllValueAsDef( ) 함수 호출과 setSelectedValueAsDef( ) 함수 호출입니다. 이들은 dst_setup.js가 기본 제공해주는 함수들입니다.

만일, 다른 동작을 원한다면, 직접 함수를 구현하면 됩니다.

</br>
이제 동작을 시험해봅시다. 가상제어기를 다시 실행한 후, ArgosX 설정화면으로 진입합니다. default와는 다른 값들로 설정을 변경한 후 [OK] 버튼으로 저장합니다.

설정 화면으로 재진입 한 후, 임의의 element에 커서를 두고 Initialize One 버튼을 클릭하여 default 값으로 복원되는지 확인합니다.

또한 Initialize All 버튼을 클릭하여 화면 전체의 값이 default 값으로 복원되는지 확인합니다.

![](../../_assets/image_48.png)# 3.4 실전 프로젝트 : ArgosX - 모니터링 panel U/I 개발

분할된 화면을 통해 플러그인의 현재 상태를 사용자에게 보여주려면, 모니터링 panel이 있어야 합니다.

이번 챕터에서는 ArgosX용 web 기반 모니터링 panel의 개발을 실습해봅시다.
<br></br>

* ArgosX의 모니터링 panel 사용자 인터페이스의 사양
* 모니터링 panel의 레이아웃
* 모니터링 panel의 동작
* panel 메뉴의 주입# 3.4.1 ArgosX의 모니터링 panel 사용자 인터페이스의 사양

아래와 같은 정보들을 관측할 수 있는 모니터링 panel 사용자 인터페이스를 만들어 봅시다.

* IP주소
* port번호
* 에러 입력할당 번호
* 요청 횟수
* 응답 횟수

갱신 주기는 500msec으로 합니다.
<br></br>
![](../../_assets/image_49.png)

# 3.4.2 모니터링 panel의 레이아웃

argosx 의 부모인 apps/ 폴더에 대해 vscode를 여십시오.


ui/panel.html 과 panel.js 파일을 생성합니다.

![](../../_assets/image_50.png)


내용은 아래와 같이 작성합니다. <table> 하나로 구성되어 있는 간단한 layout입니다. table의 첫번째 열에는 'thd' 라는 class를 부여했는데(table header의 줄임말), 이 class는 공용 style.css에 회색 바탕의 검은 글씨로 정의되어 있습니다.  스타일을 바꾸고 싶다면, 별도의 local css로 class를 정의하여 적용해도 됩니다.



panel.html
``` html
<!DOCTYPE html:5>
<!--
   @author: Jane Doe, BlueOcean Robot & Automation, Ltd.
   @brief: ArgosX Vision System interface - panel
   @create: 2021-12-07
-->
<html>
  
<head>
   <title>ArgosX Vision System</title>
   <meta http-equiv=Content-Type content='text/html; charset=utf-8'>
   <link rel='stylesheet' href='../../_common/css/style.css' type=text/css rel=stylesheet>
   <script src='../../_common/js/jquery-3.6.0.min.js'></script>
   <script src='./panel.js'></script>
   <script>
      $(document).ready(init);
   </script>
</head>
  
<body>
   <table>
      <th>name</th>
      <th>value</th>
      <tr>
         <td class='thd'>IP address</td>
         <td id='ip_addr'></td>
      </tr>
      <tr>
         <td class='thd'>port#</td>
         <td id='port'></td>
      </tr>
      <tr>
         <td class='thd'>sigcode for error</td>
         <td id='sigcode_err'></td>
      </tr>
      <tr>
         <td class='thd'>n.request</td>
         <td id='n_req'></td>
      </tr>
      <tr>
         <td class='thd'>n.response</td>
         <td id='n_res'></td>
      </tr>
   </table>
</body>
</html>
```

panel.html이 열린 상태에서 우하단의 Go Live 버튼을 클릭하여 Live server를 실행하면 Chrome 브라우저가 열립니다.

![](../../_assets/image_51.png)
<br></br>
아직 panel.js의 내용은 없지만, 레이아웃이 정상적인지는 확인할 수 있습니다.
![](../../_assets/image_52.png)




# 3.4.3 모니터링 panel의 동작


python 코드에서 IP주소, port번호, 에러 할당입력 번호의 변수는 이미 존재합니다.

요청 횟수, 응답 횟수를 관리해야 하므로 아래와 같이 n_req, n_res 변수를 추가해주고, get_general( ) 함수에서 함께 리턴되도록 연결해줍니다.



setup.py
``` python 
이전 생략...
 
 
def get_general() -> dict:
   """
   Returns:
      setting dict.
   """
 
   print('get_general()')
    
   ret = {}
   ret["ip_addr"] = ip_addr
   ret["port"] = port
   ret["sigcode_err"] = sigcode_err
   ret["n_req"] = n_req
   ret["n_res"] = n_res
    
   return ret
 
 
생략...
 
 
gen_def = get_general_def()
ip_addr : str = gen_def['ip_addr']
port : int = gen_def['port']
sigcode_err = gen_def['sigcode_err']
n_req = 0   # request count
n_res = 0   # response count
```

로봇언어 명령문의 구현에 n_req와 n_res의 count-up 동작을 추가해줍니다.



roblang.py
```python 
이전 생략...
 
 
def req(work_no: int) -> int:
   """
   send request command to ArgosX
   e.g. "req 39"
   Args:
      work_no     work#    1~100
 
 
   Returns:
         >=0   the number of bytes sent
         -1    no socket. init() should be called.
   """
   msg = "req " + str(work_no)
   setup.n_req += 1                           # <---------- 추가
   return comm.send_msg(msg)
 
 
생략...
 
 
def _res_cont(addr_on_timeout: int_or_str) -> str:
   """res() implementation for cont-mode"""
   val = 0
   msg = ""
   timeout = _check_timeout_and_branch(addr_on_timeout)
   if timeout:
      val = 1
   else:
      msg = comm.recv_msg()
      print(msg)
      if msg=="res fail":
         val = 1
         msg = ""
      elif msg=="":
         xhost.req_to_continue()
      else:
         msg = get_base_shift_array_from_res(msg)
         setup.n_res += 1                    # <---------- 추가
   xhost.io_set_out_bit(setup.sigcode_err, val)
   return msg
```

ui/panel.js 파일에 아래와 같이 작성합니다. 500msec마다 updateData( ) 함수가 호출되어, main보드로부터 general 데이터를 모두 가져온 후, display( ) 함수를 통해 html 화면에 주입하는 동작입니다.



ui/panel.js
``` js

///@author: Jane Doe, BlueOcean Robot & Automation, Ltd.
///@brief: ArgosX Vision System interface - panel
///@create: 2021-12-07
 
 
 
function init()
{
   updateData();
   setInterval('updateData()', 500);
}
 
 
///@brief
function updateData()
{
   var path = '/apps/argosx/svr_general';
   var url = domainMb()+path;
   $.get(url, function(data) {
      display(data);
   });
}
 
 
///@return     e.g. "http://192.168.1.150:8888"
function domainMb()
{
   var domain = "http://" + window.location.hostname + ":8888";
   console.log(domain);
   return domain;
}
 
 
///@param[in]  data
///@brief      (callback function) form <- data
function display(data)
{
   console.log(data);
 
   $('#ip_addr').text(data.ip_addr);
   $('#port').text(data.port);
   $('#sigcode_err').text(data.sigcode_err);
   $('#n_req').text(data.n_req);
   $('#n_res').text(data.n_res);
}
```

가상 제어기를 재실행한 후, 다시 VS Code의 Go Live를 실행합니다. 이제 아래와 같이 웹 브라우저 화면에 값이 출력되는 것을 볼 수 있습니다.

ArgosX stub를 실행하고 가상 티치펜던트로 job 프로그램을 실행해, 요청과 응답을 수행해봅시다. n.request와 n.response 값이 증가하면 정상입니다.
<br></br>
![](../../_assets/image_53.png)





# 3.4.4 panel 메뉴의 주입

ArgosX의 모니터링 기능을, panel 메뉴에 주입해봅시다.

ui/menu.json 파일에 아래와 같이 panel 항목을 추가합니다.

menu.json
``` json
[
   {
       "path": "system/appl/",
       "id": "argosx",
       "icon": "argosx/ui/lm_argosx.png",
       "label": "ArgosX Vision",
       "url": "argosx/ui/setup.html"
   },
   {
      "path": "panels",
      "id": "argosx",
      "icon": "argosx/ui/panel_argosx.png",
      "label": "ArgosX Vision",
      "url": "argosx/ui/panel.html"
  }
]
```

panel 메뉴에 주입할 그림 icon을 제작해봅시다. 40x40 투명배경의 png icon 형태이면 됩니다.

기존 lm_argosx.png를 적당히 축소한 후 색상을 조정하여 panel_argosx.png를 만들었습니다.


![](../../_assets/panel_argosx.png)
panel_argosx.png의 예 (이 그림을 다운받아 사용해도 됩니다.)



이제 가상 메인보드과 가상 티치펜던트를 재실행 합니다.


새로운 panel을 추가하기 위해 panel 메뉴를 열면, 맨 아래에 새로 추가된 ArgosX Vision 메뉴 항목이 보입니다.
![](../../_assets/image_54.png)




메뉴를 선택하면 잠시 후, 우리가 작성한 모니터링 panel이 나타납니다.
![](../../_assets/image_55.png)




가상 티치펜던트를 조작해, 요청과 응답을 수행해봅시다. n.request와 n.response 값이 증가하면 정상입니다.

# 3.5 실전 프로젝트 : ArgosX - user-bar U/I 개발

이전 챕터에서 우리는 로봇언어를 통해 플러그인에서 구현한 기능을 수행하는 방법을 배웠습니다.

그런데, 경우에 따라서는 사용자가 직접 U/I를 조작하여 플러그인의 기능을 호출하는 기능을 제공해야 할 수도 있습니다. 설정창이나 모니터링창에 이러한 조작버튼을 제공할 수도 있지만, 교시화면에서 빠르게 조작하려면 user-bar (사용자 막대) 형태가 적합할 것입니다.
<br></br>
이번 챕터에서는  ArgosX 용 web 기반 user-bar U/I의 개발을 실습해봅시다.

* ArgosX의 user-bar 사용자 인터페이스의 사양
* 사용자 막대의 레이아웃
* 사용자 막대의 동작
* 사용자 막대의 주입
# 3.5.1 ArgosX의 user-bar 사용자 인터페이스의 사양

사용자키 버튼을 여러 번 누르면 사용자 막대가 전환됩니다.

아래와 같은 U/I를 제공하는 사용자 막대 U/I를 만들어 봅시다.

light-on 버튼 : ArgosX의 LED 조명을 켠다.
light-off 버튼 : ArgosX의 LED 조명을 끈다.
<br>
<br>
![](../../_assets/image_56.png)# 3.5.2 사용자 막대의 레이아웃

argosx 의 부모인 apps/ 폴더에 대해 vscode를 여십시오.

ui/ubar.html 와 ui/ubar.js 파일을 생성합니다.
<br>![](../../_assets/image_57.png)

내용은 아래와 같이 작성합니다. 2개의 button으로 구성된 간단한 layout입니다. table의 첫번째 열에는 'ubar-bt' 이라는 class를 부여했는데, 이 class는 공용 style.css에 정의되어 있습니다. TP600과 TP630을 자동으로 인식하여 기본 U/I와 유사한 버튼 크기와 색상을 부여해줍니다. 스타일을 바꾸고 싶다면, 별도의 local css로 class를 정의하여 적용해도 됩니다.

ubar.html
``` html
<!DOCTYPE html:5>
<!--
   @author: Jane Doe, BlueOcean Robot & Automation, Ltd.
   @brief: ArgosX Vision System interface - bar
   @create: 2021-12-07
-->
<html>
  
<head>
    <title>ArgosX</title>
    <link rel='stylesheet' href='../../_common/css/style.css' type=text/css rel=stylesheet>
    <script src='../../_common/js/jquery-3.6.0.min.js'></script>
    <script src='./ubar.js'></script>
</head>
  
<body class='ubar'>
   <button id='light-on' class='ubar-bt' onclick='light_onoff(true);'>light<br>on</button>
   <button id='light-off' class='ubar-bt' onclick='light_onoff(false);'>light<br>off</button>
</body>
</html>
```

ubar.html이 열린 상태에서 우하단의 Go Live 버튼을 클릭하여 Live server를 실행하면 Chrome 브라우저가 열립니다.
<br>![](../../_assets/image_58.png)




아직 ubar.js의 내용은 없지만, 레이아웃이 정상적인지는 확인할 수 있습니다.
<br>![](../../_assets/image_59.png)# 3.5.3 사용자 막대의 동작

## 클라이언트단 (티치펜던트)


ui/ubar.js 파일에 아래와 같이 작성합니다. 버튼을 누를 때마다 티치펜던트는 "light_onoff"라는 HTTP post 메시지를 메인보드로 송신합니다. on인지 off인지는 onoff라는 속성을 가진 객체에 담아, post 메시지의 body에 실어 보냅니다.

light-on일 때의 body : { onoff: true }
light-off일 때의 body : { onoff: false }


ui/ubar.js
``` js
///@author: Jane Doe, BlueOcean Robot & Automation, Ltd.
///@brief: ArgosX Vision System interface - setup general
///@create: 2021-12-07
 
 
 
///@return     e.g. "http://192.168.1.150:8888"
function domainMb()
{
   var domain = "http://" + window.location.hostname + ":8888";
   console.log(domain);
   return domain;
}
 
 
///@param[in]  onoff    true or false
///@return
///      -  0     ok
///@brief      request light-on/off
function light_onoff(onoff)
{
   var url = domainMb()+"/apps/argosx/svr_light_onoff";
   var args = { onoff: onoff };
 
   args = JSON.stringify(args);
   $.ajax({
      url: url,
      type: 'post',
      dataType: 'json',
      contentType : "application/json; charset=utf-8",
      data: args,
      success: function(res) {
         console.log('success' + res);
      },
      error : function(res) {
         console.log('error' + res);
      }
   });
 
   return 0;
}
```



## 서버단 (메인보드)


이제 메인보드의 argosx 플러그인에서 이 메시지를 받아 실제 ArgosX 장치(stub로 실험)로 "light-on", "light-off" 메시지를 보내주면 됩니다.



argosx/ 폴더에 ubar.py 파일을 아래와 같이 작성합니다. 구현 내용은 이전 챕터의 callback에서 구현한 것과 비슷하게 comm_ex 모듈을 활용합니다.
``` python
""" ArgosX Vision System interface - main
 
 
@author:    Jane Doe, BlueOcean Robot & Automation, Ltd.
@created:   2021-12-07
"""
 
 
from . import comm_ex
 
 
def post_light_onoff(body: dict) -> int:
   """
   Args:
      onoff
    
   Returns:
      0
   """
   onoff = body['onoff']
   msg = 'light-' + ('on' if onoff else 'off')
   print(msg)
       
   return comm_ex.send_msg_once(msg)
```

마지막으로, main.py에 ubar.py의 모든 함수를 import 해줍니다.



main.py
``` python
""" ArgosX Vision System interface - main
 
 
@author:    Jane Doe, BlueOcean Robot & Automation, Ltd.
@created:   2021-12-06
"""
 
from . import setup
from .roblang import *
from .setup import *
from .callback import *
from .ubar import *
 
import xhost
 
 
이후 생략...
```

가상제어기를 재부팅한 후, job 파일을 argosx.init( )까지 실행합니다. 다시 Go Live를 실행하여 웹브라우저에 사용자 막대 화면을 띄웁니다.

ArgosX stub를 실행하고 웹 브라우저의 버튼을 조작해봅니다. stub의 콘솔 출력에 "light-on", "light-off" 수신이 표시되면 정상입니다.
<br>
![](../../_assets/image_60.png)




ArgosX stub 콘솔 출력
<br>
![](../../_assets/image_61.png)






# 3.5.4 사용자 막대의 주입

ArgosX의 사용자 막대를, 실제 티치펜던트에 주입해봅시다.



ui/menu.json 파일에 아래와 같이 "ubars" path의 항목을 추가합니다.



menu.json
``` json
[
    {
         "path": "system/appl/",
         "id": "argosx",
         "icon": "argosx/ui/lm_argosx.png",
         "label": "ArgosX Vision",
         "url": "argosx/ui/setup.html"
    },
    {
        "path": "panels",
        "id": "argosx",
        "icon": "argosx/ui/panel_argosx.png",
        "label": "ArgosX Vision",
        "url": "argosx/ui/panel.html"
    },
    {
        "path": "ubars",
        "id": "argosx",
        "url": "argosx/ui/ubar.html"
    }
]
```

가상 티치펜던트에서 우측의 사용자키 버튼을 누르면 설치된 응용의 사용자 막대들이 차례로 나타납니다. 이제 우리가 만든 ArgosX용 사용자 막대도 볼 수 있습니다. 버튼을 조작해 동작이 ArgosX stub가 정상적으로 반응하는지 확인하십시오.

![](../../_assets/image_62.png)


# 4. 디버깅

* python 코드의 디버깅
* web 기반 U/I의 디버깅
# 4.1 python 코드의 디버깅

우리가 작성한 python 코드에 web U/I에 문법오류 혹은 구현체의 논리적인 오류가 있으면, 디버거를 동원해 원인을 추적하여 보완해야 합니다.

그런데, 플러그인의 python 소스코드는 Hi6 호스트로부터 import와 callback, 로봇언어를 통해 호출됩니다. Hi6 호스트에는 python 디버거가 내장되어 있지 않기 때문에, 호스트로부터의 호출에 breakpoint를 걸어 trace하는 것은 불가능합니다. 하지만, 플러그인의 entry 함수를 외부 디버거로 실행하는 방식으로 부분적인 디버깅을 할 수 있습니다.


<br>

## 디버깅용 xhost
xhost는 플러그인에서 Hi6 호스트의 기능을 호출할 때 사용되는 모듈로서, 호스트에 의해 플러그인에 생성/주입됩니다. 아래 그림은 호스트와 플러그인의 일반적인 동작 흐름입니다.
<br> ![](../_assets/image_63.png)




하지만 vscode의 디버거로 플러그인을 실행할 때는 생성/주입된 실제 xhost 모듈이 없으므로 xhost 메소드 호출 시 실행에러가 나게 됩니다. 따라서, 디버깅용 대체 xhost를 사용해야 합니다. SDK의 common 폴더 안에는 대체 모듈인 xhost_dbg가 있으며, 이것은 이더넷을 통한 OpenAPI 호출을 통해 호스트의 동작을 수행해주는 일종의 proxy입니다.
<br> ![](../_assets/image_64.png)
<br>





apps/ 폴더에 있는 아래와 같은 간단한 xhost.py가 xhost_dbg를 import 해주고 있습니다. 실제 Hi6 host가 xhost를 생성/주입할 때는 이 xhost 대체 모듈을 override 해버립니다.



xhost.py
``` python
""" xhost for debug
   This module will be overrided by host.
"""
from _common.py.xhost_dbg import *
```

호스트는 개발환경 PC에서 함께 실행되는 Hi6 가상제어기일 수도 있고 실제제어기일 수도 있습니다. xhost_dbg가 어디에 접속해야 하는지를 지정해줘야 합니다.

Hi6 home 경로의 apps/ 폴더 안에 xhost_remote_ip.py 파일이 있습니다. 기본값은 아래와 같아서 개발환경 PC 자신 (즉, Hi6 가상제어기)으로 설정되어 있습니다.



xhost_remote_ip.py
``` python
remote_ip="127.0.0.1"
```

만일 IP주소가 "192.168.1.150"인 실제 Hi6 제어기에 연결하여 시험하고자 한다면 아래와 같이 설정하면 됩니다.



xhost_remote_ip.py
``` python
remote_ip="192.168.1.150"
```



## Visual Studio Code와 test.py에 의한 디버깅
<u>VisualStudio Code의 설치</u>에d서 microsoft Python 확장을 제대로 설치했다면,  python 디버깅 환경을 사용할 수 있습니다. vscode에서의 python 디버깅에 이미 익숙하다면 이 절은 건너뛰어도 됩니다.



로봇언어나 callback에 의해서 호출되는 함수들을 trace하기 위해, argosx/ 폴더 밑에 아래 그림과 같이  test 모듈을 정의합니다. 시험하고자 하는 루틴을 구현하고 main 루틴에서 test( ) 함수를 호출합니다. 

line 번호의 좌측을 마우스 클릭하여 breakpoint를 토글할 수 있습니다.



test.py
<br>
![](../_assets/image_65.png)



test.py 창에서 F5키를 누르거나 Run - Start Debugging 메뉴를 클릭하면, python runtime으로 실행되면서 디버깅 모드로 실행됩니다.
<br>
![](../_assets/image_66.png)





이전 절의 argosx 플러그인의 경우, 시험을 위해서는 argosx_stub 서버도 실행해야 합니다. 이렇게 2개의 python 프로그램을 실행해야 하는 경우에는 아래와 같이 Visual Studio Code를 2개를 열어 각각 디버거를 실행하면 됩니다.
<br>
![](../_assets/image_67.png)





breakpoint에 걸리면 좌측에 VARIABLES, WATCH, CALL STACK, BREAKPOINTS 창이 열립니다. 우상단의 조작 버튼 Continue (F5), Step Over (F10), Step Into (F11), Step Out (Shift+F11)으로 trace 할 수 있습니다. Restart (Ctrl+Shift+F5) 버튼을 클릭하면 처음부터 재실행하며, Stop (Shift+F5) 버튼을 클릭하면 디버깅이 중단됩니다.

python 내장함수인 print( )문을 호출하면, 하단의 TERMINAL 창에 문자열이 출력되므로, 디버깅에 활용할 수 있습니다.
<br>
![](../_assets/image_68.png)





test.py의 동작은 xhost_dbg에 의해 제어기 호스트와 연동되기 때문에 아래와 같이 실제 동작을 확인하면서 디버깅할 수 있습니다. 
<br>
![](../_assets/image_69.png)




# 4.2 web 기반 U/I의 디버깅


티치펜던트에서 web UI를 시험하기 전, 우리는 Chrome 웹 브라우저로 화면을 미리 시험해볼 수 있었습니다.

우리가 작성한 web U/I에 javascript 문법오류 혹은 구현체의 논리적인 오류가 있으면, 디버거를 동원해 원인을 추적하여 보완해야 합니다. 그런데 Chrome 웹 브라우저는 Chrome Development Tools (줄여서 Chrome DevTools)라는 디버거를 자체 내장하고 있으므로 이를 활용하면 됩니다. 이번 장에서는 Web U/I의 디버깅 방법에 대해서 알아보겠습니다. 이미 Chrome DevTools 사용에 익숙하다면 이 장은 건너뛰어도 됩니다.





## Live server 로 web UI 실행


<u>설정화면의 레이아웃</u>d 절에서 argosx 플러그인의 setup.html을 Chrome 웹 브라우저로 실행해본 바 있습니다.

다시한번 실행해봅시다.



가상제어기의 argosx가 정상적으로 import된 상태여야 합니다.

vscode에 setup.html이 열린 상태에서 우하단의 Go Live 버튼을 클릭하여 Live server를 실행합니다.

![](../_assets/image_70.png)




혹은, setup.html에 대해 우버튼으로 팝업 메뉴를 열고 "Open with Live Server"를 선택합니다.

![](../_assets/image_71.png)



아래와 같이 Chrome 웹 브라우저로 setup.html이 열렸습니다.

![](../_assets/image_72.png)






## Chrome DevTools


F12 버튼을 누르면 브라우저 우측에 Chrome DevTools가 열립니다. 아래 그림에서는 DevTools의 상단 메뉴 Console이 선택되어 있습니다. javascript에서 console.log( )로 문자열을 호출하면, 이 console 창에 문자열이 출력됩니다. 이 때, log( )가 호출된 소스코드 위치도 함께 표시되고, 클릭하여 해당 소스코드 위치로 이동할 수 있기 때문에, 디버깅 용도로 유용합니다.

![](../_assets/image_73.png)



DevTools의 상단 메뉴에서 Elements를 선택하면 html 파일의 계층 구조를 볼 수 있습니다. html 파일의 특정한 element를 선택하면, 좌측 렌더링 화면에서 해당 element가 강조되며, 우하단에는 CSS style이 표시됩니다.


![](../_assets/image_74.png)



Sources 메뉴를 선택하면, javascript 소스코드 파일들이 tree구조로 나타납니다. 원하는 파일을 선택해 열고 소스코드를 확인할 수 있으며, 좌측의 line번호를 클릭하여 breakpoint를 토글할 수 있습니다.

![](../_assets/image_75.png)



좌측의 html 렌더링 화면의 update 버튼을 클릭하거나 F5 키를 누르면, web UI가 갱신되면서 javascript가 처음부터 재실행됩니다. breakpoint에서 실행커서가 멈춘 것을 볼 수 있습니다. 그림 하단을 보면 Breakpoints와 Call Stack 창이 보이는데 항목을 클릭하여 해당하는 소스코드 위치로 이동할 수 있습니다.

우 하단의 Scope에는 Local 변수들이 나타나며, Scope 우측의 Watch 메뉴를 선택하면 원하는 변수를 추가하여 값을 관찰할 수 있습니다.

VisualStudio나 Eclipse 같은 통합개발환경의 사용자라면, 매우 익숙한 디버깅 환경입니다.



![](../_assets/image_76.png)

![](../_assets/image_77.png)




breakpoint에 걸린 상태에서는 trace가 가능합니다. Breakpoints 창 위의 조작 버튼 그룹으로 Resume(F8), Step over(F10), Step into(F11), Step out(Shift+F11)을 할 수 있습니다.


![](../_assets/image_78.png)


## 소스코드의 수정과 재실행


html이나 css, javascript 소스코드를 수정한 후, 웹 브라우저에서 update 버튼을 클릭하거나 F5 키를 누르면, 수정된 내용으로 재실행됩니다.

그런데 웹 브라우저에는 cache가 있기 때문에, 소스코드를 수정하고 재실행 해도 변경사항이 반영 안 되는 경우가 간혹 있습니다. 이 때는 update 버튼에 마우스 우버튼을 클릭해 팝업 메뉴를 열고 '캐시 비우기 및 강력 새로고침'을 선택하면 해결됩니다. (이 팝업 메뉴는 DevTools가 열려 있을 때만 사용 가능합니다.)


![](../_assets/image_79.png)


웹 브라우저가 아닌 가상 티치펜던트에서도 새로고침을 할 수 있습니다. 웹 U/I 화면에 마우스 우버튼을 클릭하면 팝업 창이 열립니다. Reload 메뉴를 선택하면, 소스코드의 수정 내용이 즉각 반영됩니다.


![](../_assets/image_80.png)




# 5. 인스톨러
