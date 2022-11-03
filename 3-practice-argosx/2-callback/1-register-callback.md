# 3.2.1 callback 함수 등록


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
