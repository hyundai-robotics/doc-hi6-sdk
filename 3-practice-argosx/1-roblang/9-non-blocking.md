# 3.1.9 로봇언어 함수 blocking 문제 해결
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
<br></br>
![](../../_assets/image_29.png)

어떤 사양이 바람직할까요? 일반적으로, wait 문과 같이 어떤 상태를 기다리는 명령문의 경우 STEP FWD키를 누르면 대기하고 떼면 커서를 이동할 수 있습니다. 또한 timeout과 퇴피주소를 인수로 지정할 수 있어서, timeout이 되면 퇴피주소로 분기하는 예외 처리 동작이 가능합니다.
<br></br>
![](../../_assets/image_30.png)

di6을 10초 대기. timeout 시, *tout으로 분기.



## 실행모드와 계속모드


아래 순서도를 봅시다. Hi6 호스트(HOST)가 로봇언어 명령문을 호출할 때는 실행모드(execution-mode)와 계속모드(continue-mode)의 2가지 상태가 있습니다. 계속모드라면, 호스트는 해당 명령문을 다시 호출해줍니다.

호스트는 명령문을 일단 실행모드로 호출합니다. 대부분의 명령문들은 자신의 동작을 수행하고 즉각 종료하며, 호스트도 연속모드가 되지 않은 것을 확인하고 해당 명령문에 대한 처리를 완료시킵니다.
<br></br>

![](../../_assets/image_31.png)


그러나 일부 명령문들은 대기 동작을 가지고 있습니다. (IO 입력이나 이더넷 데이터 수신, 일정 시간, 로봇 동작 완료 등 어떤 상태나 사건(event)의 대기를 뜻합니다.) 호스트와 플러그인 간에는 아래의 절차가 수행됩니다.

* 플러그인의 대기 동작 명령문은 xhost.exec_mode( )로 모드를 확인합니다. True이면 실행모드, False이면 연속모드입니다. 실행모드임이 확인되면(Yes), timeout 인수로 전달받은 시간을 로봇언어용 timer에 설정한 후(set_lang_timer), Hi6 호스트에게 다음 번엔 연속모드로 호출해주기를 요청(xhost.req_to_continue)한 후 종료합니다.
* xhost.req_to_continue 실행 후 종료되면 연속모드입니다. Hi6 호스트는 해당 명령문을 다시 호출(call)해줍니다.
* 플러그인의 대기 동작 명령문은 xhost.exec_mode( )로 모드를 확인합니다. 연속모드임이 확인(No)되면 timer를 확인하여 timeout 인 경우 퇴피주소로 분기(branch_to_addr)하고 연속모드 요청 없이 종료합니다.
* timeout이 아니면, 대기조건이 완료되었는지(wait-complete condition?) 확인합니다. 완료이면 연속모드 요청 없이 그대로 종료하고, 미완료이면 Hi6 호스트에게 연속모드 요청(req_to_continue)을 하고 자신의 동작을 수행(execute command)한 후 종료합니다.
* Hi6 호스트는 이번 호출에서 연속모드 요청이 없었을 경우(continue-mode No), 해당 명령문 처리를 완료(complete)시킵니다.
<br></br>

 ![](../../_assets/image_32.png)




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

