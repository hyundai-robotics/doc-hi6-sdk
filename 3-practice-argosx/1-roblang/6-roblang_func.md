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
```