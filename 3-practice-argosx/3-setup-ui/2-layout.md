### 3.3.2 设置屏幕的布局

打开包含 ArgosX 文件夹的 apps/ 文件夹中的 vscode。

（执行此步骤是因为 ArgosX 用户界面需要引用 apps/_common/ 中的文件。包含 Live 服务器引用的所有文件的文件夹应作为工作区中的顶级文件夹打开。）
<br>
![](../../_assets/image_35.png)
<br>

通过单击新建文件夹按钮创建 ui/ 文件夹。
<br>
![](../../_assets/image_36.png)
<br>

创建 ui/setup.html 文件。
<br>
![](../../_assets/image_37.png)
<br>

将内容写为如下。 

setup.html
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
         <span class='col0' name='fail_out_sig'>故障输出信号</span>
         <input class='col1' type='text' id='fail_out_sig' size='5'/>
      </div>
   </div>
</body>
</html>
```
在打开setup.html时，点击右下角的Go Live按钮以运行Live服务器。
<br>
![](../../_assets/image_38.png)
<br>




如果出现关于vscode的安全警告，请勾选“允许通信”下的所有项目，然后点击“允许访问”按钮。
<br>
![](../../_assets/image_39.png)
<br>




或者，您可以通过右键点击setup.html打开弹出菜单，然后选择“用Live服务器打开”。
<br>
![](../../_assets/image_40.png)
<br>




当谷歌Chrome浏览器打开时，我们可以检查草图布局。



setup.html的草图布局
<br>
![](../../_assets/image_41.png)
<br>