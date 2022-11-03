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
