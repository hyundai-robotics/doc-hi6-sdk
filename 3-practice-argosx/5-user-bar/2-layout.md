# 3.5.2 Layout of the user bar

Open vscode for the apps/ folder that is the parent of the ArgosX folder.

Create ui/ubar.html and ui/ubar.js files.
<br>![](../../_assets/image_57.png)

Write the content below, which is about a simple layout consisting of two buttons. The first column of the table is given a class called 'ubar-bt', which is defined in the common style.css. It automatically recognizes TP600 and TP630 and gives the button’s sizes and colors similar to the default UI. If you want to change the style, you can define a class using a separate local css and apply it.

ubar.html
``` html
<!DOCTYPE html:5>
<!--
   @author: Jane Doe, BlueOcean Robot & Automation, Ltd.
   @brief: ArgosX Vision System interface - bar
   @create: 2021-12-07
-->
<html>
  
<head>
    <title>ArgosX</title>
    <link rel='stylesheet' href='../../_common/css/style.css' type=text/css rel=stylesheet>
    <script src='../../_common/js/jquery-3.6.0.min.js'></script>
    <script src='./ubar.js'></script>
</head>
  
<body class='ubar'>
   <button id='light-on' class='ubar-bt' onclick='light_onoff(true);'>light<br>on</button>
   <button id='light-off' class='ubar-bt' onclick='light_onoff(false);'>light<br>off</button>
</body>
</html>
```

While ubar.html is open, if you execute Live server by clicking the Go Live button at the bottom right, the Google Chrome browser will open.
<br>![](../../_assets/image_58.png)




Even though there is no content in ubar.js yet, we can check whether the layout is normal.
<br>![](../../_assets/image_59.png)