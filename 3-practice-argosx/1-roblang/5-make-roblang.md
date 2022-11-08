# 3.1.5 ArgosX의 로봇언어용 함수 생성


다음 구현할 사양은 init( ), req( ), res( ), close( ) 함수입니다.

(<u>ArgosX와 interface plug-in의 사양</u>의 프로토콜과 로봇언어 function 부분을 참고하십시오.)



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
