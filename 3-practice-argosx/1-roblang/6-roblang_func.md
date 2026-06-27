#### 3.1.6 为 ArgosX 机器人语言实现功能

现在让我们实现每个功能的实际操作。

UDP 客户端通信的操作可能会在后面的部分使用。在这里，我们将操作模块化为一个单独的 .py 文件。

向项目中添加一个 comm.py 文件，并按照如下所示编写以下操作。

这些是简单的操作，例如初始化 UDP 套接字、发送、接收和关闭字符串消息。

- 虽然 xhost 是调用主机（机器人控制器）功能的模块，但主软件使其动态（没有名为 xhost.py 的文件。）后续部分将提供详细解释，因此现在您只需了解这些即可。

comm.py
``` python 
""" ArgosX 视觉系统接口 - comm.
 
 
@author:    Jane Doe, BlueOcean Robot & Automation, Ltd.
@created:   2021-12-10
"""
 
from typing import Optional
import socket
import xhost
 
 
# 全局变量
raddr : tuple
sock : Optional[socket.socket] = None
buf_size = 0x8000    # 32kb ; 允许的包长度
 
 
def is_open() -> bool:
   """
   返回:
         True     套接字已打开
         False    套接字未打开
   """
   return (sock is not None)
 
 
def open(ip_addr: str, port: int) -> int:
   """
   打开用于 UDP 通信的套接字
   参数:
      ip_addr     远程的 IP 地址，例如 "192.168.1.172"
      port        远程的端口号。例如 "192.168.1.172"
 
   返回:
         0     正常
         -1    错误
   """
   global raddr, sock
   if sock is not None: return -1
   try:
      raddr = (ip_addr, port)
      sock = socket.socket(family=socket.AF_INET, type=socket.SOCK_DGRAM)
   except socket.error as e:
      print("套接字创建或绑定错误 :", e)
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
   通过 sock, raddr 发送消息
   参数:
      msg
 
   返回:
         >=0   发送的字节数
         -1    没有套接字。应该调用 init()。
   """
   if sock is None: return -1
 
   logd('request : ' + msg)
   bts = bytearray(str.encode(msg))
   return sock.sendto(bts, raddr)
 
 
def recv_msg():
   """
   等待从 sock 接收消息
   返回:
      接收到的字符串
   """
   if sock is None: return ""
 
   try:
      data, ip_port = sock.recvfrom(buf_size)
      bts = bytearray(data)
      msg = bts.decode()
      logd('response: ' + msg)
      return msg
   except Exception as e:
      print('来自 recv_msg() 的异常: ' + str(e))
      return ""
 
 
def logd(text: str):
   print(text)
   xhost.printh(text)
```

通过导入 comm 模块，您可以简单地实现将在机器人语言中调用的各个功能。

您还需要导入 setup 模块，因为引用 ip_addr 和 port 值是必要的。

get_base_shift_array_from_res() 是一个将从 ArgosX 接收的偏移字符串转换为将被解释为 HRScript 的 shift() 函数的格式的函数。

roblang.py
``` python 
""" ArgosX 视觉系统接口 - 机器人语言
 
 
@author:    Jane Doe, BlueOcean Robot & Automation, Ltd.
@created:   2021-12-06
"""
 
from . import comm
from . import setup
 
 
# 函数
def init() -> int:
   """
   初始化用于 UDP 通信的套接字
 
   返回:
         0     正常
         -1    错误
   """
   return comm.open(setup.ip_addr, setup.port)
 
 
def close():
   """
   关闭套接字
   """
   comm.close()
 
 
def req(work_no: int) -> int:
   """
   向 ArgosX 发送请求命令
   例如 "req 39"
   参数:
      work_no     工作编号    1~100
 
   返回:
         >=0   发送的字节数
         -1    没有套接字。应该调用 init()。
   """
   msg = "req " + str(work_no)
   return comm.send_msg(msg)
 
 
def res() -> str:
   """
   等待 ArgosX 的响应
   返回:
      来自 ArgosX 的响应字符串。
      失败时返回 ""。
      例如 "[30, 25.7, 11.9, 31.6, 12.8, -54.6]"
   """
   msg = comm.recv_msg()
   print(msg)
   msg = get_base_shift_array_from_res(msg)
   return msg
 
 
def get_base_shift_array_from_res(msg: str):
   """
   从响应字符串获取基本偏移数组符号字符串
   参数:
      msg   例如 "res (30, 25.7, 11.9, 31.6, 12.8, -54.6)"
    
   返回:
      例如 '[30, 25.7, 11.9, 31.6, 12.8, -54.6, "base"]'
   """
   tmp = msg.strip('res ')
   tmp = tmp.replace('(', '[')
   tmp = tmp.replace(')', ', "base"]')
   return tmp
```

作业应按如下方式修正。


```
Hyundai 机器人作业文件; { version: 1.6, mech_type: "780(YL012-0D)", total_axis: 6, aux_axis: 0 }
     var iret
     import argosx
      
     print argosx.ip_addr
     print argosx.port
     argosx.ip_addr="192.168.1.172" # 您自己的 PC 名称
     print argosx.ip_addr # 重新检查
      
     iret=argosx.init() # 初始化套接字
     if iret<0
       print "初始化错误"
       stop
     endif
      
     iret=argosx.req(39) # 传输请求
     if iret<0
       print "请求错误"
       stop
     endif
      
     var str=argosx.res() # 等待响应
     print str
     var sft=Shift(str) # 将偏移数组字符串转换为偏移数据
     print sft.x, sft.y, sft.z, sft.rx, sft.ry, sft.rz
 
     argosx.close() # 关闭套接字
     end
```
<br></br>
首先，从命令提示符或 vscode 执行 argosx_stub。

重新启动虚拟控制器并执行作业文件。如果文件正常创建，将发生以下操作。



<U>__argosx_stub 端（充当 ArgosX 的服务器）__ </U>

每次执行 argosx.req( ) 时，控制台上将打印以下字符串。
```
request : req 39
response: res (9, 15.5, 10.3, 11.2, 19.2, 1.3)
```

<U>__argosx 接口插件端（客户端）__</U>

每次最后一个打印命令执行时，教导挂件的指导框将打印以下内容。
```
9.000000 15.500000 10.300000 11.200000, 19.200000 1.300000
```