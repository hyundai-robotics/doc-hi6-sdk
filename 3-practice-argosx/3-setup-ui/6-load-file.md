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

