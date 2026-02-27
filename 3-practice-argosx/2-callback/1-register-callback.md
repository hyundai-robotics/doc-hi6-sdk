#### 3.2.1 注册回调函数

注册回调函数的方法非常简单。每个事件的回调函数名称已经确定，因此，通过在插件代码中使用相关的回调函数名称定义回调函数，当插件被导入时，回调函数将自动注册。

我们需要再次参考<U>3.1.1 ArgosX及接口插件的规格</U>的其他函数。

根据机器人处于电机开启或关闭状态，ArgosX的LED灯应相应地开或关。具体来说，命令“light-on”或“light-off”应传递给ArgosX。

让我们在一个单独的文件（Python模块）中创建回调函数，如下所示。现在，我们只需打印字符串用于测试，而不执行特定操作。

callback.py（用于测试）
```python
def on_motor_on() -> int:
   """(回调) 电机开启时
   返回: 0
   """
   print('on_motor_on')
   return 0
 
 
def on_motor_off() -> int:
   """(回调) 电机关闭时
   返回: 0
   """
   print('on_motor_off')
   return 0
```

从入口文件导入回调模块。

main.py
```python
之前的步骤已略过...
 
from . import setup
from .roblang import *
from .setup import *
from .callback import *
 
import xhost
 
后续步骤已略过...

```
重新启动控制器，打开电机，并使用“Step FWD”按钮导入 ArgosX。

在这种状态下，如果每次电机开启或关闭时，控制台窗口打印出以下结果，则意味着回调函数定义良好。
```
on_motor_on

on_motor_off
```