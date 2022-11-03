# 3.3.3 설정화면의 동작

setup.html의 head 안에 아래와 같이 script를 추가합니다.

setup.js 파일은 이 설정화면에만 적용되는 동작들을 구현하기 위한 script 파일입니다. 아래에서 다시 설명됩니다.



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



ui/ 폴더에 setup.js 파일을 추가하고 내용을 아래와 같이 작성합니다.



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

updateGuideBar( ) 함수는 입력 element에 커서가 위치했을 때, 안내 프레임에 어떤 메시지를 표시할 지를 setGuideBarMsg( ) 함수를 호출하여 지정합니다.



__setGuideBarMsg(element의 id 혹은 name, 표시할 message)___



가령, 위의 예에서 sg('ip_addr', msg_ip_addr);는 name이 'ip_addr'인 input element에 커서가 위치했을 때, msg_ip_addr 문자열을 안내 프레임에 표시하라는 설정입니다.



처음 설정화면이 열렸을 때, 현재 설정 값을 불러오고 [OK] 버튼을 클릭하면 저장해야 합니다.

이러한 설정값과 관련된 동작은 setDomPath("/apps/argosx/svr_setup");의 호출과 updateData( ) 함수의 정의로 구현됩니다.

자세한 내용은 이 후의 절에서 다시 설명하겠습니다.