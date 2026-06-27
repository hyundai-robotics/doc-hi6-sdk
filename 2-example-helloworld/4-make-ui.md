# 2.4 创建简单的基于网页的用户界面

可以添加基于网页的用户界面来设置使用教学挂件的应用程序。

在这个例子中，我们将创建一个简单的功能，它将在教学挂件屏幕上打印“Hello, world!”。

通过点击左上角的“新建文件夹”按钮创建一个新文件夹。将文件夹名称设置为ui。

![](../_assets/image_19.png)

在ui/文件夹中创建一个新文件，并命名为setup.html。结果如下图所示。

![](../_assets/image_20.png)

将以下代码写入setup.html。
```
<!DOCTYPE html:5>
<html>
 
<head>
    <title>hello, world - setup</title>
    <meta http-equiv=Content-Type content='text/html; charset=utf-8'>
</head>
 
<body>
   <div>
      <div id='contents'>
         <h1>你好，世界！</h1>
      </div>
   </div>
</body>
</html>
```

现在，将菜单项注入到教学挂件的系统 - 应用程序参数菜单下，用于打开此屏幕。

在ui/文件夹下创建一个menu.json文件。  
![](../_assets/image_21.png)

``` json
[
    {
        "path": "system/appl/",
        "id": "hello_world",
        "icon": "hello_world/ui/lm_hello.png",
        "label": "你好，世界",
        "url": "hello_world/ui/setup.html"
    }
]
```

每个项的含义如下。
<table>
  <thead>
    <tr>
      <th style="text-align:left">键</th>
      <th style="text-align:left">含义</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>path</td>
      <td>
       注入菜单项的菜单路径<br>(system/appl/ 意味着 "system/4: 应用程序参数。")
      </td>
    </tr>
   <tr>
      <td>id</td>
      <td>
       菜单项的ID 
      </td>
    </tr>
    <tr>
      <td>icon</td>
      <td>
       将在菜单屏幕上显示的项目图标的相对路径和文件名（基于apps/文件夹）
      </td>
    </tr>
    <tr>
      <td>label</td>
      <td>
       将在菜单屏幕上显示的项目名称
      </td>
    </tr>
    <tr>
      <td>url</td>
      <td>选择菜单项时要显示的html屏幕的相对路径和文件名（基于apps/文件夹）</td>
    </tr>
  </tbody>
</table>

即使没有指定图标，它的操作也将继续进行。不过，我们将创建一个图标并进行实践。

图标的格式应为包含透明信息的png文件，大小为104x104像素。

![](../_assets/lm_hello.png) lm_hello.png的例子（您可以下载并使用这张图片。）

<br>
使用Windows中的Paint创建透明背景的png文件是不可行的。我们推荐以下软件。

供您参考，我们在示例中在短短一分钟内使用COOLTEXT创建了图片。

Adobe Illustrator (https://www.adobe.com/kr/products/illustrator.html)：插图软件（商业）

GIMP (http://gimp.org)：Photoshop级别的图像编辑软件（免费）

Medibang Paint Pro (https://medibangpaint.com/pc/)：
易于使用的图形工具（免费）

COOLTEXT (https://cooltext.com/)：用于创建文本到徽标图像文件的网站（免费）


再次执行虚拟主板和虚拟教学挂件。


当进入[系统] - [应用程序参数]菜单时，您可以看到新添加的hello, world菜单项，如下所示。  
![](../_assets/image_22.png)

当您按下菜单时，教学挂件将显示setup.html屏幕，如下所示。

（最初，加载将需要大约一两秒。在后续实例中，缓存加载将更快。）

![](../_assets/image_23.png)