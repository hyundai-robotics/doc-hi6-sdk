#### 3.1.7 调用 xhost 模块方法

xhost 是一个包含各种方法的模块，用于调用主机（机器人控制器）的功能。

虚拟控制器是主要模块，将创建 xhost 并将其注入到 Python 运行时中。您可以通过导入 xhost 来使用它，无需自己编写 xhost.py 文件。

请参阅<U>3.1.8 手动参考 xhost 模块的方法</U>。

在<U>3.1.1 ArgosX 规格及接口插件</U>中也有关于错误处理的内容。

- 当从 ArgosX 收到“fail”时，相应于预设号码的机器人控制器的通用 I/O 输出信号将被打开。

可以使用下面的方法开/关机器人控制器的通用 I/O 输出信号。
``` python 
def io_set_out_bit(sigcode: int, val: int) -> int
```

sigcode 是一个将块号码和 I/O 索引合并为一个数字的代码，如下所示。

sigcode = 块号码 x 10000 + 索引
<br></br>

例如，fb3.do72 的 sigcode 如下所示。

3 x 10000 + 72 = 30072
<br></br>

如果 val 为 1，则表示打开，如果为 0，则表示关闭。
<br></br>

将用于 ArgosX 错误的输出信号分配号码添加为名为 sigcode_err 的模块变量，并将其默认值设置为 5（即 fb0.do5.）

（我们也可以将其声明为属性，以便在 HRScript 中进行更改。然而，在本示例中将跳过此步骤。）

setup.py
```python 
..previous steps skipped
ip_addr : str = "192.168.1.100"
port : int = 54321
sigcode_err = 5
```
收到的 msg 值将在 res( ) 函数中与 "res fail" 进行比较，并根据结果传输输出信号。
<br></br>

roblang.py
```python
.. previous steps skipped
  
  
import xhost
  
  
...skipped
 
 
def res() -> str:
   """
   等待来自 ArgosX 的响应
   返回：
      来自 ArgosX 的响应字符串
      失败时返回 ""。
      例如 "[30, 25.7, 11.9, 31.6, 12.8, -54.6]"
   """
   val = 0
   msg = comm.recv_msg()
   print(msg)
   if msg=="res fail":
      val = 1
      msg = ""
   else:
      msg = get_base_shift_array_from_res(msg)
   xhost.io_set_out_bit(setup.sigcode_err, val)
   return msg
```

执行虚拟控制器，并在保持教学挂件的通用输出面板打开的同时，执行作业程序。

由于没有失败，操作将与之前相同，并且 fb0.do5 打印信号将不会开启。

argosx_stub.py 被设计为在请求工作 #98 时无条件地响应失败。修改作业，以便可以执行 req(98)，如下所示，然后再次执行实施。

作业
```
...Previous steps skipped
 
 
     iret=argosx.req(98) # 发送请求
     if iret<0
       print "req error"
       stop
     endif
      
     var str=argosx.res() # 等待响应
     print str
     if str==""
       print "req error"
       stop
     else
       var sft=Shift(str) # 将移动数组字符串转换为移动数据
       print sft.x, sft.y, sft.z, sft.rx, sft.ry, sft.rz
     endif
 
     argosx.close() # 关闭套接字
     end
```
如果在执行 res( ) 时 fb0.do5 打印信号被打开，这意味着错误信号已正常打印。

![](../../_assets/image_27.png)

信号 #5 应仅用于 ArgosX 错误。因此，它不能用于其他需要被分配信号的应用。

使用以下 xhost 方法，您可以指定一个特定的 sigcode 作为分配。
```python 
def io_assign_set_out_bit(sigcode: int) -> int
```

当您在 main.py 中定义 on_app_init( ) 函数时，然后输入一个指定分配的例程，如下所示，执行将在导入 ArgosX 的时候发生。

main.py
```python 
.. previous steps skippd
 
 
import xhost
 
 
...skipped
 
 
def on_app_init() -> int:
   """(callback) 自我诊断后调用
   返回:
      0
   """
   print('[argosx] on_app_init();')
   xhost.io_assign_set_out_bit(setup.sigcode_err)
   return 0
```

再次执行虚拟控制器。然后，当在任务中执行 import argosx 时，重新打开通用输出面板。

指定的信号将显示为已分配（加粗）。

![](../../_assets/image_28.png)