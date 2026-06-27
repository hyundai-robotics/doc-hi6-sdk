#### 3.1.5 为 ArgosX 机器人语言创建函数

接下来要实现的规格是 init( )、req( )、res( ) 和 close( ) 函数。

(请参阅 <u>3.1.1 ArgosX 及接口插件的规格</u> 中的协议和机器人语言的函数。)

现在，让我们为每个函数执行一个打印操作。

在 argosx/ 文件夹下创建 roblang.py 文件，如下所示。

roblang.py (用于测试)
``` python 
""" ArgosX 视觉系统接口 - 机器人语言
 
 
@author:    Jane Doe, BlueOcean Robot & Automation, Ltd.
@created:   2021-12-06
"""
 
 
# 函数
def init() -> int:
   print("init()")
   return 0
 
 
def close():
   print("close()")
 
 
def req(work_no: int) -> int:
   msg = "req(" + str(work_no) + ")"
   print(msg)
   return 0
 
 
def res():
   print("res()")
   return "data"
```

将 roblang.py 中的所有名称导入到 main.py 中。

main.py
```python

""" ArgosX 视觉系统接口 - 主
 
 
@author:    Jane Doe, BlueOcean Robot & Automation, Ltd.
@created:   2021-12-06
"""
 
 
from .roblang import *
from .setup import *
 
 
def attr_names() -> tuple:
   """返回要暴露的属性名称。"""
   return ("ip_addr", "port")

```

作业文件
```
var iret
import argosx
 
print argosx.ip_addr
print argosx.port
argosx.ip_addr="192.168.1.172" # 您自己的计算机名称
print argosx.ip_addr # 重新检查
 
iret=argosx.init() # 初始化套接字
if iret<0
  print "init error"
  stop
endif
 
iret=argosx.req(39) # 传输请求
if iret<0
  print "req error"
  stop
endif
 
var str=argosx.res() # 等待响应
print str
 
argosx.close() # 关闭套接字
end
```

重启虚拟控制器并执行作业文件。如果文件创建正常，则将在虚拟控制器控制台打印以下结果。这是从 HRScript 调用的 Python 函数。
```
192.168.1.172

init()

req(39)

res()

data

close()
```