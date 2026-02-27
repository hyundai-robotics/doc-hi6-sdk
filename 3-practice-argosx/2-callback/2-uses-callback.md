#### 3.2.2 实现回调函数
因为我们确保了回调函数能够正常调用，接下来让我们实现实际操作。

如通过检查 <U>3.1.1 ArgosX 的规格和接口插件</U> 所示，您只需向 ArgosX 硬件发送“light-on”和“light-off”消息。

因为通信模块已经实现了发送以太网字符串的功能，您只需调用一个，如下所示。
```
comm.send_msg("light-on")
comm.send_msg("light-off")
```
<br></br>

然而，这个实现存在一个问题。如果通过 argosx.init 调用 comm.open( ) 函数，一个机器人语言命令，字符串将正常传输。然而，如果未调用此函数，字符串将不会被传输。

此外，即使关闭通信因为 comm.close( )，也不会发生传输。

因此，有必要定义一个字符串传输函数，使其能够在通信处于关闭状态时打开通信，并执行传输及关闭通信。

<br></br>
在与 comm.py 存在相同文件夹中创建一个 comm_ex.py 文件，如下所示。
<br></br>

comm_ex.py

``` python
""" ArgosX 视觉系统接口 - 主程序
 
 
@author:    Jane Doe, BlueOcean Robot & Automation, Ltd.
@created:   2021-12-07
"""
 
 
from . import setup
from . import comm
 
 
def send_msg_once(msg: str) -> int:
   was_closed = (comm.is_open()==False)
   if was_closed: comm.open(setup.ip_addr, setup.port)
   iret = comm.send_msg(msg)
   if was_closed: comm.close()
   return iret

```

现在，我们可以简单地通过导入 comm_ex 模块来实现回调函数，如下所示。
callback.py

``` python
""" ArgosX 视觉系统接口 - 回调函数
 
 
@author:    Jane Doe, BlueOcean Robot & Automation, Ltd.
@created:   2021-12-07
"""
 
from . import comm_ex
 
 
def on_motor_on() -> int:
   """(回调) 电机开启
   返回: 0
   """
   print('on_motor_on')
   return comm_ex.send_msg_once("light-on")
 
 
def on_motor_off() -> int:
   """(回调) 电机关闭
   返回: 0
   """
   print('on_motor_off')
   return comm_ex.send_msg_once("light-off")
```

首先，从命令提示符或 vscode 执行 argosx_stub。

重启虚拟控制器，然后运行作业文件，直到 argosx.init( )。如果在此状态下执行了如下操作，则意味着正常照明功能的操作已被检查。


<br></br>
<U>__argosx_stub 侧 (充当 ArgosX 的服务器)__</U>

每当电机关闭和电机开启功能发生时，控制台将打印以下字符串。
```
request : light-off
LED 灯已关闭

request : light-on
LED 灯已开启
```