#### 3.1.5 为ArgosX机器人语言创建函数

接下来要实现的规范是init( ), req( ), res( )和close( )函数。

(请参阅<u>3.1.1 ArgosX规范和接口插件的协议与机器人语言</u>的函数。)

现在，让我们对每个函数执行一次打印操作。

在argosx/文件夹下创建一个roblang.py文件，如下所示。

roblang.py (用于测试)
``` python 
""" ArgosX视觉系统接口 - 机器人语言
 
 
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

将所有名称从roblang.py导入到main.py中。
main.py
```python

""" ArgosX视觉系统接口 - 主要
 
 
@author:    Jane Doe, BlueOcean Robot & Automation, Ltd.
@created:   2021-12-06
"""
 
 
from .roblang import *
from .setup import *
 
 
def attr_names() -> tuple:
   """返回要暴露的属性名称。"""
   return ("ip_addr", "port")

```


job file
```
var iret
import argosx
 
print argosx.ip_addr
print argosx.port
argosx.ip_addr="192.168.1.172" # 你自己的PC名称
print argosx.ip_addr # 重新检查
 
iret=argosx.init() # 初始化套接字
if iret<0
  print "初始化错误"
  stop
endif
 
iret=argosx.req(39) # 发送请求
if iret<0
  print "请求错误"
  stop
endif
 
var str=argosx.res() # 等待响应
print str
 
argosx.close() # 关闭套接字
end
```
重新启动虚拟控制器并执行作业文件。如果文件正常创建，以下结果将打印在虚拟控制器控制台上。这是一个从 HRScript 调用的 Python 函数。
```
192.168.1.172

init()

req(39)

res()

data

close()
```