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




