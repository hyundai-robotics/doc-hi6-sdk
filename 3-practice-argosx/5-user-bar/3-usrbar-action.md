#### 3.5.3 操作用户条

##### 客户端（教学挂件）

将以下内容写入 ui/ubar.js 文件。每次按钮被按下时，教学挂件将会向主板发送一个 HTTP POST 消息，名为 "light_onoff"。该消息是否开启或关闭将包含在一个具有 onoff 属性的对象中，并将加载在 POST 消息的主体中并发送。

- 开启灯光状态的主体：{ onoff: true }
- 关闭灯光状态的主体：{ onoff: false }

ui/ubar.js
///@author: Jane Doe, BlueOcean Robot & Automation, Ltd.
///@brief: ArgosX Vision System 界面 - 设置通用
///@create: 2021-12-07

///@return     例如 "http://192.168.1.150:8888"
function domainMb()
{
   var domain = "http://" + window.location.hostname + ":8888";
   console.log(domain);
   return domain;
}

///@param[in]  onoff    true 或 false
///@return
///      -  0     ok
///@brief      请求灯光开启/关闭
function light_onoff(onoff)
{
   var url = domainMb()+"/apps/argosx/svr_light_onoff";
   var args = { onoff: onoff };

   args = JSON.stringify(args);
   $.ajax({
      url: url,
      type: 'post',
      dataType: 'json',
      contentType : "application/json; charset=utf-8",
      data: args,
      success: function(res) {
         console.log('成功' + res);
      },
      error : function(res) {
         console.log('错误' + res);
      }
   });

   return 0;
}
##### 服务器端 (主板)

现在，主板的 ArgosX 插件需要接收此消息并向实际的 ArgosX 设备发送“light-on”和“light-off”消息（使用存根进行测试）。

将 ubar.py 文件写入 argosx/ 文件夹，具体如下。关于其实现，comm_ex 模块将类似于我们在前一章中执行回调实现的方式来使用。
``` python
""" ArgosX 视觉系统接口 - main
 
 
@author:    Jane Doe, BlueOcean Robot & Automation, Ltd.
@created:   2021-12-07
"""
 
 
from . import comm_ex
 
 
def post_light_onoff(body: dict) -> int:
   """
   Args:
      onoff
    
   Returns:
      0
   """
   onoff = body['onoff']
   msg = 'light-' + ('on' if onoff else 'off')
   print(msg)
       
   return comm_ex.send_msg_once(msg)
```

作为最后一步，将 ubar.py 的所有函数导入到 main.py。

main.py
``` python
""" ArgosX 视觉系统接口 - main
 
 
@author:    Jane Doe, BlueOcean Robot & Automation, Ltd.
@created:   2021-12-06
"""
 
from . import setup
from .roblang import *
from .setup import *
from .callback import *
from .ubar import *
 
import xhost
 
 
后续步骤已跳过...
```
重新启动虚拟控制器，然后运行作业文件直到 argosx.init( )。再次执行 Go Live 以在网页浏览器上显示用户栏界面。

执行 ArgosX 存根，然后在网页浏览器上操作按钮。如果存根在控制台上的输出显示为 "light-on" 和 "light-off"，则表示操作正常。
<br>
![](../../_assets/image_60.png)

控制台上的 ArgosX 存根输出
<br>
![](../../_assets/image_61.png)