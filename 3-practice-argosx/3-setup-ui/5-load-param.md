### 3.3.5 加载和保存设置屏幕的值

当打开设置屏幕时，保存在 Python 插件中的设置值将作为中间 JavaScript 对象加载，然后显示为屏幕上的 HTML 元素。

当用户修改这些值并点击 [OK] 按钮时，屏幕上的 HTML 元素将作为中间 JavaScript 对象创建并保存到 Python 插件中的变量中。

要执行传输，实施 JavaScript 的 updateData( ) 函数以及 Python setup 模块的 getter 和 putter 函数。
<br></br>

![](../../_assets/image_44.png)

##### element ↔ javascript object

setup.js
``` js
Previous steps skipped...
 
 
///@param[in]  data
///@param[in]  to_data     true; element->data, false; element<-data
function updateData(data, to_data)
{
   ddx_edit_ip(data, 'ip_addr', to_data);
   ddx_edit_i(data, 'port', to_data);
   ddx_edit_sig(data, 'sigcode_err', to_data);
}
```

元素与 JavaScript 对象之间的双向值传输通过 updateData( ) 函数定义。参数 data 是 JavaScript 对象，而 to_data 是布尔变量，指示传输的方向。如果为 true，方向为从元素到数据；如果为 false，方向为从数据到元素。

我们还可以直接使用文档对象模型应用程序编程接口（DOM API）或 jQuery 实现传输。然而，通过使用 dst_setup.js 提供的动态数据交换（DDX）函数，可以更简洁地实现传输。
<br></br>

DDX 函数

|Function signature|HTML element|Data type|Description|
|---|---|---|---|
|ddx_edit(data, name, to_data)|```<input type='text'>```|string||
|ddx_edit_i(data, name, to_data)|```<input type='text'>```|integer||
|ddx_edit_sig(data, name, to_data)|```<input type='text'>```|integer|如果设置通用 I/O 信号的元素值为 sigcode，则将按原样传输。<br>如果值为 fb?.? 格式，将转换为 sigcode 并传输。|
|ddx_edit_ip(data, name, to_data)|```<input type='text'>``` x 4 (units)|string|这是用于设置 IP 地址的。<br>如果名称为 'ip'，则四个元素的每个 ID 应为 'ip_0'，'ip_1'，'ip_2' 和 'ip_3'。<br>数据将保存为 "xxx.xxx.xxx.xxx."|
|ddx_check(data, name, to_data)|```<input type='checkbox'>```|boolean||
|ddx_radio(data, name, to_data)|```<input type='radio'>```x N (units)|integer|每个单选元素应具有唯一的值属性。|

<br>

##### javascript object ↔ python data

setup.js
``` js
///@author: Jane Doe, BlueOcean Robot & Automation, Ltd.
///@brief: ArgosX Vision System interface - setup general
///@create: 2021-12-06
 
 
function init()
{
   setDomPath('/apps/argosx/svr_general');
   setUpdateGuideBar(updateGuideBar);
   setUpdateData(updateData);
   onReady();
}
 
 
...Subsequent steps skipped
```

在初始化步骤中，使用 setDomPath( ) 函数指定了 "/apps/argosx/svr_general" 路径。

所有插件，包括 ArgosX，都将部署在 /apps/ 下，路径的最后一个名称是 "svr_"，并附加设置组名称。

<br></br>

**/apps/{app name}/svr_{setup group name}**

<br></br>
一个插件可以有一个或多个设置屏幕，一个屏幕上的数据将被视为一个对象，并称为设置组。每个设置组可以自由命名，那个名称成为设置组名称。在本例中，设置组名称确定为 'general'。

如果你在设置组名称前面添加前缀 "get_" 或 "put_"，而不是 "svr_"，它将分别成为 getter 或 putter 服务函数名称。此外，如果在 getter 函数名称后添加 '_def'，它将成为获取默认值的默认 getter 服务函数名称。

因此，在上面的例子中，默认 getter、getter 和 putter 服务函数名称将分别变为 get_general_def( )，get_general( ) 和 put_general( )。

将 setup.py 文件添加到项目文件夹 argosx/ 中。
<table>
  <thead>
    <tr>
      <th style="text-align:left"></th>
      <th style="text-align:left">Function</th>
      <th style="text-align:left">调用发生的时间点</th>
      <th style="text-align:left">操作</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>默认 getter</td>
      <td>get_general_def( )</td>
      <td>当请求默认值时。</td>
      <td>每个默认设置值将作为属性值保存在一个对象中，所有对象将被返回。</td>
    </tr>
    <tr>
      <td>dgetter</td>
      <td>get_general( )</td>
      <td>当打开设置屏幕时。</td>
      <td>每个 Python 插件具有的默认设置值将作为属性值保存在对象中，所有对象将被返回。</td>
    </tr>
    <tr>
      <td>putter</td>
      <td>put_general( )</td>
      <td>当使用 [OK] 或 [Apply] 按钮保存设置屏幕的值时。</td>
      <td>传递到 body 参数的属性值将被读取并保存在 Python 插件中。</td>
    </tr>
  </tbody>
</table>

<br>

现在，让我们实现各个函数。每个属性的键应与 DDX 函数中使用的相同名称。

setup.py
``` python

""" 机器人应用程序 - argosx - 设置
 
 
@author:    Jane Doe, BlueOcean Robot & Automation, Ltd.
@created:   2021-12-06
"""
 
 
def get_general_def() -> dict:
   """
   返回:
      设置的默认值
   """
   print('def_general_def()')
 
 
   data_def = {
      'ip_addr': '192.168.1.100',
      'port' : 54321,
      'sigcode_err': 5
   }
    
   return data_def
 
 
def get_general() -> dict:
   """
   返回:
      设置字典。
   """
 
   print('get_general()')
    
   ret = {}
   ret["ip_addr"] = ip_addr
   ret["port"] = port
   ret["sigcode_err"] = sigcode_err
    
   return ret
 
    
def put_general(body: dict) -> int:
   """
   参数:
      body  设置字典。
    
   返回:
      0
   """
   global ip_addr, port, sigcode_err
   print('put_general()')
 
   ip_addr = body["ip_addr"]
   port = body["port"]
   sigcode_err = body["sigcode_err"]
 
   save_to_setup_file(body) # 保存到文件
    
   return 0
```

关于 setup.py 中全局变量的初始化，让我们将当前方法更改为使用默认 getter 函数的方法，以便代码不会复制。

setup.py
``` python
...Previous steps skipped
 
 
gen_def = get_general_def()
ip_addr : str = gen_def['ip_addr']
port : int = gen_def['port']
sigcode_err = gen_def['sigcode_err']
```

##### 操作测试

现在，让我们重启虚拟控制器，导入 ArgosX，然后进入 ArgosX 设置屏幕。默认设置值将显示如下。
<br> ![](../../_assets/image_45.png) <br>

将 IP 地址更改为 192.168.1.172，输入 3.4 作为故障输出信号，然后按 <Enter> 将其值更改为 fb3.4。之后，按 [OK] 按钮退出屏幕。
<br>

当你再次进入屏幕时，如果新设置的值正常显示，这意味着操作正常。
<br> ![](../../_assets/image_46.png) <br>