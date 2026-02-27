### 3.3.3 操作设置屏幕

如下所示，将脚本添加到 setup.html 的头部。

setup.js 文件是一个脚本文件，旨在实现仅在此设置屏幕上应用的操作。将提供以下附加描述。



ui/setup.html
``` html
<!DOCTYPE html:5>
<!--
   @author: Jane Doe, BlueOcean Robot & Automation, Ltd.
   @brief: ArgosX Vision System interface - setup
   @create: 2021-12-06
-->
<html>
  
<head>
   <title>ArgosX Vision System - setup</title>
   <meta http-equiv=Content-Type content='text/html; charset=utf-8'>
   <link rel='stylesheet' href='../../_common/css/style.css' type=text/css rel=stylesheet>
   <script src='../../_common/js/jquery-3.6.0.min.js'></script>
   <script src='../../_common/js/Parser.js'></script>
   <script src='../../_common/js/sigcode.js'></script>
   <script src='../../_common/js/dst_setup.js'></script>
   <script src='./setup.js'></script>
   <script>
      $(document).ready(init);
   </script>
</head>
  
<body>
   <div>
      <div id='contents'>
         <span class='col0' name='ip_addr'>IP 地址</span>
         <input class='col1' type='text' name='ip_addr' id='ip_addr_0' size='3'/>
         .
         <input class='col1' type='text' name='ip_addr' id='ip_addr_1' size='3'/>
         .
         <input class='col1' type='text' name='ip_addr' id='ip_addr_2' size='3'/>
         .
         <input class='col1' type='text' name='ip_addr' id='ip_addr_3' size='3'/>
         <br>
         <span class='col0' name='port'>端口#</span>
         <input class='col1' type='text' id='port' size='5'/>
         <br>
         <span class='col0' name='sigcode_err'>故障输出信号</span>
         <input class='col1' type='text' id='sigcode_err' size='5'/>
      </div>
      <div id='guidebar'></div>
   </div>
</body>
</html>
```
将setup.js文件放入ui/文件夹并写入以下内容。

``` js
///@author: Jane Doe, BlueOcean Robot & Automation, Ltd.
///@brief: ArgosX视觉系统接口 - 设置
///@create: 2021-12-06
 
 
 
function init()
{
   setDomPath("/apps/argosx/svr_setup");
   setUpdateGuideBar(updateGuideBar);
   setUpdateData(updateData);
   onReady();
}
 
 
///@return     f-button infos数组
function initButtonBar()
{
   console.log('initButtonBar()'); 
   var btn_infos = [
   ]
   return btn_infos;
}
 
 
///@brief      在点击小部件时，指南栏显示消息
function updateGuideBar()
{
   let sg = setGuideBarMsg;
   let msg_ip_addr = '输入ArgosX的IP地址。'
   let msg_port = '输入ArgosX的端口号。'
   let msg_sigcode = '输入要分配的信号的号码。[0 - 4096]';
    
   sg('ip_addr', msg_ip_addr);
   sg('port', msg_port);
   sg('sigcode_err', msg_sigcode);
}
 
 
///@param[in]  data
///@param[in]  to_data     true; widget->data, false; widget<-data
function updateData(data, to_data)
{
   ddx_edit_ip(data, 'ip_addr', to_data);
   ddx_edit_i(data, 'port', to_data);
   ddx_edit_sig(data, 'sigcode_err', to_data);
}
```
当光标位于输入元素上时，updateGuideBar( ) 函数将调用 setGuideBarMsg( ) 函数，然后指定要在指导框中显示的消息。

__setGuideBarMsg(元素的 ID 或名称和要显示的消息)___
<br></br>

例如，sg('ip_addr', msg_ip_addr); 在上面的例子中指的是当光标位于名称为 'ip_addr' 的输入元素上时，将 msg_ip_addr 字符串显示在指导框中的设置。

当初始打开设置屏幕时，需要加载当前设置值。在按下 [OK] 按钮时，值应被保存。

与这些设置相关的操作通过调用 setDomPath("/apps/argosx/svr_setup"); 和定义 updateData() 函数来实现。

更详细的描述将在后续部分中提供。