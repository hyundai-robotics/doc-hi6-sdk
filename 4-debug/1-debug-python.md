# 4.1 调试 Python 代码

如果我们编写的 Python 代码在 веб UI 中有语法错误或在实现主体中有逻辑错误，则应使用调试器来追踪原因并加以补充。

然而，插件的 Python 源代码是通过导入操作、回调操作或机器人语言从 ${cont_model} 主机调用的。由于 ${cont_model} 主机中没有内置的 Python 调试器，因此无法通过在主机调用时放置断点来进行追踪。不过，您可以通过使用外部调试器执行插件的入口函数来进行部分调试。


<br>

#### 调试用的 xhost
xhost 是一个模块，用于插件调用 ${cont_model} 主机的功能。它是由主机创建并注入到插件中的。下图显示了主机与插件之间典型的操作流程。
<br> ![](../_assets/image_63.png)




然而，当使用 vscode 调试器执行插件时，调用 xhost 方法时会发生执行错误，因为没有创建/注入实际的 xhost 模块。因此，应使用调试用的替代 xhost。在 SDK 的公共文件夹中，有一个替代模块 xhost_dbg。它是通过以太网调用 OpenAPI 来执行主机操作的一种代理。

 ![](../_assets/image_64.png)
<br>





下面看到的是 apps/ 文件夹中一个简单的 xhost.py，它导入了 xhost_dbg。当实际的 ${cont_model} 主机创建/注入 xhost 时，它将覆盖这个 xhost 替代模块。



xhost.py
``` python
""" 调试用的 xhost
   此模块将被主机覆盖。
"""
from _common.py.xhost_dbg import *
```

主机可以是运行在 PC 开发环境中的 ${cont_model} 虚拟控制器或实际控制器。您需要指定 xhost_dbg 应该访问的位置。

在 ${cont_model} 主路径的 apps/ 文件夹中有 xhost_remote_ip.py 文件。默认值设置为 PC 开发环境本身（即 ${cont_model} 虚拟控制器），如下所示。



xhost_remote_ip.py
``` python
remote_ip="127.0.0.1"
```

如果您希望通过 IP 地址 "192.168.1.150" 访问实际的 ${cont_model} 控制器进行测试，可以如下设置。
xhost_remote_ip.py
``` python
remote_ip="192.168.1.150"
```



#### 使用 Visual Studio Code 和 test.py 进行调试
如果在<u> 1.5 安装 Visual Studio Code</u>中正确安装了 Microsoft Python 扩展，则可以使用 Python 调试环境。如果您已经熟悉在 vscode 中调试 Python，则可以跳过此部分。




为了追踪机器人语言或回调操作要调用的函数，我们在 argosx/ 文件夹下定义了测试模块，如下图所示。实现需要测试的例程，然后从主例程调用 test( ) 函数。

您可以通过点击行号左侧切换断点。



test.py
<br></br>
![](../_assets/image_65.png)



当您在 test.py 窗口中按下 F5 键或单击运行 - 启动调试菜单时，会在 Python 运行时和调试模式下执行。
<br></br>
![](../_assets/image_66.png)


对于上一节中的 ArgosX 插件，您应该执行 argosx_stub 以进行测试。如果需要运行两个 Python 程序，如本例中所示，则需要打开两个 vscode 会话并在每个会话中运行调试器，如下图所示。
<br></br>
![](../_assets/image_67.png)





当放置断点时，VARIABLES、WATCH、CALL STACK 和 BREAKPOINTS 窗口将在左侧打开。您可以使用右上角的操作按钮进行追踪：继续 (F5)、跳过 (F10)、进入 (F11) 和退出 (Shift+F11)。如果您单击重启 (Ctrl+Shift+F5) 按钮，执行将从头开始重启。如果您单击停止 (Shift+F5) 按钮，调试将停止。

当调用 print( ) 命令（一个内置的 Python 函数）时，字符串将在底部的 TERMINAL 窗口中打印出来，可以用于调试。
<br></br>
![](../_assets/image_68.png)
test.py 的操作与通过 xhost_dbg 连接的控制主机联锁，因此可以在检查实际操作的同时进行调试，如下所示。 
<br></br>
![](../_assets/image_69.png)