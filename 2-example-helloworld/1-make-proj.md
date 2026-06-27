# 2.1 创建 hello_world 项目 - 文件夹和元信息

在 apps/ 文件夹下创建一个名为 hello_world 的文件夹。文件夹名称应为项目名称，并且必须在 apps/ 文件夹中唯一。

在资源管理器中右键单击 hello_world/ 文件夹，然后在弹出菜单中单击“使用代码打开”。

![](../_assets/image_13.png)

随后，vscode 将以 hello_world/ 文件夹作为项目打开。您可以通过单击左上角的新文件来在该文件夹中创建新文件。文件名应为 info.json。

![](../_assets/image_14.png)

单击 info.json 打开它并输入以下内容。

![](../_assets/image_15.png)

#### info.json

``` json
{
   "author" : "HD Hyundai Robotics",
   "binding" : "plug-in",
   "copyright" : "版权所有",
   "description" : "第一个示例 - hello_world",
   "entry" : "hello_world.py",
   "menu" : "ui/menu.json",
   "startup" : "手动",
   "version" : "v0.9.0"
}
```

<br>
<table>
  <thead>
    <tr>
      <th style="text-align:left">Key</th>
      <th style="text-align:left">Meaning</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>`作者 (author)`</td>
      <td>
       作者
      </td>
    </tr>
   <tr>
      <td>`binding`</td>
      <td>
       与 ${cont_model} 主机软件的绑定形式<br>
       - 插件：将以绑定形式执行。<br>
       - 独立：将作为独立应用（进程）执行。
      </td>
    </tr>
    <tr>
      <td>`copyright`</td>
      <td>
       版权
      </td>
    </tr>
    <tr>
      <td>`说明 (description)`</td>
      <td>
       描述	
      </td>
    </tr>
    <tr>
      <td>`entry`</td>
      <td>
       执行开始位置的文件名称<br>
       名称必须在 apps/ 文件夹中唯一。如果可能，将其设置为以下名称之一<br>
       - {project name}.py<br>
       - {project name}_main.py
      </td>
    </tr>
    <tr>
      <td>`菜单 (menu)`</td>
      <td>
       用户界面的菜单结构	
      </td>
    </tr>
     <tr>
      <td>`启动 (startup)`</td>
      <td>
       执行启动模式
       - 手动：手动启动执行
       - 启动：在启动时自动开始执行	
      </td>
    </tr>
     <tr>
      <td>`version`</td>
      <td>
       版本字符串	
      </td>
    </tr>
  </tbody>
</table>