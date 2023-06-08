# 3.1.6 Implementing functions for the ArgosX robot language

Now let's implement the actual operations of each function.

The operation for UDP client communications may be used in other sections later. Here, we will modularize the operation into a separate .py file.

Add a comm.py file to the project and write the following operations, as shown below.

They are simple operations, such as initializing a UDP socket, sending, receiving, and closing a string message.

- While xhost is a module that calls the functions of the host (robot controller), the main software makes it dynamic (there is no file called xhost.py.) Subsequent sections will provide detailed explanations, so this is all you need to understand for now.


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

By importing the comm module, you can simply implement individual functions that are to be called in the robot language.

You also need to import the setup module because referring to the ip_addr and port values is necessary.

get_base_shift_array_from_res() is a function that converts the shift string received from ArgosX into a format that will be interpreted as the shift( ) function of HRScript.


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

The job should be corrected as follows.


```
Hyundai Robot Job File; { version: 1.6, mech_type: "780(YL012-0D)", total_axis: 6, aux_axis: 0 }
     var iret
     import argosx
      
     print argosx.ip_addr
     print argosx.port
     argosx.ip_addr="192.168.1.172" # your own PC's name
     print argosx.ip_addr # re-checking
      
     iret=argosx.init() # initializing the socket
     if iret<0
       print "init error"
       stop
     endif
      
     iret=argosx.req(39) # transmitting the request
     if iret<0
       print "req error"
       stop
     endif
      
     var str=argosx.res() # waiting for a response
     print str
     var sft=Shift(str) # converting the shift array string into shift data
     print sft.x, sft.y, sft.z, sft.rx, sft.ry, sft.rz
 
     argosx.close() # closing the socket
     end
```
<br></br>
First, execute the argosx_stub from the Command Prompt or vscode.

Reboot the virtual controller and execute the job file. If the file was created normally, the following operation will occur.



<U>__argosx_stub side (a server playing the role of ArgosX)__ </U>

Every time argosx.req( ) is executed, the following string will be printed on the console.
```
request : req 39
response: res (9, 15.5, 10.3, 11.2, 19.2, 1.3)
```

<U>__argosx interface plug-in side (client)__</U>

Every time the last print command is executed, the guidance frame of the teach pendant will print the following.
```
9.000000 15.500000 10.300000 11.200000, 19.200000 1.300000
```