# 3.4.2 모니터링 panel의 레이아웃

argosx 의 부모인 apps/ 폴더에 대해 vscode를 여십시오.


ui/panel.html 과 panel.js 파일을 생성합니다.

![](../../_assets/image_50.png)


내용은 아래와 같이 작성합니다. <table> 하나로 구성되어 있는 간단한 layout입니다. table의 첫번째 열에는 'thd' 라는 class를 부여했는데(table header의 줄임말), 이 class는 공용 style.css에 회색 바탕의 검은 글씨로 정의되어 있습니다.  스타일을 바꾸고 싶다면, 별도의 local css로 class를 정의하여 적용해도 됩니다.



panel.html
``` html
<!DOCTYPE html:5>
<!--
   @author: Jane Doe, BlueOcean Robot & Automation, Ltd.
   @brief: ArgosX Vision System interface - panel
   @create: 2021-12-07
-->
<html>
  
<head>
   <title>ArgosX Vision System</title>
   <meta http-equiv=Content-Type content='text/html; charset=utf-8'>
   <link rel='stylesheet' href='../../_common/css/style.css' type=text/css rel=stylesheet>
   <script src='../../_common/js/jquery-3.6.0.min.js'></script>
   <script src='./panel.js'></script>
   <script>
      $(document).ready(init);
   </script>
</head>
  
<body>
   <table>
      <th>name</th>
      <th>value</th>
      <tr>
         <td class='thd'>IP address</td>
         <td id='ip_addr'></td>
      </tr>
      <tr>
         <td class='thd'>port#</td>
         <td id='port'></td>
      </tr>
      <tr>
         <td class='thd'>sigcode for error</td>
         <td id='sigcode_err'></td>
      </tr>
      <tr>
         <td class='thd'>n.request</td>
         <td id='n_req'></td>
      </tr>
      <tr>
         <td class='thd'>n.response</td>
         <td id='n_res'></td>
      </tr>
   </table>
</body>
</html>
```

panel.html이 열린 상태에서 우하단의 Go Live 버튼을 클릭하여 Live server를 실행하면 Chrome 브라우저가 열립니다.

![](../../_assets/image_51.png)
<br></br>
아직 panel.js의 내용은 없지만, 레이아웃이 정상적인지는 확인할 수 있습니다.
![](../../_assets/image_52.png)




