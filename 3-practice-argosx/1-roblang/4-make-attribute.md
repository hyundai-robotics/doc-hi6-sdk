#### 3.1.4 创建 ip_addr 和 port 属性

在查看 <u>3.1.1 ArgosX 规范及接口插件</u> 时，您可以找到一个字符串属性用于指定 ip_addr。

将全局变量和 attr_names( ) 函数添加到 argosx_main.py 文件，如下所示。

我们可以在 Python 程序中定义和使用许多全局变量。然而，只有在 attr_names( ) 返回的元组中列出的名称将以 ArgosX 模块属性的形式暴露在 HRScript 中。对于作为属性暴露的变量，应该定义添加有 get_ 和 set_ 前缀的 getter 和 setter 函数。

在此示例中，定义两个全局变量 ip_attr 和 port，然后将它们暴露为属性，并允许在 HRScript 中读取和写入它们。

在 argosx/ 项目文件夹中创建一个新的 setup.py 文件，然后将其实现如下。

setup.py
``` python 
""" robot application - argosx - setup
 
 
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

将整个 setup 模块导入 main.py，以便可以从 HRScript 调用 getter 和 setter 函数，然后定义 attr_name( ) 函数，返回属性名称的元组。
(The host handles the argosx/ folder as a package. When importing the setup module from the main module, you should explicitly add a . (period) in front of setup, which means the same folder.)



main.py
```python 
""" ArgosX视觉系统接口 - main
 
 
@author:    Jane Doe, BlueOcean Robot & Automation, Ltd.
@created:   2021-12-06
"""
 
from .setup import *
 
 
def attr_names() -> tuple:
   """返回要公开的属性名称。"""
   return ("ip_addr", "port")
```

测试作业程序应该如下所示进行教学和执行。
```
import argosx
print argosx.ip_addr
print argosx.port
end
```

如果Python全局变量的值按照下面所示的顺序在指导框中显示，表示操作正常。

```
192.168.1.100

54321
```


我们将测试ArgosX和同一台PC上的argosx_stub之间的通信。让我们将ip_addr的值更改为您的PC的IP地址，并检查是否已更改。

更改并执行作业程序如下。

```
import argosx
print argosx.ip_addr
print argosx.port
argosx.ip_addr="192.168.1.172" # 您自己的PC的IP地址
print argosx.ip_addr # 重新检查
end
```
检查新分配给 ip_addr 的值是否在指导框架中打印。  
```
192.168.1.172
```