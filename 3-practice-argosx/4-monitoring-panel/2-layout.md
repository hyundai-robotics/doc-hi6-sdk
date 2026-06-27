#### 3.4.2 监控面板的布局

打开vscode，进入ArgosX文件夹的父文件夹apps/。

创建ui/panel.html和panel.js文件。

![](../../_assets/image_50.png)

编写如下内容，这是由一个表格组成的简单布局。表格的第一列被赋予了一个名为'thd'（表头的缩写）的类，该类在common style.css中定义，采用黑色字符和灰色背景。如果您想更改样式，可以使用单独的本地css定义一个类并应用。

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
      <th>名称</th>
      <th>值</th>
      <tr>
         <td class='thd'>IP地址</td>
         <td id='ip_addr'></td>
      </tr>
      <tr>
         <td class='thd'>端口#</td>
         <td id='port'></td>
      </tr>
      <tr>
         <td class='thd'>错误代码</td>
         <td id='sigcode_err'></td>
      </tr>
      <tr>
         <td class='thd'>请求数</td>
         <td id='n_req'></td>
      </tr>
      <tr>
         <td class='thd'>响应数</td>
         <td id='n_res'></td>
      </tr>
   </table>
</body>
</html>
```

当panel.html处于打开状态时，如果通过点击右下角的“Go Live”按钮执行Live server，谷歌浏览器将打开。

![](../../_assets/image_51.png)
<br></br>
尽管panel.js中尚无内容，但我们可以检查布局是否正常。  
![](../../_assets/image_52.png)