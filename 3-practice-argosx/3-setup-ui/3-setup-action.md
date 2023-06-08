# 3.3.3 Operating the setup screen

As shown below, add the scripts into the head of setup.html.

The setup.js file is a script file designed to implement operations that are to be applied only on this setup screen. Additional descriptions will be provided below.



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
         <span class='col0' name='ip_addr'>IP address</span>
         <input class='col1' type='text' name='ip_addr' id='ip_addr_0' size='3'/>
         .
         <input class='col1' type='text' name='ip_addr' id='ip_addr_1' size='3'/>
         .
         <input class='col1' type='text' name='ip_addr' id='ip_addr_2' size='3'/>
         .
         <input class='col1' type='text' name='ip_addr' id='ip_addr_3' size='3'/>
         <br>
         <span class='col0' name='port'>Port#</span>
         <input class='col1' type='text' id='port' size='5'/>
         <br>
         <span class='col0' name='sigcode_err'>Failure output signal</span>
         <input class='col1' type='text' id='sigcode_err' size='5'/>
      </div>
      <div id='guidebar'></div>
   </div>
</body>
</html>
```



Add the setup.js file into the ui/ folder and write the following content.



ui/setup.js
``` js
///@author: Jane Doe, BlueOcean Robot & Automation, Ltd.
///@brief: ArgosX Vision System interface - setup
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
 
 
///@brief      have guidebar display message on clicking widget
function updateGuideBar()
{
   let sg = setGuideBarMsg;
   let msg_ip_addr = 'Enter the IP address of ArgosX.'
   let msg_port = 'Enter the port # of ArgosX.'
   let msg_sigcode = 'Enter the number of the signal to assign.[0 - 4096]';
    
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

When the cursor is located on the input element, the updateGuideBar( ) function will call the setGuideBarMsg( ) function, then it will designate the message to be displayed in the guidance frame.

__setGuideBarMsg(The ID or name of the element and the message to be displayed)___
<br></br>

For example, sg('ip_addr', msg_ip_addr); in the example above refers to the setting that displays the msg_ip_addr string in the guidance frame when the cursor is located on the input element with the name 'ip_addr.'



When the setup screen is opened initially, the current setup value needs to be loaded. Upon pressing the [OK] button, the value should be saved.

Operations related to these settings are to be implemented by calling setDomPath("/apps/argosx/svr_setup"); and defining the updateData() function.

More detailed descriptions will be provided in the subsequent sections.