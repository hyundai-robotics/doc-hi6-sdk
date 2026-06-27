#### 3.1.4 创建 ip_addr 和 port 属性

当查看 <u>3.1.1 ArgosX 的规范和接口插件</u> 时，您可以找到一个用于指定 ip_addr 的字符串属性。

将全局变量和 attr_names( ) 函数添加到 argosx_main.py 文件中，如下所示。

我们可以在 Python 程序中定义和使用许多全局变量。然而，只有在 attr_names( ) 返回的元组中列出的名称才会作为 ArgosX 模块属性暴露在 HRScript 中。对于暴露为属性的变量，应定义以 get_ 和 set_ 为前缀的 getter 和 setter 函数。

在本示例中，让我们定义两个全局变量，ip_attr 和 port，然后将它们暴露为属性，并允许在 HRScript 中读取和写入它们。

在 argosx/ 项目文件夹中创建一个新的 setup.py 文件，然后按照如下实现。

setup.py
``` python 
""" 机器人应用程序 - argosx - setup
 
 
@author:    Jane Doe, BlueOcean Robot & Automation, Ltd.
@created:   2021-12-06
"""
 
 
 
# 属性 getter/setter
def get_ip_addr() -> str:
   return ip_addr
 
def set_ip_addr(addr: str):
   global ip_addr
   ip_addr = addr
 
def get_port() -> int:
   return port
 
def set_port(_port: int):
   global port
   port = _port
 
ip_addr : str = "192.168.1.100"
port : int = 54321
```

将整个 setup 模块导入 main.py，以便可以从 HRScript 调用 getter 和 setter 函数，然后定义返回属性名称元组的 attr_name( ) 函数。

（主机将 argosx/ 文件夹作为一个包处理。当从主模块导入 setup 模块时，您应显式在 setup 前面添加 .（句点），这意味着同一文件夹。）

main.py
```python 
""" ArgosX 视觉系统接口 - main
 
 
@author:    Jane Doe, BlueOcean Robot & Automation, Ltd.
@created:   2021-12-06
"""
 
from .setup import *
 
 
def attr_names() -> tuple:
   """返回要暴露的属性名称。"""
   return ("ip_addr", "port")
```

测试作业程序应按如下方式进行教学和执行。
```
import argosx
print argosx.ip_addr
print argosx.port
end
```

如果 Python 全局变量的值按顺序显示在指导框中，如下所示，则意味着操作正常。

```
192.168.1.100

54321
```

我们将测试 ArgosX 与同一台 PC 上的 argosx_stub 之间的通信。让我们将 ip_addr 的值更改为您的 PC 的 IP 地址，并检查是否已更改。

更改并执行作业程序，如下所示。

```
import argosx
print argosx.ip_addr
print argosx.port
argosx.ip_addr="192.168.1.172" # 您自己的 PC 的 IP 地址
print argosx.ip_addr # 重新检查
end
```

检查在指导框中是否打印出新分配给 ip_addr 的值。
```
192.168.1.172
```