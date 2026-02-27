#### 3.1.9 解决机器人语言功能阻塞问题
##### 阻塞问题

在上一节中实现的 recv_msg( ) 函数存在一个问题。

当调用此函数时，将会有一个等待远程响应的时间，只有在收到响应后，函数的操作才会结束。如果由于本应响应的 ArgosX 系统出现问题而没有响应，等待将无限期地继续，而函数不会结束。

我们来检查在没有来自 argosx_sub.py 的响应的情况下作业程序是如何操作的。

为了进行测试，在 argosx_stub.py 中找到一个名为 sleep_sec 的变量，并将其值更改为 20。然后，在接收到 "req ~" 请求时，argosx_stub 将会等待 20 秒，然后才会以 "res ~" 响应。

argosx_stub.py
``` python 
# const
buf_size = 0x8000    # 32kb ; 允许的数据包长度
port_no = 54321      # ArgosX 命令的端口
sleep_sec = 20        # 响应前的延迟
```

让我们再次执行 argosx_stub.py，然后使用教导挂钩的 STEP FWD 键逐行运行作业程序。

如果在执行 argosx.req 后立即执行 argosx.res( )，则在 req 执行后 20 秒内将无法移动光标，如下所示。这种状态称为阻塞，在用户可操作性方面并不好。该规格有问题，因为在没有响应的情况下可能导致需要关闭并重新开启机器人控制器的情况。
<br></br>
![](../../_assets/image_29.png)

哪个规格是理想的？一般来说，当命令等待某个状态，例如等待命令时，当按下 STEP FWD 键时，将会发生等待，而在未按下键时光标可以移动。此外，超时和转移地址可以指定为参数，因此当发生超时时，可以执行分支到转移地址的异常处理操作。
<br></br>
![](../../_assets/image_30.png)

如果在 wait-di6 状态下 10 秒后发生超时，则将会分支到 *tout。

#### 执行模式和继续模式

让我们看一下下面的流程图。主机 ${cont_model} 调用机器人语言命令时有两种模式：执行模式和继续模式。在继续模式下，主机再次调用命令。

主机首先在执行模式下调用命令。在大多数情况下，单个命令执行其操作并立即结束，而主机在确认不在继续模式后完成命令的处理。
<br></br>

![](../../_assets/image_31.png)
然而，一些命令具有等待操作（意味着它会等待某个状态或事件，例如 I/O 输入、以太网数据接收、某些时间段、机器人操作完成等）。在主机和插件之间将执行以下程序。

* 插件的等待操作命令将通过 xhost.exec_mode( ) 检查模式。True 表示执行模式，false 表示继续模式。因此，如果确认模式为执行模式（是），则传递给超时参数的时间将被设置为机器人语言的定时器（set_lang_timer），并且会请求 ${cont_model} 主机在操作结束之前下次以继续模式调用。
* 如果在执行 xhost.req_to_continue 后操作结束，则意味着模式为继续模式。因此，${cont_model} 主机再次调用相关命令。
* 插件的等待操作命令将通过 xhost.exec_mode( ) 检查模式。如果确认模式为继续模式（否），则将检查定时器。如果发生超时，将转向逃逸地址（branch_to_addr），并且操作将结束而不请求继续模式。
* 如果没有发生超时，将检查等待条件是否完成（等待完成条件？）。如果等待条件完成，操作将如是结束，因为没有请求继续模式，但如果没有完成，操作将结束，而不请求 ${cont_model} 主机以继续模式调用（req_to_continue）。
* 如果在当前调用中没有请求继续模式（继续模式 否），则 ${cont_model} 主机将完成相关命令的处理。
<br></br>

 ![](../../_assets/image_32.png)




现在让我们改进 argosx.res( )，使其也能够具有等待操作的规范。



##### 制作非阻塞通信模块


对于实现以太网传输/接收的通信模块，内部使用了套接字模块。套接字默认处于阻塞模式，这意味着 socket.recvfrom( ) 函数，即 UDP 接收函数，直到接收到数据才会执行任何返回操作。

首先，我们需要将使用的套接字实例更改为非阻塞模式。将 sock.setblocking(False) 插入到 comm.open( ) 函数中，如下所示。



comm.py
``` python
def open(ip_addr: str, port: int) -> int:
   """
   打开 UDP 通信的套接字
   参数：
      ip_addr     远程的 IP 地址。例如 "192.168.1.172"
      port        远程的端口号。例如 "192.168.1.172"
 
 
   返回：
         0     正常
         -1    错误
   """
   global raddr, sock
   try:
      raddr = (ip_addr, port)
      sock = socket.socket(family=socket.AF_INET, type=socket.SOCK_DGRAM)
      sock.setblocking(False)
   except socket.error as e:
      print("套接字创建或绑定错误 :", e)
      return -1
   logd('comm.open: ' + str(raddr))
   return 0
```
现在，对于 socket.recvfrom( ) 函数，如果没有接收到数据，将立即发生 BlockingIOError 异常，而不会出现阻塞（如果接收到数据，将立即发生返回操作）。

在 comm.recv_msg( ) 中插入处理，以便在发生 BlockingIOError 异常时返回一个空字符串。



comm.py
``` python
def recv_msg():
   """
   等待来自 sock 的消息
   返回:
      接收到的字符串
   """
   if sock is None: return ""
 
 
   try:
      data, ip_port = sock.recvfrom(buf_size)
      bts = bytearray(data)
      msg = bts.decode()
      logd('响应: ' + msg)
      return msg
   except BlockingIOError:
      return ""
   except Exception as e:
      print('来自 recv_msg() 的异常: ' + str(e))
      return ""

```


##### 在 res( ) 函数中实现等待操作


在 res( ) 函数中添加两个参数，timeout 和 addr_on_timeout（逃逸地址），如下所示。

如果未指定超时，则将应用默认值 -1，导致无限等待期。如果未指定逃逸地址，则将应用默认值 -1，导致在超时时不发生分支而转到下一个命令。

删除现有实现。之后，以简单的形式实现等待操作，模式将通过 xhost.exec_mode( ) 函数进行检查。如果模式为执行模式，将调用 res_exec( )，但如果处于继续模式，将调用 res_cont( ) 函数。



roblang.py
``` python 
""" ArgosX 视觉系统接口 - 机器人语言
 
 
@author:    Jane Doe, BlueOcean Robot & Automation, Ltd.
@created:   2021-12-06
"""
 
from . import comm
from . import setup
 
import xhost
import typing
 
 
# 类型
int_or_str = typing.Union[int, str]
 
 
 
跳过...
 
 
 
def res(timeout: int=-1, addr_on_timeout: int_or_str=-1) -> str:
   """
   等待来自 ArgosX 的响应
    
   参数:
      timeout: (毫秒)，默认值(-1)表示无限。
      addr_on_timeout: 超时时的分支地址。
         (例如 99, "S7", "*TimeOut")
         默认值(-1)表示无分支。
 
 
   返回:
      来自 ArgosX 的响应字符串。
      如果失败则返回 ""。
      例如 "[30, 25.7, 11.9, 31.6, 12.8, -54.6]"
   """
   ret = ""
   if xhost.exec_mode():
      _res_exec(timeout)
   else:
      ret = _res_cont(addr_on_timeout)
   return ret
```
现在，让我们在 res( ) 函数下面实现 _res_exec( )。在这个实现中，机器人的语言计时器将设置为 timeout 参数值，并请求主机在操作结束前以继续模式调用。不是很简单吗？



roblang.py
``` python
之前的步骤跳过...
 
 
def _res_exec(timeout: int) -> None:
   """res() 的 exec 模式实现"""
   xhost.set_lang_timer(timeout)
   xhost.req_to_continue()
```

实际操作将在 _res_cont( ) 函数中进行继续模式。在超时操作时，将创建 check_timeout_and_branch( ) 函数以进行分支。

如果发生超时，sigcode_err 输出信号将开启，操作将立即结束。

如果没有发生超时，将执行数据接收。如果在此过程中没有接收到数据，将请求主机以继续模式调用 (xhost.req_to_continue)。之后，将执行返回空字符串的操作。



roblang.py
``` python
之前的步骤跳过...
 
 
def _res_cont(addr_on_timeout: int_or_str) -> str:
   """res() 的 cont 模式实现"""
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
      elif msg=="":    # 没有响应
         xhost.req_to_continue()
      else:    # 正常响应
         msg = get_base_shift_array_from_res(msg)
   xhost.io_set_out_bit(setup.sigcode_err, val)
   return msg
 
 
 
def _check_timeout_and_branch(addr_on_timeout: int_or_str) -> bool:
   """如果超时，进行分支。
 
   返回：
      True     超时。已分支。
      False    未超时。
   """
   timer = xhost.lang_timer()
   if timer!=0: return False  # 未超时
   # 超时！
   if addr_on_timeout==-1:
      return True
   # 无条件转为字符串
   str_addr = str(addr_on_timeout)
   xhost.branch_to_addr(str_addr)
   return True
```
##### 测试非阻塞操作  

现在让我们检查一下规格是否按照我们的要求进行。再次执行虚拟控制器，并使用教学挂件的 STEP FWD 键逐行运行作业程序。

如果您执行 argosx.req，然后立即执行 argosx.res( )，操作将不会完成，因为响应尚未到来。当您释放 STEP FWD 键时，前进指示器将关闭，您可以移动光标。如果您按下 STEP FWD 键，等待状态将恢复。

如果在按住 STEP FWD 键时执行 req( ) 后经过 20 秒，接收将完成。  
<br>
![](../../_assets/image_33.png)

现在，让我们添加一个超时参数。将其指定为 3000 毫秒并再次执行操作。如果在按住 STEP FWD 键的情况下经过 3 秒，将移动到下一个命令。

job  
```
var str=argosx.res(3000) # waiting for a response
 
print str
```

现在，让我们添加一个逃逸步骤。当您按照以下方式进行教学，并在 res( ) 函数上按下 STEP FWD 键超过 3 秒时，您会看到分支到行号 99 发生，"timeout" 将被执行。

job  
```
... Previous steps skipped
      
     var str=argosx.res(3000,99) #waiting for a response
     print str
     if str==""
       print "req error"
       stop
     else
       var sft=Shift(str) # converting the shift array string into shift data
       print sft.x, sft.y, sft.z, sft.rx, sft.ry, sft.rz
     endif
      
     argosx.close()
     end
      
  99 print "timeout"
     end
```
