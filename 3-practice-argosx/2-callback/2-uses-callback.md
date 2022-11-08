# 3.2.2 callback 함수 구현
callback 함수가 잘 호출되는 것을 확인했으니, 이제 실제 동작을 구현해봅시다.

<U>ArgosX와 interface plug-in의 사양</U>을 확인해보면, ArgosX 하드웨어측으로 "light-on"과 "light-off" 메시지를 송신하기만 하면 됩니다.



comm 모듈에 이미 이더넷 문자열 송신 기능이 구현되어 있으므로, 아래와 같이 간단하게 호출만 하면 됩니다.
```
comm.send_msg("light-on")
comm.send_msg("light-off")
```
<br></br>

그런데 이 구현은 한가지 문제가 있습니다. 로봇언어 명령문 argosx.init를 통해 comm.open( ) 함수가 호출된 상태라면 정상적으로 문자열이 송신되지만, 호출되지 않은 상황에서는 송신이 안됩니다.

또한, comm.close( )로 통신이 닫혀버린 상황에서도 송신이 안됩니다. 

따라서, 통신이 닫혀있으면 열고 송신한 후 다시 닫아주는 동작까지 해주는 문자열 송신 함수를 정의해야 합니다.

<br></br>
comm.py와 같은 폴더에 comm_ex.py 파일을 아래와 같이 작성합니다.
<br></br>


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


<br></br>
<U>__argosx_stub측 (ArgosX 역할의 server)__</U>

motor OFF, motor ON을 할 때마다, 콘솔 출력에 아래와 같은 문자열이 출력됩니다.
```
request : light-off
LED light is OFF

request : light-on
LED light is ON
```
