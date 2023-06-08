# 3.1.2 ArgosX stub

The ArgosX vision system is not real. Therefore, if we want to test the interface plug-ins, we need the test software, namely the stub, to take ArgosX's place.

The Python code below is an ArgosX stub. You do not need to understand the details of its implementation.

argosx_stub.py
``` python
"""ArgosX stub
Test double for ArgosX interface plug-in
 
 
@author:    Jane Doe, BlueOcean Robot & Automation, Ltd.
@created:   2021-12-06
@version:   v1.0
"""
 
import time
from typing import Optional, Union, Dict
import socket
 
 
# const
buf_size = 0x8000    # 32kb ; permitted packet length
port_no = 54321      # port for ArgosX command
sleep_sec = 0        # delay before response
 
# global variables
inaddr_any : str = ""
ip_port_of_req = ("", 0)
sock : Optional[socket.socket] = None
 
# test samples
test_shifts : Dict[str, str]= {
   "5" : "(30, 25.7, 11.9, 31.6, 12.8, -54.6)",
   "39" : "(9, 15.5, 10.3, 11.2, 19.2, 1.3)",
   "98" : "fail",
   "else" : "(0, 0, 0, 0, 0, 0)"
}
 
 
# functions
def init():
   """
   init server
   """
   global sock
   try:
      sock = socket.socket(family=socket.AF_INET, type=socket.SOCK_DGRAM)
      sock.bind((inaddr_any, port_no))
   except socket.error as e:
      print("socket creation or binding error :", e)
      return -1
   return 0
 
 
def close():
   """
   close server
   """
   if sock is None: return
   sock.close()
 
 
def do_service():
   """
   do service loop
   """
   state = 0
    
   while(True):
      msg = recv_msg()
      msg = msg.strip('\x00')
      ret = do_service_sub(msg)
      if ret==1:
         break
 
 
def recv_msg() -> str:
   """
   receive UDP message (blocking)
   IP address and port of sender is stored in ip_port_of_req
   Returns:
         received message     e.g. "req 39"
   """
   global ip_port_of_req
   if sock is None: return ""
   data, ip_port_of_req = sock.recvfrom(buf_size)
   bts = bytearray(data)
   msg = bts.decode()
   return msg
 
 
def do_service_sub(msg: str) -> int:
   """
   do service subroutine
   Args:
         msg   e.g. "req 39"
   Returns:
         1     quit the service
         0     continue the service
         -1    invalid command  
   """
   print('')
   print('request : ', msg)
   strs = msg.split()
       
   n_str = len(strs)
   if n_str < 1:
      return -1
    
   cmd = strs[0]
   param = ""
   if n_str >= 2:
      param = strs[1]
    
   if cmd=="req":
      time.sleep(sleep_sec)
      do_service_req(param)
   elif cmd=="light-on":
      print('LED light is ON')
   elif cmd=="light-off":
      print('LED light is OFF')
   elif cmd=="quit":
      return 1
   else:
      print('invalid command')
      return -1
   return 0
 
 
def do_service_req(param: str) -> int:
   """
   do service for req
   Args:
      param    work#    "1"~"100"
 
   Returns:
         -1    no socket
         >=0   the number of bytes sent
   """
   if sock is None: return -1
   res_value = ""
   try:
      res_value = test_shifts[param]
   except:
      res_value = test_shifts["else"]
   msg = "res " + res_value
   print('response: ', msg)
   bts = bytearray(str.encode(msg))
   return sock.sendto(bts, ip_port_of_req)
    
 
# -----------------------------------------------
# main
print('***** ArgosX stub v1.0 *****')
iret = init()
if iret < 0:
   quit()
print('inaddr_any, port_no=%d' % port_no)
print('server started...')
do_service()
print('closing...')
close()
print('...server ended')
```

Copy the content above and create an argosx_stub.py file under the hello_world/ folder. Next, go to the hello_world/ folder using Windows PowerShell or Command Prompt, then execute it using the command below.

```
python argosx_stub.py
```

Alternatively, if you open vscode and press F5, the execution will be performed in debug mode.

- Rather than opening the argosx/ project in the opened vscode, you need to open the project by executing another vscode session.
- While the debug configuration list may open initially, as shown below, you only need to select the Python File item.

![](../../_assets/image_24.png)

The result will be printed on the TERMINAL window at the bottom. You can hold, resume, or stop debugging by operating the ![](../../_assets/image_25.png) button on the top right.

![](../../_assets/image_26.png)