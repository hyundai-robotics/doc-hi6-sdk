#### 3.1.2 ArgosX stub

ArgosX视觉系统并不是真实的。因此，如果我们想测试接口插件，我们需要测试软件，即stub，以替代ArgosX。

下面的Python代码是一个ArgosX stub。您无需理解其实现的细节。

argosx_stub.py
``` python
"""ArgosX stub
ArgosX接口插件的测试替身
 
 
@author:    Jane Doe, BlueOcean Robot & Automation, Ltd.
@created:   2021-12-06
@version:   v1.0
"""
 
import time
from typing import Optional, Union, Dict
import socket
 
 
# 常量
buf_size = 0x8000    # 32kb ; 允许的数据包长度
port_no = 54321      # ArgosX命令的端口
sleep_sec = 0        # 响应前的延迟
 
# 全局变量
inaddr_any : str = ""
ip_port_of_req = ("", 0)
sock : Optional[socket.socket] = None
 
# 测试样本
test_shifts : Dict[str, str]= {
   "5" : "(30, 25.7, 11.9, 31.6, 12.8, -54.6)",
   "39" : "(9, 15.5, 10.3, 11.2, 19.2, 1.3)",
   "98" : "fail",
   "else" : "(0, 0, 0, 0, 0, 0)"
}
 
 
# 函数
def init():
   """
   初始化服务器
   """
   global sock
   try:
      sock = socket.socket(family=socket.AF_INET, type=socket.SOCK_DGRAM)
      sock.bind((inaddr_any, port_no))
   except socket.error as e:
      print("套接字创建或绑定错误 :", e)
      return -1
   return 0
 
 
def close():
   """
   关闭服务器
   """
   if sock is None: return
   sock.close()
 
 
def do_service():
   """
   执行服务循环
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
   接收UDP消息（阻塞）
   发送者的IP地址和端口存储在ip_port_of_req中
   返回:
         接收到的消息     例如 "req 39"
   """
   global ip_port_of_req
   if sock is None: return ""
   data, ip_port_of_req = sock.recvfrom(buf_size)
   bts = bytearray(data)
   msg = bts.decode()
   return msg
 
 
def do_service_sub(msg: str) -> int:
   """
   执行服务子例程
   参数:
         msg   例如 "req 39"
   返回:
         1     退出服务
         0     继续服务
         -1    无效命令  
   """
   print('')
   print('请求 : ', msg)
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
      print('LED灯已开启')
   elif cmd=="light-off":
      print('LED灯已关闭')
   elif cmd=="quit":
      return 1
   else:
      print('无效命令')
      return -1
   return 0
 
 
def do_service_req(param: str) -> int:
   """
   执行请求服务
   参数:
      param    工作#    "1"~"100"
 
   返回:
         -1    无套接字
         >=0   发送的字节数
   """
   if sock is None: return -1
   res_value = ""
   try:
      res_value = test_shifts[param]
   except:
      res_value = test_shifts["else"]
   msg = "res " + res_value
   print('响应: ', msg)
   bts = bytearray(str.encode(msg))
   return sock.sendto(bts, ip_port_of_req)
    
 
# -----------------------------------------------
# 主程序
print('***** ArgosX stub v1.0 *****')
iret = init()
if iret < 0:
   quit()
print('inaddr_any, port_no=%d' % port_no)
print('服务器已启动...')
do_service()
print('正在关闭...')
close()
print('...服务器已结束')
将上面的内容复制并在 hello_world/ 文件夹下创建一个 argosx_stub.py 文件。接下来，使用 Windows PowerShell 或命令提示符转到 hello_world/ 文件夹，然后使用以下命令执行它。

```
python argosx_stub.py
```

或者，如果您打开 vscode 并按 F5，将在调试模式下执行。

- 请不要在打开的 vscode 中打开 argosx/ 项目，而是通过执行另一个 vscode 会话来打开该项目。
- 尽管调试配置列表可能会首先打开，如下所示，您只需要选择 Python 文件项。

![](../../_assets/image_24.png)

结果将在底部的 TERMINAL 窗口中打印。您可以通过操作右上角的 ![](../../_assets/image_25.png) 按钮来保持、继续或停止调试。

![](../../_assets/image_26.png)