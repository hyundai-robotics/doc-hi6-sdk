#### 3.2.1 注册回调函数


注册回调函数的方法非常简单。每个事件的回调函数名称已经确定，因此通过在插件代码中使用相关的回调函数名称定义回调函数，回调函数将在插件导入时自动注册。


我们需要再次参考<U>3.1.1 ArgosX及接口插件的规格</U>中的其他功能。

根据机器人是否处于电机开启或关闭状态，ArgosX的LED灯也应相应地打开或关闭。具体而言，命令"light-on"或"light-off"应发送到ArgosX。



让我们在一个单独的文件（Python模块）中创建回调函数，如下所示。现在，我们将只打印字符串进行测试，而不执行特定操作。



callback.py (用于测试)
```python
def on_motor_on() -> int:
   """(callback) 在电机开启时
   Returns: 0
   """
   print('on_motor_on')
   return 0
 
 
def on_motor_off() -> int:
   """(callback) 在电机关闭时
   Returns: 0
   """
   print('on_motor_off')
   return 0
```

从入口文件导入回调模块。



main.py
```python
跳过之前的步骤...
 
from . import setup
from .roblang import *
from .setup import *
from .callback import *
 
import xhost
 
跳过后续步骤...

```
重启控制器，开启电机，并使用Step FWD按钮导入ArgosX。

在这个状态下，如果每次电机开启或关闭时在控制台窗口中打印的结果如下，这意味着回调函数定义良好。
```
on_motor_on

on_motor_off
```