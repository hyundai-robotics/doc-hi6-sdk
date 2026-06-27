#### 3.2.2 实现回调函数
因为我们确保回调函数被正确调用，接下来实现实际操作。

如您在检查<U>3.1.1 ArgosX 及接口插件的规格</U>时所见，只需将 “light-on” 和 “light-off” 消息发送到 ArgosX 硬件即可。

因为 comm 模块已经实现了发送以太网字符串的函数，您只需如下调用一个。

```
comm.send_msg("light-on")
comm.send_msg("light-off")
```
<br></br>

但是，这个实现有一个问题。如果通过 argosx.init 调用 comm.open( ) 函数，一个机器人语言命令，字符串将会正常传输。但是，如果未调用该函数，字符串将不会被传输。

此外，即使由于 comm.close( ) 而关闭通信时也不会发生传输。

因此，有必要定义一个字符串传输函数，使其能够在关闭状态下打开通信，并进行传输后再关闭通信。

<br></br>
在 comm.py 所在的同一文件夹中创建一个 comm_ex.py 文件，如下所示。
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

现在，我们可以简单地实现回调函数，如下所示，通过导入 comm_ex 模块。

callback.py

``` python
""" ArgosX 视觉系统接口 - 回调函数
 
 
@author:    Jane Doe, BlueOcean Robot & Automation, Ltd.
@created:   2021-12-07
"""
 
from . import comm_ex
 
 
def on_motor_on() -> int:
   """(callback) 电机开启
   Returns: 0
   """
   print('on_motor_on')
   return comm_ex.send_msg_once("light-on")
 
 
def on_motor_off() -> int:
   """(callback) 电机关闭
   Returns: 0
   """
   print('on_motor_off')
   return comm_ex.send_msg_once("light-off")
```

首先，从命令提示符或 vscode 执行 argosx_stub。

重启虚拟控制器，然后运行作业文件直到 argosx.init( )。如果在此状态下执行操作如下，则表示已检查正常照明功能的操作。

<br></br>
<U>__argosx_stub 侧（充当 ArgosX 的服务器）__</U>

每当发生电机关闭和电机开启功能时，以下字符串将打印在控制台上。
```
request : light-off
LED 灯已关闭

request : light-on
LED 灯已开启
```