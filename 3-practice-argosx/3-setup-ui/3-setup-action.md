### 3.3.3 操作设置屏幕

如下面所示，将脚本添加到setup.html的头部。

setup.js文件是一个脚本文件，旨在实现仅在此设置屏幕上应用的操作。后续将提供更多描述。

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
   <title>ArgosX 视觉系统 - 设置</title>
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
         <span class='col0' name='sigcode_err'>失败输出信号</span>
         <input class='col1' type='text' id='sigcode_err' size='5'/>
      </div>
      <div id='guidebar'></div>
   </div>
</body>
</html>
```

将setup.js文件添加到ui/文件夹中，并写入以下内容。

ui/setup.js
``` js
///@author: Jane Doe, BlueOcean Robot & Automation, Ltd.
///@brief: ArgosX 视觉系统界面 - 设置
///@create: 2021-12-06
 
 
 
function init()
{
   setDomPath("/apps/argosx/svr_setup");
   setUpdateGuideBar(updateGuideBar);
   setUpdateData(updateData);
   onReady();
}
 
 
///@return     f-button infos array
function initButtonBar()
{
   console.log('initButtonBar()'); 
   var btn_infos = [
   ]
   return btn_infos;
}
 
 
///@brief      在点击小部件时让指南条显示消息
function updateGuideBar()
{
   let sg = setGuideBarMsg;
   let msg_ip_addr = '输入ArgosX的IP地址。'
   let msg_port = '输入ArgosX的端口#。'
   let msg_sigcode = '输入分配的信号编号。[0 - 4096]';
    
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

当光标位于输入元素上时，updateGuideBar()函数将调用setGuideBarMsg()函数，然后将指定要在指导框中显示的消息。

__setGuideBarMsg(元素的ID或名称和要显示的消息)___
<br></br>

例如，上面示例中的sg('ip_addr', msg_ip_addr);指的是当光标位于名称为'ip_addr'的输入元素时，将msg_ip_addr字符串显示在指导框中的设置。

当设置屏幕首次打开时，需要加载当前的设置值。在按下[确定]按钮时，应保存该值。

与这些设置相关的操作将通过调用setDomPath("/apps/argosx/svr_setup");和定义updateData()函数来实现。

更详细的描述将在后续部分提供。