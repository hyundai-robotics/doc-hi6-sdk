
[__SOURCE](README.md)
# ${cont_model} 控制器功能手册 - 软件开发工具包 (SDK)
[__SOURCE](0-about-this-manual/README.md)
# 关于手册
[__SOURCE](0-about-this-manual/precautions.md)
# 注意事项

{% include file="zh/precautions.md" %}
[__SOURCE](0-about-this-manual/safety-notice.md)
# 安全注意事项

{% include file="zh/safety-notice.md" %}
[__SOURCE](1-intro/README.md)
# 1. ${cont_model} SDK 概述

本手册描述了如何使用 SDK 开发插件应用程序，以开发 ${cont_model} 控制器的附加功能。

可以使用 SDK 开发的应用程序的功能如下。

您可以向机器人语言添加新的命令或对象类型。
您可以添加可以在各种事件中执行的操作，例如开机、电机开/关和开始/停止。
您可以添加可以定期执行的操作。
您可以添加用户定义的用户界面（UI），例如教导挂件的设置屏幕。
[__SOURCE](1-intro/1-prior-knowledge.md)
# 1.1 所需知识

使用 SDK 开发应用程序需要对以下技术具备基本熟练程度以上的了解。

如果您对以下技术不熟悉，建议首先使用适当的材料进行学习。

<table>
  <thead>
    <tr>
      <th style="text-align:left">所需技术</th>
      <th style="text-align:left">用途</th>
      <th style="text-align:left">教材</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>${cont_model} 控制器的基本使用方法</td>
      <td>
       操作机器人的基本知识
      </td>
      <td>${cont_model} 控制器操作手册</td>
    </tr>
   <tr>
      <td>HRScript 机器人语言编程</td>
      <td>
       HRScript 与应用程序之间的互联
      </td>
      <td>${cont_model} 控制器功能手册 - HRScript</td>
    </tr>
    <tr>
      <td>Python 3 编程</td>
      <td>
       应用操作的实现
      </td>
      <td>Python 教程或在线/离线培训程序</td>
    </tr>
    <tr>
      <td>Web 应用编程<br>
      (HTML5/CSS/JavaScript, jQuery)</td>
      <td>
       应用 UI 的实现	
      </td>
      <td>Web 开发教程或在线/离线培训程序<br>
      (开发没有 UI 的应用程序时不需要。)</td>
    </tr>
    <tr>
      <td>以太网用户数据协议 (UDP) 通信的基本概念</td>
      <td>
       理解 ArgosX 示例
      </td>
      <td>Python 教程中关于以太网套接字通信的章节</td>
    </tr>

  </tbody>
</table>
[__SOURCE](1-intro/2-plugin-app-concept.md)
# 1.2 ${cont_model} 插件应用的概念
一个 ${cont_model} 应用由在主模块中运行的 Python 3 脚本和执行教导挂件中 UI 操作的基于 JavaScript 的 Web 软件组成。

对于没有 UI 的应用，它们可能仅由 Python 脚本组成。

多个 Python 文件和 Web 应用文件将安装在主模块中的一个文件夹下。

#### Python 脚本
这些脚本可以通过特定的机器人控制器事件或定期调用。此外，可以使用 .job 文件中的机器人语言命令调用指定的 Python 函数。

通过名为 xhost 的模块，可以控制或监视机器人控制器的软件对象，xhost 通过 Python 接口或 OpenAPI 与软件对象进行交互。

#### 教导挂件 UI Web 应用
该 Web 应用与在 PC 或移动环境中使用的普通 Web 应用相同。它由超文本标记语言 (HTML)/层叠样式表 (CSS)/JavaScript 和各种图像等资源文件组成。

该 Web 应用存储在主模块中，将转移到教导挂件上以便在 Web 浏览器引擎上执行。

通过调用 OpenAPI，教导挂件 Web 应用可以调用 Python 函数并发送和接收数据。

![](../_assets/image_1.png)
[__SOURCE](1-intro/3-install-python.md)
# 1.3 安装 Python 3 开发环境
#### 安装 Python 3 
按照以下步骤安装 Python v3.8。

<br></br>
1) 以下链接将引导您进入 Python v3.8.0 安装页面。安装 x86 32位。

    <span style='background-color:#ffdce0'>(注意：由于 ${cont_model} 虚拟控制器是 32 位应用程序，因此 Python 运行时应与其匹配。请勿安装 x86-64)</span>

    https://www.python.org/downloads/release/python-380/

    ![](../_assets/image_2.png)

2) 勾选将 Python 3.8 添加到 PATH，然后选择自定义安装。  
    ![](../_assets/image_3.png)

3) 勾选所有选项，然后单击下一步。  
    ![](../_assets/image_4.png)
4) 勾选所有选项。保持安装路径为 C:\Program Files (x86)\Python38-32 并单击安装。  
    ![](../_assets/image_5.png)
5) 您不需要按下禁用路径长度限制。单击关闭。  
    ![](../_assets/image_6.png)

6) 打开 Windows 命令提示符（按 Windows + R，输入 cmd，然后按回车键）。

    输入 ```python --version``` 然后按回车键以检查是否打印出以下版本。

    ```
    Python 3.8.0
    ```

#### 添加 Python 导入搜索路径
1) 创建一个 .pth 文件并指定 _common/ 文件夹所在的路径。 
    在 SDK 中打开 .pth 文件并指定以下路径，以匹配 ${cont_model} 虚拟控制器的 HOME 路径。
 

    文件内容示例：    
    ```
    D:\${cont_model}\home_main\apps
    ```

2) 将编辑过的 .pth 文件部署到 Python 安装路径/Lib/site-packages/。

    示例：C:\Program Files (x86)\Python38-32\Lib\site-packages\.pth


#### 部署动态库
将 ucrtbased.dll 和 vcruntime140d.dll 从 SDK 部署到 Python 安装路径。

示例：复制到 C:\Program Files (x86)\Python38-32\。
[__SOURCE](1-intro/4-install-vscode.md)
# 1.4 安装 Visual Studio Code

Microsoft Visual Studio Code（以下简称 vscode）是一个功能强大的文本编辑器，提供免费使用。通过安装各种扩展，vscode 为多种编程语言提供了开发环境。

您可以使用其他熟悉的编辑器，如 Atom 或 SublimeText，但本手册提供的说明基于 vscode。

#### 安装代码
1) 访问以下链接，然后下载适用于 Windows 的稳定版。

    对于 Windows: https://code.visualstudio.com/<br>
    ![](../_assets/image_7.png)

2) 执行下载的安装文件并接受许可协议。

    ![](../_assets/image_8.png)

3) 在保留安装路径等默认值的情况下，继续按 Next 按钮。当出现选择附加任务的屏幕时，按如下所示勾选，然后单击 Next 按钮。
    
    ![](../_assets/image_9.png)

4) 单击 Install 按钮。

5) 安装完成后，尝试运行 vscode。

#### 安装扩展
这是 vscode 独特的优势，使得在市场上安装各种扩展成为可能。

活动栏是 vscode 屏幕最左侧的垂直栏，其中图标被排列。点击红色标记的图标将打开 EXTENSIONS: MARKETPLACE 屏幕，如图所示。当您在顶部的过滤窗口中输入所需扩展的名称时，可以轻松找到该扩展。

![](../_assets/image_10.png)

您需要安装的扩展如下。通过使用过滤器找到每个扩展，并使用 Install 按钮进行安装。

- 可能会有多个相同名称的扩展。通过检查作者的名称选择正确的扩展。
- 安装 Microsoft 的 Python 时，Pylance 等其他扩展将自动安装。Pylance 与我们应该使用的 Pyright 冲突。移除除 Python 之外的所有其他扩展。
- 当您编辑作业文件时，它应被识别为 HRScript，而不是 HR-BASIC。保持 HR-BASIC 禁用。

![](../_assets/image_11.png)

在开发过程中，除了 Python，您还将处理 html、css、javascript 和 json 扩展的文件。然而，由于这些格式的编辑功能已嵌入在 vscode 中，您无需单独安装扩展。

现在，您可以开始使用 vscode。建议通过参考 vscode 帮助菜单或在线讲座了解其使用方法。

![](../_assets/image_12.png)
[__SOURCE](1-intro/5-install-web-ui.md)
# 1.5 安装基于网络的 UI 开发环境

教导挂件的定制 UI 应作为 HTML5/CSS/jQuery 的网络应用程序形式进行开发。


#### 安装 Google Chrome 网络浏览器

Google Chrome 网络浏览器提供运行和调试网络应用程序的环境。如果您尚未安装该浏览器，请通过单击以下链接进行安装。

(Microsoft Edge 或 Mozilla Firefox 也提供几乎相同的功能。然而，本手册将基于 Google Chrome 提供说明。)

https://www.google.com/intl/ko/chrome/
[__SOURCE](1-intro/6-test-in-hrspace.md)
# 1.6 在 HRSpace 中测试插件

您可以在 HRSpace 的虚拟控制器和虚拟教学挂件上测试自己的插件应用程序。

{% hint style="warning" %}   
<b>与实际控制器环境存在差异。</b> 请仅在开发阶段进行简单的功能测试，并确保在真实环境中进行彻底测试后再应用插件。我们对未考虑此信息而导致的任何损坏或问题不承担任何责任。  
{% endhint %}  

<br>

#### 1.6.1 HRSpace 安装环境
- 操作系统：Windows 64-bit

<br>

#### 1.6.2 HRSpace 安装过程

1. 访问 HD Hyundai Robotics 网站，如未注册，请先注册。
2. 打开 [HRSpace 下载页面](https://www.hd-hyundairobotics.com/biz/product/support/291)。
3. 安装最新版本（截至文档日期：v3.95b10）。
4. 解压下载的 zip 文件。
5. 运行安装程序（HRSpace3.msi）→ 选择语言 → 选择安装位置 → 完成安装并退出。

<br>

#### 1.6.3 运行 HRSpace

#### a. 加载机器人模型
1. 按 `Windows 键` > 输入 `HRSpace3_eng` > 点击 > 运行程序。
2. 右键单击左侧工作区面板中的 `workspace component` > 点击 `Load Model as a Child...` > 点击 `机器人 (Robot)` 文件夹 > 选择所需模型。  
   
   <img src="../_assets/hrspace/01_select_robot_model.png" height="400hw">


   <p style="background-color: orange; color: black; width:max-content"><b>为了避免错误，只能加载位于 ${HRSpace installation folder}\VRC_${cont_model}\fbrr 中的模型。</b></p>

3. `Robot Controller (RC) Type Selection Popup` > 点击 VRC_${cont_model} > 确认 > 加载机器人。  

   <img src="../_assets/hrspace/03_robot_loaded.PNG" height=350hw>


#### b. 保存工作区
1. 
   <img src="../_assets/hrspace/00_save_btn.PNG" height=50vw> 点击任务栏上的 `保存 (Save)`。

2. 在所需位置创建新文件夹并保存文件。  
示例：点击文件资源管理器地址栏中的 "HRSpace3" 进行导航 > 右键单击空白处 > 新建 > 文件夹 > 创建临时文件夹 > 创建模型为 temp.hrs > 保存后，保存的文件名将在标题栏中显示。 > 点击任务栏上的 `保存 (Save)`。   
   <img src="../_assets/hrspace/04_temp_hrs.PNG" height=150vw>  


#### c. 运行虚拟教学挂件

1. 右键单击左侧工作区面板中创建的机器人模型 > 点击虚拟教学挂件。  
   <img src="../_assets/hrspace/05_start_virtual_tp.PNG" height=500vw>

   <img src="../_assets/hrspace/06_tp_imp.PNG" height=500vw>

2. 退出教学挂件的两种方式

   1. TP Home - `[F1: 服务] - 9: 退出TP应用程序 ([F1: Service] - 9: Exit TP application)`
    
   2. 右键单击键盘区域 - 点击 `关闭 (close)`

<br>

#### 1.6.4 在虚拟教学挂件上运行插件
1. 创建一个 hello-world 示例插件并将其注入到 HRSpace 的虚拟教学挂件中。此过程参考了 [HRBook 手册](../2-example-helloworld/README.md)。
   ```text
   hello_world
    ├── cmds.json
    ├── hello_world.py
    ├── info.json
    └── ui
        ├── lm_hello.png
        ├── menu.json
        └── setup.html
   ```

1. 将开发的 hello-world 插件保存到以下路径。
   ```text
   ${HRSpace installation folder}\VRC_${cont_model}\apps
   ex) C:\Program Files\HHI Robotics\HRSpace3\VRC_${cont_model}\apps
   ```


3. 检查保存的插件位置。  

   <img src="../_assets/hrspace/07_saved_hello_world.PNG" height=200vw>



4. 退出虚拟教学挂件 > 重启 > TP Home > `[F2: 系统] - 4: 应用参数 ([F2: system] - 4: Application parameter)` - 检查 `hello, world` 插件   

   <img src="../_assets/hrspace/08_hello_world_menu.PNG" height=500vw>  

5. 执行 `hello, world` 插件。

   <img src="../_assets/hrspace/09_hello_world_menu_success.PNG" height=500vw>

<br>

#### 1.6.5 注意事项
1. 如果您修改 HTML、CSS 或 JavaScript 代码，只需返回 TP Home 屏幕并重新进入插件以使更改生效。
2. <p style="background-color:darkslategrey; color:white;width:max-content">如果您在插件运行时修改 Python 代码，则必须重启虚拟控制器以使更改生效。</p>  
   → 右键单击工作区面板中的机器人 > 点击 `VRC Tools...` > 点击 `Reboot`   
   <br>
   <img src="../_assets/hrspace/10_vrc_tools.PNG" height=150vw>
   <img src="../_assets/hrspace/11_vrc_tools_complete.PNG" height=150vw>  

3. 重启虚拟控制器后，重新运行 `1.6.4 在虚拟教学挂件上运行插件`。
[__SOURCE](2-example-helloworld/README.md)
# 2. 非常简单的项目：hello_world
跳到元数据的结尾
由 Wonhyeok Choi 创建，最后修改于 2021 年 12 月 24 日
跳到元数据的开始
让我们开始创建一个应用程序，作为一个非常简单的项目。

这个名为 hello_world 的应用程序的功能是在历史屏幕和设置屏幕上打印字符串 Hello, world!。



创建 hello_world 项目 - 文件夹和元信息
实现 Python 函数 hello( )
将参数传递给 Python 函数并接收返回值
创建一个简单的基于网页的 UI
[__SOURCE](2-example-helloworld/1-make-proj.md)
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
[__SOURCE](2-example-helloworld/2-make-hello.md)
# 2.2 实现 python 函数 hello( )

使用新建文件按钮创建一个新文件，并将其命名为 hello_world.py。  
![](../_assets/image_16.png)

您可以如下编写 hello_world.py。

```python
import xhost
 
def hello():
    print("Hello, world!")
    xhost.printh("Hello, world!")
```

- 如果您熟悉 python 编程，您可能知道 def 下面的行应该用制表符缩进。
- xhost 是一个调用主机（机器人控制器）函数的模块。您不必自己编写 xhost.py 文件。后面的部分将提供详细的说明，因此现在您只需要了解这些即可。
<br></br>

现在，依次执行 ${cont_model} 虚拟控制器和 TP。

在启动时，${cont_model} 控制器通过读取 apps/ 文件夹下所有文件夹中的 info.json 来识别已安装的应用程序。

[service] - 10: 点击应用程序将出现一个名为 10: app - TP 的屏幕。

双击 `[location]` 按钮将通过 USB 将标题中的 TP 更改为 MAIN。在此屏幕上，可以找到之前创建的 hello_world。

![](../_assets/image_17.png)

我们将在机器人语言中执行 hello_world，因此请按 ESC 键退出屏幕。

现在，使用 HRScript 创建一个作业程序。按如下方式进行教学过程。

```
import hello_world
hello_world.hello()
end
```

在保持先前屏幕面板打开并开启电机的同时，如果您使用 Step FWD 或 START 按钮执行，则将打印 Hello, World!

```
15:05:20.894 ( 968) .import hello_world

15:05:20.894 ( 0)_____[STOP for CmdMode]

15:05:25.413 (4518890)_____[START:StepFwd]__(P2/S0./F1)___

15:05:25.415 Hello, world!

15:05:25.416 ( 2968) .var ret=hello_world.hello()

15:05:25.417 ( 1024)_____[STOP for CmdMode]

15:05:32.157 (6740100)_____[START:StepFwd]__(P2/S0./F2)___

15:05:32.158 ( 970) .end
```

<span style='background-color:#ffdce0'>
注意：如果您修改 Python 程序，则应运行虚拟控制器以反映修改。
</span>
[__SOURCE](2-example-helloworld/3-function.md)
# 2.3 将参数传递给 Python 函数并接收返回值

现在，让我们将两个参数，姓名和年龄，应用于 Python 函数。通过如下修改代码添加 introduce( ) 函数。

```python
import xhost
 
def hello():
    print("Hello, world!")
    xhost.printh("Hello, world!")
 
 
def introduce(name: str, age: int) -> str:
    msg = f"Hello, {name}! You are {age} years old."
    return msg
```

- :str, :int 和 -> str 在 introduce 函数中是称为类型提示的语法元素，从 Python 3.5 开始引入。省略类型提示不会影响操作。虽然类型提示在执行脚本时并没有角色，但它为 vscode 扩展（例如 Pyright）提供了检查其语法的信息。因此，类型提示有助于在 Python 编码时防止语法错误 (https://docs.python.org/3.8/library/typing.html.)

在一个工作程序中，您需要额外教它调用 introduce( )。

```
import hello_world
hello_world.hello()
var msg=hello_world.introduce("Steve", 25)
print msg
end
```

如果以下字符串在教学挂件的指导框中打印出来，则意味着操作正常。  
![](../_assets/image_18.png)

如示例所示，HRScript 的字符串值和整数值自然地作为 Python 的字符串值和整数值传递。相反，Python 的字符串返回值也自然返回为 HRScript 的字符串值。两种语言的值以这种方式自动相互转换。下面的表格是两种语言的数据类型映射。

<table>
  <thead>
    <tr>
      <th style="text-align:left">HRScript</th>
      <th style="text-align:left">Python</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>bool</td>
      <td>
       bool
      </td>
    </tr>
   <tr>
      <td>number - integer</td>
      <td>
       long 
      </td>
    </tr>
    <tr>
      <td>number - real</td>
      <td>
       float
      </td>
    </tr>
    <tr>
      <td>string</td>
      <td>
       str
      </td>
    </tr>
    <tr>
      <td>array</td>
      <td>tuple</td>
    </tr>
    <tr>
      <td>object</td>
      <td>
       dictionary	
      </td>
    </tr>
     <tr>
      <td>xpy-object</td>
      <td>object</td>
    </tr>
  </tbody>
</table>

- 当一个在 Python 中创建的对象传递到 HRScript 时，被特别称为 xpy-object。对于 xpy-object，可以读取属性值并调用方法。我们将在后面更详细地讨论它们。
- 对于 Python 语法，可以从函数传递多个返回值到外部，但不能传递到 HRScript。
[__SOURCE](2-example-helloworld/4-make-ui.md)
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
[__SOURCE](3-practice-argosx/README.md)
# 3. 实际项目 : ArgosX
[__SOURCE](3-practice-argosx/1-roblang/README.md)
# 3.1 实际项目：ArgosX - Robot language

现在让我们通过一个更现实的示例项目进行练习。

我们将学习如何通过为名为 ArgosX 的虚拟视觉系统开发插件来开发一个应用程序，该系统将安装在机器人上。

- ArgosX 和接口插件的规格<br>
- ArgosX 框架<br>
- 创建 ArgosX 项目<br>
- 创建 ip_addr 和 port 属性<br>
- 为 ArgosX 机器人语言创建函数<br>
- 实现 ArgosX 机器人语言的函数<br>
- 调用 xhost 模块的方法<br>
- 参考 xhost 模块方法的手册<br>
- 解决机器人语言功能阻塞问题<br>
[__SOURCE](3-practice-argosx/1-roblang/1-concept-interface.md)
# 3.1.1 ArgosX 规格和接口插件

#### ArgosX 视觉系统的规格


##### 基本规格
<table>
  <thead>
    <tr>
      <th style="text-align:left">项目</th>
      <th style="text-align:left">描述</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>功能</td>
      <td>
       - 它包含一个嵌入式LED灯，可以通过通信请求打开和关闭。<br>
       - 它可以同时测量最多100个工件的位移值，并在响应通信请求时报告。
      </td>
    </tr>
   <tr>
      <td>通信接口</td>
      <td>
       - 机器人控制器和ArgosX硬件通过以太网UDP通信相互通信。<br>
        - ArgosX硬件的IP地址是192.168.1.XX。最后一组数字XX应使用拨码开关设置，机器人一侧应相应地发送UDP请求。<br>
        - ArgosX硬件的端口号固定为54321。然而，在未来的产品中可能会发生更改。<br>
        - 在接收到UDP请求后，ArgosX硬件将向发送者的IP地址发送响应。
      </td>
    </tr>
    <tr>
      <td>可以安装的系统数量</td>
      <td>
       - 在机器人控制器中只能安装一个ArgosX系统。换句话说，ArgosX系统在软件上是唯一实例。
      </td>
    </tr>
  </tbody>
</table>

##### 协议
<table>
  <thead>
    <tr>
      <th style="text-align:left">传输方向</th>
      <th style="text-align:left">端口#</th>
      <th style="text-align:left">语法和示例</th>
      <th style="text-align:left">含义</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>机器人 → ArgosX</td>
      <td>54321</td>
      <td>req {workpiece#}<br>
        例如 "req 39"</td>
      <td>请求工件#的位移值<br>工件编号（#）范围从1到100</td>
    </tr>
   <tr>
      <td>机器人 ← ArgosX</td>
      <td></td>
      <td>res ({x}, {y}, {z}, {rx}, {ry}, {rz})<br>
            如果测量失败，字符串“fail”将被传送。<br>
            例如 "res (30, 25.7, 11.9, 31.6, 12.8, -54.6)"<br>
            例如 "fail"</td>
      <td>关于工件#的位移值的响应<br>
        x-rz的值是真实数，单位为mm和deg。</td>
    </tr>
    <tr>
      <td>机器人 → ArgosX</td>
      <td>54321</td>
      <td>light-on</td>
      <td>打开LED灯。</td>
    </tr>
    <tr>
      <td>机器人 → ArgosX</td>
      <td>54321</td>
      <td>light-off</td>
      <td>关闭LED灯。</td>
    </tr>
  </tbody>
</table>

#### ArgosX 接口插件的规格


ArgosX的接口插件将按照以下规格开发。



##### 机器人语言
<table>
  <thead>
    <tr>
      <th style="text-align:left">项目</th>
      <th style="text-align:left">语法</th>
      <th style="text-align:left">描述</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>模块</td>
      <td>argosx</td>
      <td></td>
    </tr>
   <tr>
      <td rowspan="2">属性</td>
      <td>ip_addr</td>
      <td>ArgosX硬件的IP地址字符串（可以设置。）<br>例如 "192.168.1.44"</td>
    </tr>
    <tr>
      <td>port</td>
      <td>ArgosX硬件的端口号。<br>（设置应该是可能的，因为未来的产品可能会有更改。）<br>例如 54321</td>
    </tr>
    <tr>
      <td rowspan="4">功能</td>
      <td>init( )</td>
      <td>初始化UDP通信的套接字。</td>
    </tr>
    <tr>
      <td>req({workpiece#})</td>
      <td>请求工件#的结果位移值</td>
    </tr>
    <tr>
      <td>res( )</td>
      <td>在等待响应时接收请求。<br>返回值是基于基坐标系统的位移数组字符串。<br>例如 "[30, 25.7, 11.9, 31.6, 12.8, -54.6, \"base\"]"</td>
    </tr>
    <tr>
      <td>close( )</td>
      <td>关闭UDP通信的套接字。</td>
    </tr>
  </tbody>
</table>

##### 照明功能
- 当机器人处于电机开启状态时，ArgosX LED灯也会亮起。
- 当机器人处于电机关闭状态时，ArgosX LED灯也会熄灭。

##### 错误处理
- 当从ArgosX接收到“fail”时，机器人控制器对应于预设编号的通用I/O输出信号将被打开。


##### 监控
通过在教学挂架上打开ArgosX监控面板，可以看到以下信息。

- IP地址
- 端口#
- 错误输入分配号
- 请求计数
- 响应计数



##### 用户栏
当您在教学挂架上打开ArgosX用户栏时，将提供如下所示的UI。

- 开灯按钮：打开ArgosX LED灯。
- 关灯按钮：关闭ArgosX LED灯。
[__SOURCE](3-practice-argosx/1-roblang/2-argosx-stub.md)
#### 3.1.2 ArgosX stub

ArgosX 视觉系统并不真实。因此，如果我们想要测试接口插件，我们需要测试软件，也就是 stub，来代替 ArgosX。

下面的 Python 代码是 ArgosX stub。您无需理解其实现的细节。

argosx_stub.py
``` python
"""ArgosX stub
ArgosX 接口插件的测试替代品
 
 
@author:    Jane Doe, BlueOcean Robot & Automation, Ltd.
@created:   2021-12-06
@version:   v1.0
"""
 
import time
from typing import Optional, Union, Dict
import socket
 
 
# const
buf_size = 0x8000    # 32kb ; 允许的包长度
port_no = 54321      # ArgosX 命令的端口
sleep_sec = 0        # 响应前的延迟
 
# global variables
inaddr_any : str = ""
ip_port_of_req = ("", 0)
sock : Optional[socket.socket] = None
 
# test samples
test_shifts : Dict[str, str]= {
   "5" : "(30, 25.7, 11.9, 31.6, 12.8, -54.6)",
   "39" : "(9, 15.5, 10.3, 11.2, 19.2, 1.3)",
   "98" : "fail",
   "else" : "(0, 0, 0, 0, 0, 0)"
}
 
 
# functions
def init():
   """
   初始化服务器
   """
   global sock
   try:
      sock = socket.socket(family=socket.AF_INET, type=socket.SOCK_DGRAM)
      sock.bind((inaddr_any, port_no))
   except socket.error as e:
      print("套接字创建或绑定错误 :", e)
      return -1
   return 0
 
 
def close():
   """
   关闭服务器
   """
   if sock is None: return
   sock.close()
 
 
def do_service():
   """
   执行服务循环
   """
   state = 0
    
   while(True):
      msg = recv_msg()
      msg = msg.strip('\x00')
      ret = do_service_sub(msg)
      if ret==1:
         break
 
 
def recv_msg() -> str:
   """
   接收 UDP 消息 (阻塞)
   发送者的 IP 地址和端口存储在 ip_port_of_req
   Returns:
         接收到的消息     例如 "req 39"
   """
   global ip_port_of_req
   if sock is None: return ""
   data, ip_port_of_req = sock.recvfrom(buf_size)
   bts = bytearray(data)
   msg = bts.decode()
   return msg
 
 
def do_service_sub(msg: str) -> int:
   """
   执行服务子程序
   Args:
         msg   例如 "req 39"
   Returns:
         1     退出服务
         0     继续服务
         -1    无效命令  
   """
   print('')
   print('请求 : ', msg)
   strs = msg.split()
       
   n_str = len(strs)
   if n_str < 1:
      return -1
    
   cmd = strs[0]
   param = ""
   if n_str >= 2:
      param = strs[1]
    
   if cmd=="req":
      time.sleep(sleep_sec)
      do_service_req(param)
   elif cmd=="light-on":
      print('LED 灯已开启')
   elif cmd=="light-off":
      print('LED 灯已关闭')
   elif cmd=="quit":
      return 1
   else:
      print('无效命令')
      return -1
   return 0
 
 
def do_service_req(param: str) -> int:
   """
   执行 req 服务
   Args:
      param    work#    "1"~"100"
 
   Returns:
         -1    无套接字
         >=0   发送的字节数
   """
   if sock is None: return -1
   res_value = ""
   try:
      res_value = test_shifts[param]
   except:
      res_value = test_shifts["else"]
   msg = "res " + res_value
   print('响应: ', msg)
   bts = bytearray(str.encode(msg))
   return sock.sendto(bts, ip_port_of_req)
    
 
# -----------------------------------------------
# main
print('***** ArgosX stub v1.0 *****')
iret = init()
if iret < 0:
   quit()
print('inaddr_any, port_no=%d' % port_no)
print('服务器已启动...')
do_service()
print('关闭中...')
close()
print('...服务器已结束')
```

复制上述内容，并在 hello_world/ 文件夹中创建一个 argosx_stub.py 文件。接下来，使用 Windows PowerShell 或命令提示符进入 hello_world/ 文件夹，然后使用以下命令执行它。

```
python argosx_stub.py
```

或者，如果您打开 vscode 并按 F5，执行将在调试模式下进行。

- 而不是在打开的 vscode 中打开 argosx/ 项目，您需要通过执行另一个 vscode 会话来打开该项目。
- 虽然调试配置列表可能会先打开，如下所示，您只需选择 Python File 项目。

![](../../_assets/image_24.png)

结果将在底部的 TERMINAL 窗口中打印。您可以通过操作右上角的 ![](../../_assets/image_25.png) 按钮来暂停、恢复或停止调试。

![](../../_assets/image_26.png)
[__SOURCE](3-practice-argosx/1-roblang/3-make-proj-argosx.md)
#### 3.1.3 创建 ArgosX 项目

在 apps/ 文件夹下创建一个 ArgosX 文件夹。

在资源管理器中右键单击 argosx/ 文件夹，并在弹出菜单中点击 “使用代码打开”。

这将用 argosx/ 文件夹作为项目打开 vscode。按照如下方式在 argosx/ 文件夹下创建 info.json。

info.json

```json
{
   "author" : "BlueOcean Robot & Automation, Ltd.",
   "binding" : "plug-in",
   "copyright" : "All right reserved",
   "description" : "ArgosX vision system interface",
   "entry" : "main.py",
   "menu" : "ui/menu.json",
   "startup" : "manual",
   "version" : "v0.9.0"
}
```


首先，在 argosx/ 文件夹下创建一个 main.py 文件（您可以在 @author 中写上您的名字。）

main.py
```python 
""" ArgosX Vision System interface - main
 
 
@author:    Jane Doe, BlueOcean Robot & Automation, Ltd.
@created:   2021-12-06
"""
```

然后，教一个作业文件，如下所示，执行相关测试。

```
import argosx
end
```
[__SOURCE](3-practice-argosx/1-roblang/4-make-attribute.md)
#### 3.1.4 创建 ip_addr 和 port 属性

当查看 <u>3.1.1 ArgosX 的规范和接口插件</u> 时，您可以找到一个用于指定 ip_addr 的字符串属性。

将全局变量和 attr_names( ) 函数添加到 argosx_main.py 文件中，如下所示。

我们可以在 Python 程序中定义和使用许多全局变量。然而，只有在 attr_names( ) 返回的元组中列出的名称才会作为 ArgosX 模块属性暴露在 HRScript 中。对于暴露为属性的变量，应定义以 get_ 和 set_ 为前缀的 getter 和 setter 函数。

在本示例中，让我们定义两个全局变量，ip_attr 和 port，然后将它们暴露为属性，并允许在 HRScript 中读取和写入它们。

在 argosx/ 项目文件夹中创建一个新的 setup.py 文件，然后按照如下实现。

setup.py
``` python 
""" 机器人应用程序 - argosx - setup
 
 
@author:    Jane Doe, BlueOcean Robot & Automation, Ltd.
@created:   2021-12-06
"""
 
 
 
# 属性 getter/setter
def get_ip_addr() -> str:
   return ip_addr
 
def set_ip_addr(addr: str):
   global ip_addr
   ip_addr = addr
 
def get_port() -> int:
   return port
 
def set_port(_port: int):
   global port
   port = _port
 
ip_addr : str = "192.168.1.100"
port : int = 54321
```

将整个 setup 模块导入 main.py，以便可以从 HRScript 调用 getter 和 setter 函数，然后定义返回属性名称元组的 attr_name( ) 函数。

（主机将 argosx/ 文件夹作为一个包处理。当从主模块导入 setup 模块时，您应显式在 setup 前面添加 .（句点），这意味着同一文件夹。）

main.py
```python 
""" ArgosX 视觉系统接口 - main
 
 
@author:    Jane Doe, BlueOcean Robot & Automation, Ltd.
@created:   2021-12-06
"""
 
from .setup import *
 
 
def attr_names() -> tuple:
   """返回要暴露的属性名称。"""
   return ("ip_addr", "port")
```

测试作业程序应按如下方式进行教学和执行。
```
import argosx
print argosx.ip_addr
print argosx.port
end
```

如果 Python 全局变量的值按顺序显示在指导框中，如下所示，则意味着操作正常。

```
192.168.1.100

54321
```

我们将测试 ArgosX 与同一台 PC 上的 argosx_stub 之间的通信。让我们将 ip_addr 的值更改为您的 PC 的 IP 地址，并检查是否已更改。

更改并执行作业程序，如下所示。

```
import argosx
print argosx.ip_addr
print argosx.port
argosx.ip_addr="192.168.1.172" # 您自己的 PC 的 IP 地址
print argosx.ip_addr # 重新检查
end
```

检查在指导框中是否打印出新分配给 ip_addr 的值。
```
192.168.1.172
```
[__SOURCE](3-practice-argosx/1-roblang/5-make-roblang.md)
#### 3.1.5 为 ArgosX 机器人语言创建函数

接下来要实现的规格是 init( )、req( )、res( ) 和 close( ) 函数。

(请参阅 <u>3.1.1 ArgosX 及接口插件的规格</u> 中的协议和机器人语言的函数。)

现在，让我们为每个函数执行一个打印操作。

在 argosx/ 文件夹下创建 roblang.py 文件，如下所示。

roblang.py (用于测试)
``` python 
""" ArgosX 视觉系统接口 - 机器人语言
 
 
@author:    Jane Doe, BlueOcean Robot & Automation, Ltd.
@created:   2021-12-06
"""
 
 
# 函数
def init() -> int:
   print("init()")
   return 0
 
 
def close():
   print("close()")
 
 
def req(work_no: int) -> int:
   msg = "req(" + str(work_no) + ")"
   print(msg)
   return 0
 
 
def res():
   print("res()")
   return "data"
```

将 roblang.py 中的所有名称导入到 main.py 中。

main.py
```python

""" ArgosX 视觉系统接口 - 主
 
 
@author:    Jane Doe, BlueOcean Robot & Automation, Ltd.
@created:   2021-12-06
"""
 
 
from .roblang import *
from .setup import *
 
 
def attr_names() -> tuple:
   """返回要暴露的属性名称。"""
   return ("ip_addr", "port")

```

作业文件
```
var iret
import argosx
 
print argosx.ip_addr
print argosx.port
argosx.ip_addr="192.168.1.172" # 您自己的计算机名称
print argosx.ip_addr # 重新检查
 
iret=argosx.init() # 初始化套接字
if iret<0
  print "init error"
  stop
endif
 
iret=argosx.req(39) # 传输请求
if iret<0
  print "req error"
  stop
endif
 
var str=argosx.res() # 等待响应
print str
 
argosx.close() # 关闭套接字
end
```

重启虚拟控制器并执行作业文件。如果文件创建正常，则将在虚拟控制器控制台打印以下结果。这是从 HRScript 调用的 Python 函数。
```
192.168.1.172

init()

req(39)

res()

data

close()
```
[__SOURCE](3-practice-argosx/1-roblang/6-roblang_func.md)
#### 3.1.6 为 ArgosX 机器人语言实现功能

现在让我们实现每个功能的实际操作。

UDP 客户端通信的操作可能会在后面的部分使用。在这里，我们将操作模块化为一个单独的 .py 文件。

向项目中添加一个 comm.py 文件，并按照如下所示编写以下操作。

这些是简单的操作，例如初始化 UDP 套接字、发送、接收和关闭字符串消息。

- 虽然 xhost 是调用主机（机器人控制器）功能的模块，但主软件使其动态（没有名为 xhost.py 的文件。）后续部分将提供详细解释，因此现在您只需了解这些即可。

comm.py
``` python 
""" ArgosX 视觉系统接口 - comm.
 
 
@author:    Jane Doe, BlueOcean Robot & Automation, Ltd.
@created:   2021-12-10
"""
 
from typing import Optional
import socket
import xhost
 
 
# 全局变量
raddr : tuple
sock : Optional[socket.socket] = None
buf_size = 0x8000    # 32kb ; 允许的包长度
 
 
def is_open() -> bool:
   """
   返回:
         True     套接字已打开
         False    套接字未打开
   """
   return (sock is not None)
 
 
def open(ip_addr: str, port: int) -> int:
   """
   打开用于 UDP 通信的套接字
   参数:
      ip_addr     远程的 IP 地址，例如 "192.168.1.172"
      port        远程的端口号。例如 "192.168.1.172"
 
   返回:
         0     正常
         -1    错误
   """
   global raddr, sock
   if sock is not None: return -1
   try:
      raddr = (ip_addr, port)
      sock = socket.socket(family=socket.AF_INET, type=socket.SOCK_DGRAM)
   except socket.error as e:
      print("套接字创建或绑定错误 :", e)
      return -1
   logd('comm.open: ' + str(raddr))
   return 0
 
 
def close() -> None:
   global sock
   if sock is None: return
   logd('comm.close')
   sock.close()
   sock = None
 
 
def send_msg(msg: str) -> int:
   """
   通过 sock, raddr 发送消息
   参数:
      msg
 
   返回:
         >=0   发送的字节数
         -1    没有套接字。应该调用 init()。
   """
   if sock is None: return -1
 
   logd('request : ' + msg)
   bts = bytearray(str.encode(msg))
   return sock.sendto(bts, raddr)
 
 
def recv_msg():
   """
   等待从 sock 接收消息
   返回:
      接收到的字符串
   """
   if sock is None: return ""
 
   try:
      data, ip_port = sock.recvfrom(buf_size)
      bts = bytearray(data)
      msg = bts.decode()
      logd('response: ' + msg)
      return msg
   except Exception as e:
      print('来自 recv_msg() 的异常: ' + str(e))
      return ""
 
 
def logd(text: str):
   print(text)
   xhost.printh(text)
```

通过导入 comm 模块，您可以简单地实现将在机器人语言中调用的各个功能。

您还需要导入 setup 模块，因为引用 ip_addr 和 port 值是必要的。

get_base_shift_array_from_res() 是一个将从 ArgosX 接收的偏移字符串转换为将被解释为 HRScript 的 shift() 函数的格式的函数。

roblang.py
``` python 
""" ArgosX 视觉系统接口 - 机器人语言
 
 
@author:    Jane Doe, BlueOcean Robot & Automation, Ltd.
@created:   2021-12-06
"""
 
from . import comm
from . import setup
 
 
# 函数
def init() -> int:
   """
   初始化用于 UDP 通信的套接字
 
   返回:
         0     正常
         -1    错误
   """
   return comm.open(setup.ip_addr, setup.port)
 
 
def close():
   """
   关闭套接字
   """
   comm.close()
 
 
def req(work_no: int) -> int:
   """
   向 ArgosX 发送请求命令
   例如 "req 39"
   参数:
      work_no     工作编号    1~100
 
   返回:
         >=0   发送的字节数
         -1    没有套接字。应该调用 init()。
   """
   msg = "req " + str(work_no)
   return comm.send_msg(msg)
 
 
def res() -> str:
   """
   等待 ArgosX 的响应
   返回:
      来自 ArgosX 的响应字符串。
      失败时返回 ""。
      例如 "[30, 25.7, 11.9, 31.6, 12.8, -54.6]"
   """
   msg = comm.recv_msg()
   print(msg)
   msg = get_base_shift_array_from_res(msg)
   return msg
 
 
def get_base_shift_array_from_res(msg: str):
   """
   从响应字符串获取基本偏移数组符号字符串
   参数:
      msg   例如 "res (30, 25.7, 11.9, 31.6, 12.8, -54.6)"
    
   返回:
      例如 '[30, 25.7, 11.9, 31.6, 12.8, -54.6, "base"]'
   """
   tmp = msg.strip('res ')
   tmp = tmp.replace('(', '[')
   tmp = tmp.replace(')', ', "base"]')
   return tmp
```

作业应按如下方式修正。


```
Hyundai 机器人作业文件; { version: 1.6, mech_type: "780(YL012-0D)", total_axis: 6, aux_axis: 0 }
     var iret
     import argosx
      
     print argosx.ip_addr
     print argosx.port
     argosx.ip_addr="192.168.1.172" # 您自己的 PC 名称
     print argosx.ip_addr # 重新检查
      
     iret=argosx.init() # 初始化套接字
     if iret<0
       print "初始化错误"
       stop
     endif
      
     iret=argosx.req(39) # 传输请求
     if iret<0
       print "请求错误"
       stop
     endif
      
     var str=argosx.res() # 等待响应
     print str
     var sft=Shift(str) # 将偏移数组字符串转换为偏移数据
     print sft.x, sft.y, sft.z, sft.rx, sft.ry, sft.rz
 
     argosx.close() # 关闭套接字
     end
```
<br></br>
首先，从命令提示符或 vscode 执行 argosx_stub。

重新启动虚拟控制器并执行作业文件。如果文件正常创建，将发生以下操作。



<U>__argosx_stub 端（充当 ArgosX 的服务器）__ </U>

每次执行 argosx.req( ) 时，控制台上将打印以下字符串。
```
request : req 39
response: res (9, 15.5, 10.3, 11.2, 19.2, 1.3)
```

<U>__argosx 接口插件端（客户端）__</U>

每次最后一个打印命令执行时，教导挂件的指导框将打印以下内容。
```
9.000000 15.500000 10.300000 11.200000, 19.200000 1.300000
```
[__SOURCE](3-practice-argosx/1-roblang/7-xhost-call.md)
#### 3.1.7 调用 xhost 模块方法

xhost 是一个包含各种方法的模块，用于调用主机（机器人控制器）的功能。

主要模块虚拟控制器将创建 xhost 并将其注入到 Python 运行时中。您可以通过导入 xhost 来使用它，无需自己编写 xhost.py 文件。



请参阅 <U>3.1.8 手动，参考 xhost 模块的方法</U>。



在 <U>3.1.1 ArgosX 规格和接口插件的说明</U> 中还有一个关于错误处理的项目。

- 当从 ArgosX 收到“fail”时，相应于预设号的机器人控制器的通用 I/O 输出信号将被打开。


机器人控制器的通用 I/O 输出信号可以使用以下方法开/关。
``` python 
def io_set_out_bit(sigcode: int, val: int) -> int
```

sigcode 是一个将块号和 I/O 索引合并为一个数字的代码，如下所示。

sigcode = 块号 x 10000 + 索引
<br></br>

例如，fb3.do72 的 sigcode 如下所示。

3 x 10000 + 72 = 30072
<br></br>



如果 val 为 1 则为开启，0 则为关闭。
<br></br>

将分配给 ArgosX 错误的输出信号编号作为名为 sigcode_err 的模块变量添加，并将其默认值设置为 5（即 fb0.do5）。

（我们也可以将其声明为属性，以便在 HRScript 中进行修改。但在此示例中将跳过。）

setup.py
```python 
..previous steps skipped
ip_addr : str = "192.168.1.100"
port : int = 54321
sigcode_err = 5
```

在 res( ) 函数中接收到的 msg 值将与“res fail”进行比较，并根据结果传输输出信号。
<br></br>


roblang.py
```python
.. previous steps skipped
  
  
import xhost
  
  
...skipped
 
 
def res() -> str:
   """
   等待来自 ArgosX 的响应
   返回：
      来自 ArgosX 的响应字符串
      "" 如果失败。
      例如"[30, 25.7, 11.9, 31.6, 12.8, -54.6]"
   """
   val = 0
   msg = comm.recv_msg()
   print(msg)
   if msg=="res fail":
      val = 1
      msg = ""
   else:
      msg = get_base_shift_array_from_res(msg)
   xhost.io_set_out_bit(setup.sigcode_err, val)
   return msg
```

执行虚拟控制器时，同时保持教学挂件的通用输出面板打开，执行工作程序。

因为没有失败，操作将与之前相同，fb0.do5 打印信号将不会被打开。

argosx_stub.py 设计为在请求工作 #98 时无条件响应失败。修改工作以便能够执行 req(98)，如下所示，然后再次进行实现。



job
```
...Previous steps skipped
 
 
     iret=argosx.req(98) # 发送请求
     if iret<0
       print "req error"
       stop
     endif
      
     var str=argosx.res() # 等待响应
     print str
     if str==""
       print "req error"
       stop
     else
       var sft=Shift(str) # 将移位数组字符串转换为移位数据
       print sft.x, sft.y, sft.z, sft.rx, sft.ry, sft.rz
     endif
 
     argosx.close() # 关闭套接字
     end
```

如果在执行 res( ) 时 fb0.do5 打印信号被打开，则表示错误信号已正常打印。

![](../../_assets/image_27.png)




信号 #5 应仅用于 ArgosX 错误。因此，不能用于其他需要将其作为分配信号的应用程序。



使用以下 xhost 方法，您可以指定特定的 sigcode 作为分配。
```python 
def io_assign_set_out_bit(sigcode: int) -> int
```

当您在 main.py 中定义 on_app_init( ) 函数时，输入一个例程以指定分配，如下所示，执行将在导入 ArgosX 的那一刻发生。



main.py
```python 
.. previous steps skipped
 
 
import xhost
 
 
...skipped
 
 
def on_app_init() -> int:
   """（回调）在自我诊断后立即调用
   返回：
      0
   """
   print('[argosx] on_app_init();')
   xhost.io_assign_set_out_bit(setup.sigcode_err)
   return 0
```

再次执行虚拟控制器。在作业中执行 import argosx 时，重新打开通用输出面板。

指定的信号将显示为已分配（粗体）。

![](../../_assets/image_28.png)
[__SOURCE](3-practice-argosx/1-roblang/8-xhost-method.md)
#### 3.1.8 手动引用 xhost 模块方法
<hr>

##### get(url, query)
* 描述:
  OpenAPI GET 方法。
* 参数:  
  url (str): API 端点路径。ex. "project/rgen"  
             **不包括** 基本 URL (http://192.168.1.150:8888/)。仅提供后面的路径。
  query: str. OpenAPI 查询。
* 返回:
  str. 响应值。

<hr>

##### put(url, body)
* 描述:
  OpenAPI PUT 方法。
* 参数:
  url (str): API 端点路径。ex. "project/rgen"  
             **不包括** 基本 URL (http://192.168.1.150:8888/)。仅提供后面的路径。
  body: str. 请求体。
* 返回:
  str. 响应体。

<hr>

##### post(url, body)
* 描述:
  OpenAPI POST 方法。
* 参数:
  url (str): API 端点路径。ex. "project/rgen"  
             **不包括** 基本 URL (http://192.168.1.150:8888/)。仅提供后面的路径。
  body: str. 请求体。
* 返回:
  str. 响应体。

<hr>

##### hist_print(msg)
* 描述:
  同 printh()，只应用用户参数/hist_print_level 设置。
* 参数:
  msg: str. 消息。
* 返回:
  None

<hr>

##### printh(msg)
* 描述:
  打印到历史日志。
* 参数:
  msg: str. 消息。
* 返回:
  None

<hr>

##### issue_alarm(task_no, type, code)
* 描述:
  发布错误或警告事件。
* 参数:
  task_no: int. 任务编号 (0~7)
  type:
    'E': 错误
    'W': 警告
  code: int. 警报代码编号
* 返回:
  None

<hr>

##### issue_notice(task_no, code, msg, delay_sec)
* 描述:
  发布通知事件。
* 参数:
  task_no: int. 任务编号 (0~7)
  code: int. 警报代码编号
  msg: str. 通知消息
  delay_sec: float. 隐藏前的延迟时间 (秒)
* 返回:
  None

<hr>

##### set_job_state_msg(task_no, msg)
* 描述:
  在教学显示器上设置作业状态消息。
* 参数:
  task_no: int. 任务编号 (0~7)
  msg: str. 要显示的状态消息
* 返回:
  None

<hr>

##### io_set_so(sig_no, val)
* 描述:
  设置系统 I/O 输出位。
* 参数:
  sig_no: int. 信号编号 (0~959)
  val: int. (1 或 0)
* 返回:
  0: ok
  -1: 索引范围超出

<hr>

##### io_get_in_bit(sigcode)
* 描述:
  根据信号代码获取用户 I/O 输入位。
* 参数:
  sigcode: int. 信号代码 (例如 30017 对 fb3.di17)
* 返回:
  0 或 1

<hr>

##### io_set_out_bit(sigcode, val)
* 描述:
  根据信号代码设置用户 I/O 输出位。
* 参数:
  sigcode: int. 信号代码
  val: int. (1 或 0)
* 返回:
  0: ok
  -1: 索引范围超出

<hr>

##### io_set_pulse_by_sigcode(sigcode, onoff, count, on_ms, off_ms, lag_ms, non_update)
* 描述:
  生成 I/O 脉冲输出
* 参数:
  sigcode: int
  onoff:
    1: 开脉冲
    0: 非脉冲关 (延迟关)
    -1: 关脉冲
  count: int. 脉冲计数
  on_ms: int. 开的宽度 (毫秒)
  off_ms: int. 关的宽度 (毫秒)
  lag_ms: int. 延迟的宽度 (毫秒)
  non_update:
    1: 如果已经注册，则不更新
    0: 重新注册脉冲
* 返回:
  0: ok
  -2: 已经注册

<hr>

##### io_assign_set_in_bit(sigcode)
* 描述:
  设置信号代码为分配的输入 I/O
* 参数:
  sigcode: int
* 返回:
  0: ok
  -1: 无效的信号代码

<hr>

##### io_assign_set_out_bit(sigcode)
* 描述:
  设置信号代码为分配的输出 I/O
* 参数:
  sigcode: int
* 返回:
  0: ok
  -1: 无效的信号代码

<hr>

##### io_set_triggout(task_no, fbname, val, ofs, ax_no, type)
* 描述:
  触发输出 I/O
* 参数:
  task_no: int
  fbname: str. (例如 fb3.do17, dob3)
  val: int
  ofs: int. 偏移时间 (毫秒) 或 偏移距离 (毫米)
  ax_no: int. 0(TCP), 1~ 轴编号
  type:
    0x01: OT (基于时间)
    0x02: OD (基于距离)
    0x04: 强制输出
    0x10: OX
    0x20: OY
    0x30: OZ
* 返回:
  1: 缓存满
  2: 完成
  0: ok
  -1 ~ -4: 错误

<hr>

##### io_n_blocks()
* 描述:
  获取 I/O 块的数量
* 返回:
  int

<hr>

##### io_size_block_addr()
* 描述:
  获取块中的位 (地址空间)
* 返回:
  int

<hr>

##### io_fbname_from_sigcode(sigcode, is_out)
* 描述:
  从信号代码获取 fbname
* 参数:
  sigcode: int
  is_out: 1 输出 / 0 输入
* 返回:
  fbname 字符串

<hr>

##### solve_expr_as_string(task_no, expr)
* 描述:
  求解表达式并返回字符串结果
* 参数:
  task_no: int
  expr: str
* 返回:
  str

<hr>

##### solve_expr_as_int(task_no, expr)
* 描述:
  求解表达式并返回整数结果
* 参数:
  task_no: int
  expr: str
* 返回:
  int

<hr>

##### exec_mode()
* 描述:
  检查执行模式
* 返回:
  True 或 False

<hr>

##### cont_mode()
* 描述:
  检查连续模式
* 返回:
  True 或 False

<hr>

##### req_to_continue()
* 描述:
  请求主机进入连续模式
* 返回:
  None

<hr>

##### set_err_code(code)
* 描述:
  设置错误代码
* 参数:
  code: int
* 返回:
  None

<hr>

##### lang_timer()
* 描述:
  获取语言计时器值
* 返回:
  int (毫秒)

<hr>

##### set_lang_timer(timeout)
* 描述:
  设置语言计时器值
* 参数:
  timeout: int (毫秒)
* 返回:
  None

<hr>

##### branch_to_addr(addr)
* 描述:
  跳转到地址
* 参数:
  addr: str
* 返回:
  None

<hr>

##### abs_path(name)
* 描述:
  获取文件系统中的绝对路径
* 参数:
  name: home, project, log, jobs, vars, backup, fbrr, module, apps_main, help
* 返回:
  绝对路径字符串

<hr>

##### sci_open(port)
* 描述:
  打开串口
* 参数:
  port: int
* 返回:
  0: OK
  -1: 不 OK

<hr>

##### sci_close(port)
* 描述:
  关闭串口
* 参数:
  port: int
* 返回:
  0: OK
  -1: 已经关闭

<hr>

##### sci_send_bytes(port, data)
* 描述:
  串口发送字节数据
* 参数:
  port: int
  data: bytes
* 返回:
  0: OK
  -1: 不 OK

<hr>

##### sci_recv_bytes(port, len)
* 描述:
  串口接收字节数据
* 参数:
  port: int
  len: int
* 返回:
  bytes

<hr>

##### sci_clear_buf(port)
* 描述:
  清除串口缓冲区
* 参数:
  port: int
* 返回:
  None

<hr>

##### sci_send(port, data)
* 描述:
  串口发送字符串数据
* 返回:
  0: OK
  -1: 不 OK

<hr>

##### sci_recv(port)
* 描述:
  串口接收字符串数据
* 参数:
  port: int
* 返回:
  str

<hr>
[__SOURCE](3-practice-argosx/1-roblang/9-non-blocking.md)
#### 3.1.9 解决机器人语言功能阻塞问题
##### 阻塞问题

在上一个章节中实现的 recv_msg( ) 函数存在一个问题。

当调用此函数时，会有一个等待远程响应的时间，只有在收到响应后，该函数的操作才会结束。如果由于 ArgosX 系统的问题没有响应，等待将会永久持续而无法结束该函数。

让我们检查在没有来自 argosx_sub.py 的响应的情况下，作业程序是如何工作的。

为了进行测试，找到 argosx_stub.py 中名为 sleep_sec 的变量，并将其值更改为 20。然后，在接收到 "req ~" 请求时，argosx_stub 将在响应 "res ~" 之前等待 20 秒。

argosx_stub.py
``` python 
# const
buf_size = 0x8000    # 32kb ; 允许的数据包长度
port_no = 54321      # ArgosX 命令的端口
sleep_sec = 20        # 响应前的延迟
```

让我们再次执行 argosx_stub.py，然后使用教学挂件的 STEP FWD 键逐行运行作业程序。

如果在执行 argosx.req 之后立即执行 argosx.res( )，则在执行 req 后 cursor 将在 20 秒内无法移动，如下所示。此状态称为阻塞，从用户操作性上来说并不好。这个规范是有问题的，因为如果没有响应，可能会导致必须关闭和重新启动机器人控制器的情况。
<br></br>
![](../../_assets/image_29.png)

哪个规范更为理想？一般来说，当命令在等待某种状态时，例如 wait 命令，当按下 STEP FWD 键时，会发生等待，而当未按下按键时，光标可以移动。此外，超时和逃逸地址可以指定为参数，因此在发生超时时，可以执行异常处理操作来分支到逃逸地址。
<br></br>
![](../../_assets/image_30.png)

如果在 wait-di6 状态下 10 秒后发生超时，则将分支到 *tout。

#### 执行模式与继续模式

让我们来看下面的流程图。${cont_model} 主机调用机器人语言命令的模式有两种：执行模式和继续模式。在继续模式中，主机再次调用命令。

主机首先在执行模式中调用命令。在大多数情况下，各个命令执行它们的操作并立即结束，而主机在确认不处于继续模式后完成对命令的处理。
<br></br>

![](../../_assets/image_31.png)

然而，一些命令具有等待操作（意味着它等待某种状态或事件，例如 I/O 输入、以太网数据接收、特定的时间段、机器人操作完成等）。主机与插件之间将执行以下过程。

* 插件的等待操作命令将通过 xhost.exec_mode( ) 检查模式。True 表示执行模式，False 表示继续模式。因此，如果确认该模式为执行模式（是），则时间将转移到超时参数，并且将请求 ${cont_model} 主机下次以继续模式调用，直到操作结束。
* 如果在执行 xhost.req_to_continue 后操作结束，则意味着该模式为继续模式。因此，${cont_model} 主机再次调用相关命令。
* 插件的等待操作命令将通过 xhost.exec_mode( ) 检查模式。如果确认该模式为继续模式（否），则将检查定时器。如果发生超时，将分支到逃逸地址（branch_to_addr），并在没有继续模式请求的情况下结束操作。
* 如果未发生超时，则会检查等待条件是否完成（wait-complete condition？）。如果等待条件完成，由于没有继续模式请求，操作将如常结束；但如果未完成，操作将在未请求 ${cont_model} 主机继续模式（req_to_continue）调用的情况下结束。
* 如果当前调用中没有继续模式请求（继续模式否），则 ${cont_model} 主机将完成对相关命令的处理。
<br></br>

 ![](../../_assets/image_32.png)

现在让我们改进 argosx.res( ) 使其也具备等待操作的规范。

##### 制作非阻塞通讯模块

为进行以太网传输/接收实现的通讯模块，内部使用了 socket 模块。默认情况下，socket 是阻塞模式，这意味着 socket.recvfrom( ) 函数，即 UDP 接收函数，在数据接收之前不会执行任何返回操作。

首先，我们需要将使用的 socket 实例更改为非阻塞模式。在 comm.open( ) 函数中插入 sock.setblocking(False)，如下面所示。

comm.py
``` python
def open(ip_addr: str, port: int) -> int:
   """
   打开用于 UDP 通信的 socket
   参数：
      ip_addr     远程的 ip 地址。例如 "192.168.1.172"
      port        远程的端口号。例如 "192.168.1.172"
 
   返回：
         0     正常
         -1    错误
   """
   global raddr, sock
   try:
      raddr = (ip_addr, port)
      sock = socket.socket(family=socket.AF_INET, type=socket.SOCK_DGRAM)
      sock.setblocking(False)
   except socket.error as e:
      print("socket 创建或绑定错误 :", e)
      return -1
   logd('comm.open: ' + str(raddr))
   return 0
```

现在，对于 socket.recvfrom( ) 函数，如果没有收到数据，将立即发生 BlockingIOError 异常，而不会发生阻塞（如果收到数据，将立即进行返回操作）。

在 comm.recv_msg( ) 中插入处理，用于在发生 BlockingIOError 异常时返回空字符串。

comm.py
``` python
def recv_msg():
   """
   等待来自 sock 的消息
   返回：
      接收到的字符串
   """
   if sock is None: return ""
 
   try:
      data, ip_port = sock.recvfrom(buf_size)
      bts = bytearray(data)
      msg = bts.decode()
      logd('响应: ' + msg)
      return msg
   except BlockingIOError:
      return ""
   except Exception as e:
      print('recv_msg() 中发生异常: ' + str(e))
      return ""
```

##### 在 res( ) 函数中实现等待操作

向 res( ) 函数添加两个参数，timeout 和 addr_on_timeout（逃逸地址），如下所示。

如果未指定超时，则将应用默认值 -1，导致无限等待。如果未指定逃逸地址，则将应用默认值 -1，导致在超时时不会发生分支。

删除现有实现。之后，在简单形式中实现等待操作，其中使用 xhost.exec_mode( ) 函数检查模式。如果模式为执行模式，将调用 res_exec( )，但如果处于继续模式，则将调用 res_cont( ) 函数。

roblang.py
``` python 
""" ArgosX 视觉系统接口 - 机器人语言
 
 
@author:    Jane Doe, BlueOcean Robot & Automation, Ltd.
@created:   2021-12-06
"""
 
from . import comm
from . import setup
 
import xhost
import typing
 
 
# 类型
int_or_str = typing.Union[int, str]
 
 
 
省略...
 
 
 
def res(timeout: int=-1, addr_on_timeout: int_or_str=-1) -> str:
   """
   等待来自 ArgosX 的响应
    
   参数：
      timeout: (毫秒)，默认(-1) 意味着无限制。
      addr_on_timeout: 超时时的分支地址。
         (例如 99, "S7", "*TimeOut")
         默认(-1) 意味着不分支。
 
   返回：
      来自 ArgosX 的响应字符串。
      "" 如果失败。
      例如 "[30, 25.7, 11.9, 31.6, 12.8, -54.6]"
   """
   ret = ""
   if xhost.exec_mode():
      _res_exec(timeout)
   else:
      ret = _res_cont(addr_on_timeout)
   return ret
```

现在，让我们在 res( ) 函数下方实现 _res_exec( )。在此实现中，机器人语言的定时器将设置为超时参数的值，并且在操作结束前，将请求主机以继续模式调用。这不简单吗？

roblang.py
``` python
省略的步骤...
 
 
def _res_exec(timeout: int) -> None:
   """exec 模式的 res() 实现"""
   xhost.set_lang_timer(timeout)
   xhost.req_to_continue()
```

实际操作将在继续模式的 _res_cont( ) 函数中执行。超时操作的分支由 check_timeout_and_branch( ) 函数创建。

如果发生超时，sigcode_err 输出信号将被打开，并且操作将立即结束。

如果未发生超时，将进行数据接收。如果在此过程中未收到任何数据，将请求主机以继续模式调用（xhost.req_to_continue）。随后将执行返回空字符串的操作。

roblang.py
``` python
省略的步骤...
 
 
def _res_cont(addr_on_timeout: int_or_str) -> str:
   """cont 模式的 res() 实现"""
   val = 0
   msg = ""
   timeout = _check_timeout_and_branch(addr_on_timeout)
   if timeout:
      val = 1
   else:
      msg = comm.recv_msg()
      print(msg)
      if msg=="res fail":
         val = 1
         msg = ""
      elif msg=="":    # 未收到响应
         xhost.req_to_continue()
      else:    # 正常响应
         msg = get_base_shift_array_from_res(msg)
   xhost.io_set_out_bit(setup.sigcode_err, val)
   return msg
 
 
 
def _check_timeout_and_branch(addr_on_timeout: int_or_str) -> bool:
   """如果超时，则执行分支。
 
   返回：
      True     超时。已分支。
      False    非超时。
   """
   timer = xhost.lang_timer()
   if timer!=0: return False  # 非超时
   # 超时！
   if addr_on_timeout==-1:
      return True
   # 统一转为字符串
   str_addr = str(addr_on_timeout)
   xhost.branch_to_addr(str_addr)
   return True
```

##### 测试非阻塞操作

现在让我们检查规格是否如我们所愿。这次再次执行虚拟控制器，并使用教学挂件的 STEP FWD 键逐行运行作业程序。

如果执行 argosx.req，然后立即执行 argosx.res( )，则操作将不会完成，因为响应尚未到达。当释放 STEP FWD 键时，向前步骤指示将关闭，光标可以移动。如果按下 STEP FWD 键，等待状态将恢复。

如果在按下 STEP FWD 键的情况下，req( ) 执行后经过 20 秒，将完成接收。
<br>
![](../../_assets/image_33.png)

现在，让我们添加一个超时参数。将其指定为 3000 毫秒并再次执行操作。如果在按下 STEP FWK 键的情况下经过 3 秒，将转到下一个命令。

作业
```
var str=argosx.res(3000) # 等待响应
 
print str
```

现在，让我们也添加一个逃逸步骤。当您如下面所示进行教学并在 res( ) 函数上按下 STEP FWD 键超过 3 秒时，您会看到分支到行号 99 并且 "timeout" 将被执行。

作业
```
... 省略的步骤
      
     var str=argosx.res(3000,99) #等待响应
     print str
     if str==""
       print "req error"
       stop
     else
       var sft=Shift(str) # 将移位数组字符串转换为移位数据
       print sft.x, sft.y, sft.z, sft.rx, sft.ry, sft.rz
     endif
      
     argosx.close()
     end
      
  99 print "timeout"
     end
```
[__SOURCE](3-practice-argosx/1-roblang/10-regi-cmds.md)
#### 3.1.10 注册机器人语言命令输入

在使用教导操作面板时直接输入机器人语言不方便。

因此，我们需要确保机器人语言可以通过[命令输入]更轻松地编写。

为此，我们需要注册机器人语言。

首先，我们需要在info.json中添加一个“cmds”标签。对应的值需要指定一个包含cmds组织内容的json。

info.json
```json
{
	"author" : "BlueOcean Robot & Automation, Ltd.",
	"binding" : "plug-in",
	"cmds" : "cmds.json",
	"copyright" : "All right reserved",
	"description" : "ArgosX Vision System interface",
	"entry" : "main.py",
	"menu" : "ui/menu.json",
	"startup" : "boot",
	"version" : "v0.9.0"
}
```

将cmds.json添加到ArgosX文件夹中，以定义相关文件中命令的属性。

``` json
{
	"fmts" : [
		{
			"name": "init"
		},
		{
			"name": "req",
			"samples": "req 39",
			"props": [
				{
					"guide": "工作编号",
					"range": "[1~100]"
				}
			]
		},
		{
			"name": "res"
		},
		{
			"name": "close"
		}
	]
}

```

|Item|Meaning|Example|
|---|---|---|
|fmts|机器人语言模块名称|"fmts"|
|name|功能名称|"name": "init"|
|samples|示例（机器人语言输入类型）|"samples": "req 39"|
|props|功能的输入属性|"props"|
|guide|输入参数指导消息|"guide": "工作编号"|
|range|输入参数值的范围|"range": "[1-100]"|

每个项目的含义见上表。如果有多个输入参数，将属性分组为与输入参数数量相同的组，然后添加到“props”中。

<br></br>
对于添加的命令，可以通过按教导操作面板底部的[命令输入]按钮进行检查。
![](../../_assets/image_82.png)

[命令输入]-[argosx]  
![](../../_assets/image_83.png)
[__SOURCE](3-practice-argosx/2-callback/README.md)
# 3.2 实际项目：ArgosX - 回调


在操作 ${cont_model} 控制器时，有一些主要事件，例如模式变化、电动机开启、重置、启动和精度正常。我们可以注册函数到插件中，以便它为某个事件执行独特的操作。

这些函数是回调函数。之所以称之为回调函数，是因为它们是由其他部分（${cont_model} 主机）调用的，而不是在 Python 代码中主动调用的。



* 注册回调函数
* 实现回调函数
* 参考回调函数的手册
[__SOURCE](3-practice-argosx/2-callback/1-register-callback.md)
#### 3.2.1 注册回调函数


注册回调函数的方法非常简单。每个事件的回调函数名称已经确定，因此通过在插件代码中使用相关的回调函数名称定义回调函数，回调函数将在插件导入时自动注册。


我们需要再次参考<U>3.1.1 ArgosX及接口插件的规格</U>中的其他功能。

根据机器人是否处于电机开启或关闭状态，ArgosX的LED灯也应相应地打开或关闭。具体而言，命令"light-on"或"light-off"应发送到ArgosX。



让我们在一个单独的文件（Python模块）中创建回调函数，如下所示。现在，我们将只打印字符串进行测试，而不执行特定操作。



callback.py (用于测试)
```python
def on_motor_on() -> int:
   """(callback) 在电机开启时
   Returns: 0
   """
   print('on_motor_on')
   return 0
 
 
def on_motor_off() -> int:
   """(callback) 在电机关闭时
   Returns: 0
   """
   print('on_motor_off')
   return 0
```

从入口文件导入回调模块。



main.py
```python
跳过之前的步骤...
 
from . import setup
from .roblang import *
from .setup import *
from .callback import *
 
import xhost
 
跳过后续步骤...

```
重启控制器，开启电机，并使用Step FWD按钮导入ArgosX。

在这个状态下，如果每次电机开启或关闭时在控制台窗口中打印的结果如下，这意味着回调函数定义良好。
```
on_motor_on

on_motor_off
```
[__SOURCE](3-practice-argosx/2-callback/2-uses-callback.md)
#### 3.2.2 实现回调函数
因为我们确保回调函数被正确调用，接下来实现实际操作。

如您在检查<U>3.1.1 ArgosX 及接口插件的规格</U>时所见，只需将 “light-on” 和 “light-off” 消息发送到 ArgosX 硬件即可。

因为 comm 模块已经实现了发送以太网字符串的函数，您只需如下调用一个。

```
comm.send_msg("light-on")
comm.send_msg("light-off")
```
<br></br>

但是，这个实现有一个问题。如果通过 argosx.init 调用 comm.open( ) 函数，一个机器人语言命令，字符串将会正常传输。但是，如果未调用该函数，字符串将不会被传输。

此外，即使由于 comm.close( ) 而关闭通信时也不会发生传输。

因此，有必要定义一个字符串传输函数，使其能够在关闭状态下打开通信，并进行传输后再关闭通信。

<br></br>
在 comm.py 所在的同一文件夹中创建一个 comm_ex.py 文件，如下所示。
<br></br>

comm_ex.py

``` python
""" ArgosX 视觉系统接口 - 主程序
 
 
@author:    Jane Doe, BlueOcean Robot & Automation, Ltd.
@created:   2021-12-07
"""
 
 
from . import setup
from . import comm
 
 
def send_msg_once(msg: str) -> int:
   was_closed = (comm.is_open()==False)
   if was_closed: comm.open(setup.ip_addr, setup.port)
   iret = comm.send_msg(msg)
   if was_closed: comm.close()
   return iret

```

现在，我们可以简单地实现回调函数，如下所示，通过导入 comm_ex 模块。

callback.py

``` python
""" ArgosX 视觉系统接口 - 回调函数
 
 
@author:    Jane Doe, BlueOcean Robot & Automation, Ltd.
@created:   2021-12-07
"""
 
from . import comm_ex
 
 
def on_motor_on() -> int:
   """(callback) 电机开启
   Returns: 0
   """
   print('on_motor_on')
   return comm_ex.send_msg_once("light-on")
 
 
def on_motor_off() -> int:
   """(callback) 电机关闭
   Returns: 0
   """
   print('on_motor_off')
   return comm_ex.send_msg_once("light-off")
```

首先，从命令提示符或 vscode 执行 argosx_stub。

重启虚拟控制器，然后运行作业文件直到 argosx.init( )。如果在此状态下执行操作如下，则表示已检查正常照明功能的操作。

<br></br>
<U>__argosx_stub 侧（充当 ArgosX 的服务器）__</U>

每当发生电机关闭和电机开启功能时，以下字符串将打印在控制台上。
```
request : light-off
LED 灯已关闭

request : light-on
LED 灯已开启
```
[__SOURCE](3-practice-argosx/2-callback/3-ref-callback.md)
### 3.2.3 手动参考回调函数

<table>
  <thead>
    <tr>
      <th style="text-align:left">Python 回调函数</th>
      <th style="text-align:left">在主软件内部调用发生的时间点</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>on_app_init()</td>
      <td>
       自我诊断后
      </td>
    </tr>
   <tr>
      <td>on_before_self_diagnosis_proc()</td>
      <td>
       自我诊断之前
      </td>
    </tr>
    <tr>
      <td>on_mot_servoerror_detect()</td>
      <td>
       检测到伺服错误时
      </td>
    </tr>
     <tr>
      <td>on_system_status_chk_proc()</td>
      <td>检查系统是否存在异常时 (10 ms)</td>
    </tr>
    <tr>
      <td>on_period_low()</td>
      <td>
       基于最低优先级周期调用时 (5 ms)
      </td>
    </tr>
   <tr>
      <td>on_motor_on()</td>
      <td>
       电机开启
      </td>
    </tr>
    <tr>
      <td>on_motor_off()</td>
      <td>
       电机关闭
      </td>
    </tr>
    <tr>
      <td>on_stop(task_no:int)</td>
      <td>
       停止时
      </td>
    </tr>
    <tr>
      <td>on_restart(task_no:int)</td>
      <td>启动时</td>
    </tr>
     <tr>
      <td>on_cur_job_selected_by_tp(task_no:int)</td>
      <td>当TP选择作业程序时。</td>
    </tr>
    <tr>
      <td>init_signal_output_status()</td>
      <td>初始化应用信号输出时</td>
    </tr>
    <tr>
      <td>set_ext_io_sig_proc()</td>
      <td>
       处理分配的信号时
      </td>
    </tr>
  </tbody>
</table>
[__SOURCE](3-practice-argosx/3-setup-ui/README.md)
## 3.3 实用项目：开发 ArgosX 设置屏幕 UI

如果插件中有各种设置，我们需要一个设置屏幕，以向用户显示设置值，并在必要时允许用户将其更改为新值。

在本节中，让我们练习实现 ArgosX 设置屏幕 UI 并将特定菜单项部署到它们的位置。
<br>

* ArgosX 设置屏幕用户界面的规格
* 设置屏幕的布局
* 操作设置屏幕
* 将菜单注入描述屏幕
* 加载和保存设置屏幕的值
* 加载和保存设置文件
* 操作 F 按钮 - 初始化为默认值
[__SOURCE](3-practice-argosx/3-setup-ui/1-concept-if.md)
### 3.3.1 ArgosX设置屏幕用户界面的规格

让我们根据以下规格创建设置屏幕用户界面。

* 在设置 - 应用程序参数菜单中，有一个菜单可以进入ArgosX设置屏幕。
* 在ArgosX设置屏幕上，您可以设置ArgosX系统的IP地址和端口号。
* 设置的内容将作为json文件保存到argosx.json文件中。
* 提供一个F按钮（初始化全部），将整个屏幕设置为默认值。
* 提供一个F按钮（初始化单个），将当前选定的项目设置为默认值。
<br></br>

![](../../_assets/image_34.png)





无论ArgosX是否已导入到工作程序中，我们希望能够打开设置屏幕并检查/更改设置值。

将argosx/info.json文件中的启动项从“manual”更改为“boot”。现在，由于在控制器启动时将导入ArgosX，因此无需将ArgosX导入到工作程序中。

(您仍然可以在HRScript中进行设置。)
[__SOURCE](3-practice-argosx/3-setup-ui/2-layout.md)
### 3.3.2 设置屏幕的布局

打开包含 ArgosX 文件夹的 apps/ 文件夹的 vscode。

（此步骤是因为 ArgosX 用户界面需要引用 apps/_common/ 中的文件。应将包含所有由 Live server 引用的文件的文件夹作为工作区中的顶级文件夹打开。）
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

将内容写入如下。

setup.html
``` html
<!DOCTYPE html:5>
<!--
   @author: Jane Doe, BlueOcean Robot & Automation, Ltd.
   @brief: ArgosX 视觉系统接口 - 设置
   @create: 2021-12-06
-->
<html>
  
<head>
   <title>ArgosX 视觉系统 - 设置</title>
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

当 setup.html 被打开时，点击右下角的 Go Live 按钮以运行 Live server。
<br>
![](../../_assets/image_38.png)
<br>

如果出现关于 vscode 的安全警告，请勾选“允许通信”下的所有项目并点击“允许访问”按钮。
<br>
![](../../_assets/image_39.png)
<br>

另外，您可以通过右键点击 setup.html 打开弹出菜单，然后选择“通过 Live Server 打开”。
<br>
![](../../_assets/image_40.png)
<br>

当 Google Chrome 浏览器打开时，我们可以检查草图布局。

setup.html 的草图布局
<br>
![](../../_assets/image_41.png)
<br>
[__SOURCE](3-practice-argosx/3-setup-ui/3-setup-action.md)
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
[__SOURCE](3-practice-argosx/3-setup-ui/4-menu.md)
### 3.3.4 将菜单注入到描述屏幕

我们已经通过 hello_world 示例练习了注入菜单。

现在，让我们在教学挂件的系统 - 应用参数菜单下注入 ArgosX 设置屏幕。



在 ui/ 文件夹下创建一个 menu.json 文件，如下所示。



menu.json
``` json
[
    {
        "path": "system/appl/",
        "id": "argosx",
        "icon": "argosx/ui/lm_argosx.png",
        "label": "ArgosX 视觉",
        "url": "argosx/ui/setup.html"
    }
]
```


这次我们要注入一个图片图标。通过以下两个网站，我们获得了一个透明背景为 104 x 104 像素的 png 图标。



Bootstrap Icons (https://icons.getbootstrap.com/#icons)：一个开源图标库。图标以 SVG 矢量文件格式提供。

EZGIFCOM (https://ezgif.com/svg-to-png)：在线将 SVG 文件转换为所需分辨率的 PNG 文件。

<br></br>
![](../../_assets/lm_argosx.png) lm_argosx.png 的示例（您可以下载并使用此图片。）


<br></br>

现在，我们应该再次运行虚拟主板和虚拟教学挂件。


当进入系统 _ 应用参数菜单时，您可以找到新添加的 ArgosX 视觉菜单项，如下所示。
<br>
![](../../_assets/image_42.png)
<br>



当您选择该菜单时，我们编写的布局将很快出现。
<br>
![](../../_assets/image_43.png)
<br>
[__SOURCE](3-practice-argosx/3-setup-ui/5-load-param.md)
### 3.3.5 加载和保存设置屏幕的值

当打开设置屏幕时，保存在 Python 插件中的设置值将作为中间 JavaScript 对象加载，然后显示为屏幕上的 HTML 元素。

当用户修改这些值并点击 [OK] 按钮时，屏幕上的 HTML 元素将作为中间 JavaScript 对象创建并保存到 Python 插件中的变量中。

要执行传输，实施 JavaScript 的 updateData( ) 函数以及 Python setup 模块的 getter 和 putter 函数。
<br></br>

![](../../_assets/image_44.png)

##### element ↔ javascript object

setup.js
``` js
Previous steps skipped...
 
 
///@param[in]  data
///@param[in]  to_data     true; element->data, false; element<-data
function updateData(data, to_data)
{
   ddx_edit_ip(data, 'ip_addr', to_data);
   ddx_edit_i(data, 'port', to_data);
   ddx_edit_sig(data, 'sigcode_err', to_data);
}
```

元素与 JavaScript 对象之间的双向值传输通过 updateData( ) 函数定义。参数 data 是 JavaScript 对象，而 to_data 是布尔变量，指示传输的方向。如果为 true，方向为从元素到数据；如果为 false，方向为从数据到元素。

我们还可以直接使用文档对象模型应用程序编程接口（DOM API）或 jQuery 实现传输。然而，通过使用 dst_setup.js 提供的动态数据交换（DDX）函数，可以更简洁地实现传输。
<br></br>

DDX 函数

|Function signature|HTML element|Data type|Description|
|---|---|---|---|
|ddx_edit(data, name, to_data)|```<input type='text'>```|string||
|ddx_edit_i(data, name, to_data)|```<input type='text'>```|integer||
|ddx_edit_sig(data, name, to_data)|```<input type='text'>```|integer|如果设置通用 I/O 信号的元素值为 sigcode，则将按原样传输。<br>如果值为 fb?.? 格式，将转换为 sigcode 并传输。|
|ddx_edit_ip(data, name, to_data)|```<input type='text'>``` x 4 (units)|string|这是用于设置 IP 地址的。<br>如果名称为 'ip'，则四个元素的每个 ID 应为 'ip_0'，'ip_1'，'ip_2' 和 'ip_3'。<br>数据将保存为 "xxx.xxx.xxx.xxx."|
|ddx_check(data, name, to_data)|```<input type='checkbox'>```|boolean||
|ddx_radio(data, name, to_data)|```<input type='radio'>```x N (units)|integer|每个单选元素应具有唯一的值属性。|

<br>

##### javascript object ↔ python data

setup.js
``` js
///@author: Jane Doe, BlueOcean Robot & Automation, Ltd.
///@brief: ArgosX Vision System interface - setup general
///@create: 2021-12-06
 
 
function init()
{
   setDomPath('/apps/argosx/svr_general');
   setUpdateGuideBar(updateGuideBar);
   setUpdateData(updateData);
   onReady();
}
 
 
...Subsequent steps skipped
```

在初始化步骤中，使用 setDomPath( ) 函数指定了 "/apps/argosx/svr_general" 路径。

所有插件，包括 ArgosX，都将部署在 /apps/ 下，路径的最后一个名称是 "svr_"，并附加设置组名称。

<br></br>

**/apps/{app name}/svr_{setup group name}**

<br></br>
一个插件可以有一个或多个设置屏幕，一个屏幕上的数据将被视为一个对象，并称为设置组。每个设置组可以自由命名，那个名称成为设置组名称。在本例中，设置组名称确定为 'general'。

如果你在设置组名称前面添加前缀 "get_" 或 "put_"，而不是 "svr_"，它将分别成为 getter 或 putter 服务函数名称。此外，如果在 getter 函数名称后添加 '_def'，它将成为获取默认值的默认 getter 服务函数名称。

因此，在上面的例子中，默认 getter、getter 和 putter 服务函数名称将分别变为 get_general_def( )，get_general( ) 和 put_general( )。

将 setup.py 文件添加到项目文件夹 argosx/ 中。
<table>
  <thead>
    <tr>
      <th style="text-align:left"></th>
      <th style="text-align:left">Function</th>
      <th style="text-align:left">调用发生的时间点</th>
      <th style="text-align:left">操作</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>默认 getter</td>
      <td>get_general_def( )</td>
      <td>当请求默认值时。</td>
      <td>每个默认设置值将作为属性值保存在一个对象中，所有对象将被返回。</td>
    </tr>
    <tr>
      <td>dgetter</td>
      <td>get_general( )</td>
      <td>当打开设置屏幕时。</td>
      <td>每个 Python 插件具有的默认设置值将作为属性值保存在对象中，所有对象将被返回。</td>
    </tr>
    <tr>
      <td>putter</td>
      <td>put_general( )</td>
      <td>当使用 [OK] 或 [Apply] 按钮保存设置屏幕的值时。</td>
      <td>传递到 body 参数的属性值将被读取并保存在 Python 插件中。</td>
    </tr>
  </tbody>
</table>

<br>

现在，让我们实现各个函数。每个属性的键应与 DDX 函数中使用的相同名称。

setup.py
``` python

""" 机器人应用程序 - argosx - 设置
 
 
@author:    Jane Doe, BlueOcean Robot & Automation, Ltd.
@created:   2021-12-06
"""
 
 
def get_general_def() -> dict:
   """
   返回:
      设置的默认值
   """
   print('def_general_def()')
 
 
   data_def = {
      'ip_addr': '192.168.1.100',
      'port' : 54321,
      'sigcode_err': 5
   }
    
   return data_def
 
 
def get_general() -> dict:
   """
   返回:
      设置字典。
   """
 
   print('get_general()')
    
   ret = {}
   ret["ip_addr"] = ip_addr
   ret["port"] = port
   ret["sigcode_err"] = sigcode_err
    
   return ret
 
    
def put_general(body: dict) -> int:
   """
   参数:
      body  设置字典。
    
   返回:
      0
   """
   global ip_addr, port, sigcode_err
   print('put_general()')
 
   ip_addr = body["ip_addr"]
   port = body["port"]
   sigcode_err = body["sigcode_err"]
 
   save_to_setup_file(body) # 保存到文件
    
   return 0
```

关于 setup.py 中全局变量的初始化，让我们将当前方法更改为使用默认 getter 函数的方法，以便代码不会复制。

setup.py
``` python
...Previous steps skipped
 
 
gen_def = get_general_def()
ip_addr : str = gen_def['ip_addr']
port : int = gen_def['port']
sigcode_err = gen_def['sigcode_err']
```

##### 操作测试

现在，让我们重启虚拟控制器，导入 ArgosX，然后进入 ArgosX 设置屏幕。默认设置值将显示如下。
<br> ![](../../_assets/image_45.png) <br>

将 IP 地址更改为 192.168.1.172，输入 3.4 作为故障输出信号，然后按 <Enter> 将其值更改为 fb3.4。之后，按 [OK] 按钮退出屏幕。
<br>

当你再次进入屏幕时，如果新设置的值正常显示，这意味着操作正常。
<br> ![](../../_assets/image_46.png) <br>
[__SOURCE](3-practice-argosx/3-setup-ui/6-load-file.md)
### 3.3.6 加载和保存设置文件

在前面的章节中，我们练习了将设置值保存到 Python 变量中并加载它们。

为了在关闭和打开控制器时保持该设置，它应该保存到文件中。

添加 `save_to_setup_file( )` 函数，用于将设置保存到文件，以及 `load_from_setup_file( )` 函数，用于从文件加载设置，至 `setup.py` 如下。

setup.py
``` python 

""" robot application - argosx - setup
 
 
@author:    Jane Doe, BlueOcean Robot & Automation, Ltd.
@created:   2021-12-06
"""
 
import xhost
import json
 
 
fname_setup = 'argosx.json'
 
 
def get_general_def() -> dict:
   """
   Returns:
      默认设置值
   """
   print('def_general_def()')
 
   data_def = {
      'ip_addr': '192.168.1.100',
      'port' : 54321,
      'sigcode_err': 5
   }
    
   return data_def
 
 
def get_general() -> dict:
   """
   Returns:
      设置字典。
   """
 
   print('get_general()')
    
   ret = {}
   ret["ip_addr"] = ip_addr
   ret["port"] = port
   ret["sigcode_err"] = sigcode_err
    
   return ret
 
```
 
``` python
def put_general(body: dict) -> int:
   """
   Args:
      body  设置字典。
    
   Returns:
      0
   """
   global ip_addr, port, sigcode_err
   print('put_general()')
 
   ip_addr = body["ip_addr"]
   port = body["port"]
   sigcode_err = body["sigcode_err"]
 
   save_to_setup_file(body) # 保存到文件
    
   return 0
 
 
def load_from_setup_file() -> int:
   """
   load setup file
   Returns:
         0     ok
         -1    error
   """
   global ip_addr, port, sigcode_err
 
   pathname = xhost.abs_path('project') + fname_setup
   try:
      with open(pathname, 'r') as file:
         data = json.load(file)
         ip_addr = data['ip_addr']
         port = data['port']
         sigcode_err = data['sigcode_err']
   except:
      print('文件未找到: ', pathname)
      return -1
   return 0
 
 
def save_to_setup_file(body: dict) -> int:
   """
   save to setup file
   Returns:
         0     ok
         -1    error
   """
   pathname = xhost.abs_path('project') + fname_setup
   try:
      with open(pathname, 'w') as file:
         json.dump(body, file, indent='\t')
   except:
      print('文件写入失败: ', pathname)
      return -1
   return 0
```
 
``` python
# 属性获取/设置器
def get_ip_addr() -> str:
   return ip_addr
 
 
def set_ip_addr(addr: str):
   global ip_addr
   ip_addr = addr
 
def get_port() -> int:
   return port
 
def set_port(_port: int):
   global port
   port = _port
 
gen_def = get_general_def()
ip_addr : str = gen_def['ip_addr']
port : int = gen_def['port']
sigcode_err = gen_def['sigcode_err']
```
<br>

我们在 `put_general( )` 函数的最后操作中添加了调用 `save_to_setup_file(body)` 以将设置屏幕的值保存到 Python 变量中。

此外，我们在 `main.py` 的 `on_app_init( )` 函数中添加了调用 `setup.load_from_setup_file( )`，如下所示。当 ArgosX 插件被导入时，将调用 `setup.load_from_setup_file( )` 来加载设置。

<br>

main.py
```python 
""" ArgosX 视觉系统接口 - main
 
 
@author:    Jane Doe, BlueOcean Robot & Automation, Ltd.
@created:   2021-12-06
"""
 
from . import setup
from .roblang import *
from .setup import *
from .callback import *
 
import xhost
 
 
def attr_names() -> tuple:
   """返回需要暴露的属性名称。"""
   return ("ip_addr", "port")
 
 
def on_app_init() -> int:
   """(回调) 在自我诊断后立即调用
   Returns:
      0
   """
   print('[argosx] on_app_init();')
   setup.load_from_setup_file()
   xhost.io_assign_set_out_bit(setup.sigcode_err)
   return 0
```
<br>

在这里，让我们重复前一部分进行的测试。

打开 ArgosX 设置屏幕，修改设置值，例如 IP 地址和分配的输出信号，然后通过按 [OK] 键保存它们。

确保在虚拟控制器的 `project/` 文件夹中创建了 `argosx.json` 文件，如下所示。

<br>
argosx.json（将 IP 地址更改为 192.168.1.172，错误的输出分配信号为 fb3.4 的示例）

```json
{
    "port": 54321,
    "ip_addr": "192.168.1.172",
    "sigcode_err": 30004
}
```

<br>
再次运行主程序，并打开 ArgosX 设置屏幕。检查文件中保存的设置值是否正常加载。

![](../../_assets/image_46.png)
[__SOURCE](3-practice-argosx/3-setup-ui/7-f-btn.md)
### 3.3.7 操作 F 按钮 - 初始化为默认值

如在 <U>3.3.1 ArgosX 设置屏幕用户界面的规格</U> 中所述，实现用于将屏幕设置初始化为默认值的 F 按钮。

将 InitButtonBar( ) 函数添加到 setup.js，如下所示。

setup.js
``` js
...前面的步骤省略
 
 
function init()
{
   setDomPath(domPath);
   setUpdateGuideBar(updateGuideBar);
   setUpdateData(updateData);
   onReady();
}
 
 
///@return     f-button infos 数组
function initButtonBar()
{
   console.log('initButtonBar()'); 
   var btn_infos = [
      {
         label: '初始化所有',
         script: 'setAllValueAsDef();'
      },
      {
         label: '初始化一个',
         script: 'setSelectedValueAsDef();'
      }
   ]
   return btn_infos;
}
 
 
..后续步骤省略
```

InitButtonBar( ) 函数返回一个对象数组，定义了 F 按钮的接口。每个对象项由一个属性 label 组成，表示按钮标签，以及一个属性 script，表示在单击按钮时要执行的 JavaScript 代码。

<br>

InitButtonBar( ) 函数的返回值

``` js
[
   {
      label: {button-F1 的标签},
      script: {button-F1 单击时执行的脚本}
   },
   {
      label: {button-F2 的标签},
      script: {button-F2 单击时执行的脚本}
   },
   ....
]
```

上述代码中指定的脚本分别用于调用 setAllValueAsDef( ) 函数和 setSelectedValueAsDef( ) 函数。这些函数是默认由 dst_setup.js 提供的。

如果你想要其他操作，你需要自己实现相关函数。

<br>
让我们测试操作。再次运行虚拟控制器并进入 ArgosX 设置屏幕。之后，改变设置为与默认值不同的值，然后通过单击 [OK] 按钮保存它们。

再次进入设置屏幕，并将光标放在任意元素上。之后，单击初始化一个按钮，检查默认值是否恢复。

此外，单击初始化所有按钮，检查屏幕上的所有值是否恢复为默认值。

![](../../_assets/image_48.png)
[__SOURCE](3-practice-argosx/4-monitoring-panel/README.md)
# 3.4 实用项目：开发 ArgosX 监控面板 UI

应该有一个监控面板，通过分屏向用户显示插件的当前状态。

在这一部分，我们来练习开发一个基于网络的 ArgosX 监控面板。
<br></br>

* ArgosX 监控面板用户界面的规格
* 监控面板的布局
* 操作监控面板
* 将面板项注入到面板菜单
[__SOURCE](3-practice-argosx/4-monitoring-panel/1-concept-if.md)
#### 3.4.1 ArgosX 监控面板用户界面的规格

让我们创建一个监控面板用户界面，以监控信息，如下所示。

* IP 地址
* 端口号
* 分配的错误输入号码
* 请求计数
* 响应计数

更新周期应设置为 500 毫秒。
<br></br>
![](../../_assets/image_49.png)
[__SOURCE](3-practice-argosx/4-monitoring-panel/2-layout.md)
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
[__SOURCE](3-practice-argosx/4-monitoring-panel/3-panel-action.md)
#### 3.4.3 操作监控面板


Python 代码已经包含了 IP 地址、端口号和错误分配输入号码的变量。

因为需要管理请求计数和响应计数，我们需要添加 n_req 和 n_res 变量，如下所示，并将它们连接，以确保它们可以在 get_general( ) 函数中一起返回。



setup.py
``` python 
Previous steps skipped...
 
 
def get_general() -> dict:
   """
   返回:
      设置字典。
   """
 
   print('get_general()')
    
   ret = {}
   ret["ip_addr"] = ip_addr
   ret["port"] = port
   ret["sigcode_err"] = sigcode_err
   ret["n_req"] = n_req
   ret["n_res"] = n_res
    
   return ret
 
 
skipped...
 
 
gen_def = get_general_def()
ip_addr : str = gen_def['ip_addr']
port : int = gen_def['port']
sigcode_err = gen_def['sigcode_err']
n_req = 0   # 请求计数
n_res = 0   # 响应计数
```

将 n_req 和 n_res 的计数操作添加到机器人语言命令的实现中。



roblang.py
```python 
Previous steps skipped...
 
 
def req(work_no: int) -> int:
   """
   发送请求命令到 ArgosX
   例如："req 39"
   参数:
      work_no     工作#    1~100
 
 
   返回:
         >=0   发送的字节数
         -1    无套接字。应该调用 init()。
   """
   msg = "req " + str(work_no)
   setup.n_req += 1                           # <---------- 添加
   return comm.send_msg(msg)
 
 
Skipped...
 
 
def _res_cont(addr_on_timeout: int_or_str) -> str:
   """res() 在持续模式下的实现"""
   val = 0
   msg = ""
   timeout = _check_timeout_and_branch(addr_on_timeout)
   if timeout:
      val = 1
   else:
      msg = comm.recv_msg()
      print(msg)
      if msg=="res fail":
         val = 1
         msg = ""
      elif msg=="":
         xhost.req_to_continue()
      else:
         msg = get_base_shift_array_from_res(msg)
         setup.n_res += 1                    # <---------- 添加
   xhost.io_set_out_bit(setup.sigcode_err, val)
   return msg
```

将以下内容写入 ui/panel.js 文件。内容是关于每 500 毫秒调用一次 updateData( ) 函数，从主板获取所有的通用数据，并通过 display( ) 函数将它们注入到 html 屏幕中。



ui/panel.js
``` js

///@author: Jane Doe, BlueOcean Robot & Automation, Ltd.
///@brief: ArgosX 视觉系统接口 - 面板
///@create: 2021-12-07
 
 
 
function init()
{
   updateData();
   setInterval('updateData()', 500);
}
 
 
///@brief
function updateData()
{
   var path = '/apps/argosx/svr_general';
   var url = domainMb()+path;
   $.get(url, function(data) {
      display(data);
   });
}
 
 
///@return     例如 "http://192.168.1.150:8888"
function domainMb()
{
   var domain = "http://" + window.location.hostname + ":8888";
   console.log(domain);
   return domain;
}
 
 
///@param[in]  data
///@brief      （回调函数）表单 <- 数据
function display(data)
{
   console.log(data);
 
   $('#ip_addr').text(data.ip_addr);
   $('#port').text(data.port);
   $('#sigcode_err').text(data.sigcode_err);
   $('#n_req').text(data.n_req);
   $('#n_res').text(data.n_res);
}
```

再次运行虚拟控制器，然后再次在 vscode 中执行 Go Live。现在，您可以看到在网页浏览器的屏幕上打印的值如下。

让我们通过执行 ArgosX stub 和运行带虚拟教鞭的作业程序来执行请求和响应。如果 n.request 和 n.response 的值增加，这意味着操作正常。
<br></br>
![](../../_assets/image_53.png)
[__SOURCE](3-practice-argosx/4-monitoring-panel/4-menu.md)
#### 3.4.4 将面板项目注入面板菜单

让我们将 ArgosX 监控功能注入面板菜单。

在 ui/menu.json 文件中添加一个面板项目，如下所示。

menu.json
``` json
[
   {
       "path": "system/appl/",
       "id": "argosx",
       "icon": "argosx/ui/lm_argosx.png",
       "label": "ArgosX 视觉",
       "url": "argosx/ui/setup.html"
   },
   {
      "path": "panels",
      "id": "argosx",
      "icon": "argosx/ui/panel_argosx.png",
      "label": "ArgosX 视觉",
      "url": "argosx/ui/panel.html"
  }
]
```

让我们制作一个将被注入面板菜单的图标。一个透明的 40 x 40 像素背景的 png 图标就足够了。

在这里，我们只是通过适当地缩小现有的 lm_argosx.png 并调整颜色来制作 panel_argosx.png。


![](../../_assets/panel_argosx.png)
panel_argosx.png 的示例（您可以下载并使用此图片。）


现在，让我们运行虚拟主板和虚拟示教器。


当您打开面板菜单以添加新面板项目时，可以看到新添加的 ArgosX 视觉菜单项。  
![](../../_assets/image_54.png)




当您选择菜单时，我们制作的监控面板将很快出现。  
<br></br>
![](../../_assets/image_55.png)




让我们通过操作虚拟示教器执行请求和响应。如果 n.request 和 n.response 值增加，说明操作正常。
[__SOURCE](3-practice-argosx/5-user-bar/README.md)
# 3.5 实用项目：开发 ArgosX 用户栏 UI

在前面的章节中，我们学习了如何使用机器人语言执行插件中实现的功能。

但是，在某些情况下，可能需要提供一个功能，让用户直接操作 UI 以调用插件的功能。这些操作按钮可以在设置窗口或监控窗口中提供。然而，用户栏更适合在教学屏幕上快速操作。
<br></br>
在本章中，让我们练习开发基于网页的 ArgosX 用户栏 UI。

* ArgosX 用户栏用户界面的规格
* 用户栏的布局
* 操作用户栏
* 注入用户栏
[__SOURCE](3-practice-argosx/5-user-bar/1-concept-if.md)
#### 3.5.1 ArgosX 用户条用户界面的规格

多次按下用户键按钮将切换用户条。

让我们创建一个用户条 UI，以提供如下的 UI。

- 开灯按钮：打开 ArgosX 的 LED 灯。
- 关灯按钮：关闭 ArgosX 的 LED 灯。
<br></br>

![](../../_assets/image_56.png)
[__SOURCE](3-practice-argosx/5-user-bar/2-layout.md)
#### 3.5.2 用户栏的布局

打开 vscode，选择 ArgosX 文件夹的父文件夹 apps/ 。

创建 ui/ubar.html 和 ui/ubar.js 文件。
<br>![](../../_assets/image_57.png)

在下面写入内容，它是由两个按钮组成的简单布局。表的第一列被赋予了一个名为 'ubar-bt' 的类，该类在 common style.css 中定义。它会自动识别 TP600 和 TP630，并为按钮的大小和颜色提供类似于默认 UI 的样式。如果你想更改样式，可以使用单独的本地 css 定义一个类并应用它。

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
   <button id='light-on' class='ubar-bt' onclick='light_onoff(true);'>light<br>打开</button>
   <button id='light-off' class='ubar-bt' onclick='light_onoff(false);'>light<br>关闭</button>
</body>
</html>
```

当 ubar.html 打开时，如果你点击右下角的 Go Live 按钮执行实时服务器，Google Chrome 浏览器将打开。
<br>![](../../_assets/image_58.png)

尽管 ubar.js 中尚无内容，但我们可以检查布局是否正常。
<br>![](../../_assets/image_59.png)
[__SOURCE](3-practice-argosx/5-user-bar/3-usrbar-action.md)
#### 3.5.3 操作用户工具条

##### 客户端 (教学挂件)

在 ui/ubar.js 文件中写入以下内容。每次按下按钮时，教学挂件将会向主板发送一个名为 "light_onoff" 的 HTTP post 消息。开或关的状态将包含在一个具有 onoff 属性的对象中，并将加载到 post 消息的主体中发送。

- 灯光开启状态的主体: { onoff: true }
- 灯光关闭状态的主体: { onoff: false }

ui/ubar.js
``` js
///@author: Jane Doe, BlueOcean Robot & Automation, Ltd.
///@brief: ArgosX Vision System interface - setup general
///@create: 2021-12-07
 
 
 
///@return     e.g. "http://192.168.1.150:8888"
function domainMb()
{
   var domain = "http://" + window.location.hostname + ":8888";
   console.log(domain);
   return domain;
}
 
 
///@param[in]  onoff    true or false
///@return
///      -  0     ok
///@brief      请求灯光开/关
function light_onoff(onoff)
{
   var url = domainMb()+"/apps/argosx/svr_light_onoff";
   var args = { onoff: onoff };
 
   args = JSON.stringify(args);
   $.ajax({
      url: url,
      type: 'post',
      dataType: 'json',
      contentType : "application/json; charset=utf-8",
      data: args,
      success: function(res) {
         console.log('success' + res);
      },
      error : function(res) {
         console.log('error' + res);
      }
   });
 
   return 0;
}
```

##### 服务器端 (主板)

现在，主板的 ArgosX 插件需要接收此消息并向实际的 ArgosX 设备发送 "light-on" 和 "light-off" 消息（使用桩进行测试）。

将 ubar.py 文件写入 argosx/ 文件夹，如下所示。关于其实现，将使用 comm_ex 模块，类似于我们在先前章节中执行的回调实现。
``` python
""" ArgosX Vision System interface - main
 
 
@author:    Jane Doe, BlueOcean Robot & Automation, Ltd.
@created:   2021-12-07
"""
 
 
from . import comm_ex
 
 
def post_light_onoff(body: dict) -> int:
   """
   参数:
      onoff
    
   返回:
      0
   """
   onoff = body['onoff']
   msg = 'light-' + ('on' if onoff else 'off')
   print(msg)
       
   return comm_ex.send_msg_once(msg)
```

最后一步，将 ubar.py 的所有函数导入 main.py。

main.py
``` python
""" ArgosX Vision System interface - main
 
 
@author:    Jane Doe, BlueOcean Robot & Automation, Ltd.
@created:   2021-12-06
"""
 
from . import setup
from .roblang import *
from .setup import *
from .callback import *
from .ubar import *
 
import xhost
 
 
后续步骤略去...
```

重启虚拟控制器，然后运行作业文件直到 argosx.init( )。再次执行 Go Live 以在网络浏览器上调出用户工具条屏幕。

执行 ArgosX 桩，然后在网络浏览器上操作按钮。如果桩的控制台输出显示为 "light-on" 和 "light-off"，则意味着操作正常。
<br>
![](../../_assets/image_60.png)

ArgosX 桩的控制台输出
<br>
![](../../_assets/image_61.png)
[__SOURCE](3-practice-argosx/5-user-bar/4-inset-usrbar.md)
#### 3.5.4 注入用户工具条

让我们将 ArgosX 用户工具条注入到实际的教学挂件中。

将 "ubars" 路径项添加到 ui/menu.json 文件中，如下所示。

menu.json
``` json
[
    {
         "path": "system/appl/",
         "id": "argosx",
         "icon": "argosx/ui/lm_argosx.png",
         "label": "ArgosX 视觉",
         "url": "argosx/ui/setup.html"
    },
    {
        "path": "panels",
        "id": "argosx",
        "icon": "argosx/ui/panel_argosx.png",
        "label": "ArgosX 视觉",
        "url": "argosx/ui/panel.html"
    },
    {
        "path": "ubars",
        "id": "argosx",
        "url": "argosx/ui/ubar.html"
    }
]
```

按下虚拟教学挂件右侧的用户键按钮将依次显示安装的应用程序的用户工具条。现在你也可以看到我们为 ArgosX 创建的用户工具条。操作按钮以检查 ArgosX 桩是否正常响应。

![](../../_assets/image_62.png)
[__SOURCE](3-practice-argosx/6-translation/README.md)
# 3.6 实际项目：ArgosX - 本地化（多语言支持）

到目前为止，示例集中于基于英语的用户界面开发。  
然而，${cont_model} 控制器支持除英语外的多种语言。

因此，插件应用程序也可以通过本地化提供多种语言。

现在，让我们练习将本地化应用于 ArgosX 项目。

* 设置屏幕用户界面的本地化  
* 监控面板用户界面的本地化  
* 用户栏用户界面的本地化
[__SOURCE](3-practice-argosx/6-translation/1-update-setup/README.md)
### 3.6.1 设置屏幕用户界面的本地化

首先，让我们开始设置屏幕用户界面的翻译工作。

* 设置屏幕菜单的翻译
* 设置屏幕用户界面的翻译
* F-button 用户界面的翻译
[__SOURCE](3-practice-argosx/6-translation/1-update-setup/1-setup-menu.md)
#### 3.6.1.1 翻译设置屏幕菜单

##### 注册字符串表

要注册本地化资源，您必须创建一个字符串表。  
它必须以 JSON 格式注册，并按如下所示添加。

1) 在 argosx 项目的 ui 文件夹中添加文件 str_table.json。

    ![](../../../_assets/image_85.png)

2) 内容

```json
{
    "en":
    {
        "IDS_title" : "ArgosX Vision System"
    },
    "ko":
    {
        "IDS_title" : "ArgosX Vision System"
    },
    "zh":
    {
        "IDS_title" : "ArgosX 视觉系统"
    }
}
````

"en" 和 "ko" 是兼容 ${cont_model} 的语言代码（以下简称 langcode）。  
"en" 代表英语，"ko" 代表韩语，"zh" 代表中文。

每个 langcode 包含由字符串 id 和其相应字符串值组成的成员。

要在菜单中添加标题，请按照上述格式将相同 id "IDS_title" 的字符串数据添加到 "en"、"ko" 和 "zh"。

<br>

##### 翻译菜单标签

设置屏幕菜单上的标签也必须翻译。

请按照以下步骤操作：

1. info.json

修改现有的 info.json 文件。

添加创建的 str_table.json 文件作为 "strs" id 的值。

```json
{
    "author" : "BlueOcean Robot & Automation, Ltd.",
    "binding" : "plug-in",
    "cmds" : "cmds.json",
    "copyright" : "All right reserved",
    "description" : "ArgosX Vision System interface",
    "entry" : "main.py",
    "menu" : "ui/menu.json",
    "strs" : "ui/str_table.json",
    "startup" : "boot",
    "version" : "v0.9.0"
}
```

2. menu.json

修改现有的 menu.json 文件。

```json
{
    "path": "system/appl/",
    "id": "argosx",
    "icon": "argosx/ui/lm_argosx.png",
    "label": "IDS_title",
    "url": "argosx/ui/setup.html"
}
```

将标签值设置为 "IDS_title"，而不是固定文本 "ArgosX Vision System"。

<br>

3. 语言选择

在虚拟控制器环境中，您必须修改 hi6tp_platform_cfg.json 中的 "lang_code" 值。

```json
"lang_code": "en"
```

更改 lang_code 后，重启控制器和 TP 以验证更新的语言和菜单标签。

如果应用正确，菜单现在应以中文显示。

![](../../../_assets/image_86.png)
[__SOURCE](3-practice-argosx/6-translation/1-update-setup/2-setup-ui.md)
#### 3.6.1.2 翻译设置屏幕用户界面

##### 设置布局的更改

要翻译设置屏幕的用户界面，首先打开并查看 setup.html。

添加 str_table.json 和 lang.js 作为脚本文件，如下所示。

因为这些文件彼此依赖，所以必须按以下顺序包含它们。

```html
<script src='./str_table.json' type='application/json'></script>
<script src='../../_common/js/lang.js'></script>
<script src='../../_common/js/dst_setup.js'></script>
```

此外，检查在 body 内声明的内容：

```html
<span class='col0' name='ip_addr'>IP 地址</span>
```

您可以移除文本 "IP 地址"。  
（翻译后的文本将在稍后插入。）

在应用上述所有更改后，html 文件应如下所示：

setup.html

```html
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
    <script src='./str_table.json' type='application/json'></script>
    <script src='../../_common/js/lang.js'></script>
    <script src='../../_common/js/dst_setup.js'></script>
    <script src='./setup.js'></script>
    <script>
        $(document).ready(init);
    </script>
</head>

<body class='no-scroll'>
<div>
    <div id='contents'>
            <span class='col0' name='ip_addr'></span>
            <input class='col1' type='text' name='ip_addr' id='ip_addr_0' size='3'/>
            .
            <input class='col1' type='text' name='ip_addr' id='ip_addr_1' size='3'/>
            .
            <input class='col1' type='text' name='ip_addr' id='ip_addr_2' size='3'/>
            .
            <input class='col1' type='text' name='ip_addr' id='ip_addr_3' size='3'/>
            <br>
            <span class='col0' name='port'></span>
            <input class='col1' type='text' id='port' size='5'/>
            <br>
            <span class='col0' name='sigcode_err'></span>
            <input class='col1' type='text' id='sigcode_err' size='5'/>
        </div>
        <div id='guidebar'></div>
</div>
</body>
</html>
```

<br>

##### 为设置添加翻译功能

现在让我们为设置屏幕添加翻译功能。

<br>

1) 初始化

在初始化期间，添加加载来自 str_table.json 的数据的逻辑，并将从 ${cont_model} 读取的 lang_code 应用到平台的本地化系统。

setup.js

```js
function init()
{
    parseStrData();
    setDomPath('/apps/argosx/svr_general');
    setLangCode('/apps/argosx/svr_lang_code', updateAllStrByLang);
    setUpdateData(updateData);
    onReady();
}
```

parseStrData 加载字符串数据。

setLangCode 调用 Python 函数以读取在 ${cont_model} 中配置的 lang_code，并将 updateAllStrByLang 设置为回调。

为了支持 setLangCode，在 main.py 中添加 get_lang_code 函数。

它被添加到 main.py，以便 ubar 和面板可以共享同样的 lang_code。

main.py

```python
def get_lang_code()->dict:
    """ 从远程获取语言代码

    返回: data: 语言代码信息
    """
    data = {}
    lang_code = xhost.lang_code()
    print(lang_code)
    data["lang_code"] = lang_code
    return data
```

此函数使用 xhost.lang_code() 返回 lang_code。

<br>

2) 根据 lang_code 应用翻译

回调函数 updateAllStrByLang 调用 updateElement 和 updateGuideBarMsg。  
这些函数根据 lang_code 翻译元素和导引栏消息。

首先，将所需的元素和导引栏消息添加到 str_table.json。

```json
{
    "en":
    {
        "IDS_title" : "ArgosX Vision System",
        "IDS_IpAddr" :"IP Address",
        "IDS_Port" : "Port#",
        "IDS_OUtSigcodeErr" : "Failure output signal",
        "IDS_msg_ip_addr" : "Enter the IP address of ArgosX.",
        "IDS_msg_port" : "Enter the port # of ArgosX.",
        "IDS_msg_sigcode" :"Enter the number of the signal to assign.[0 - 4096]"
    },
    "ko":
    {
        "IDS_title" : "ArgosX Vision System",
        "IDS_IpAddr" :"IP Address",
        "IDS_Port" : "Port#",
        "IDS_OUtSigcodeErr" : "Failure output signal",
        "IDS_msg_ip_addr" : "Enter the IP address of ArgosX.",
        "IDS_msg_port" : "Enter the port # of ArgosX.",
        "IDS_msg_sigcode" :"Enter the number of the signal to assign.[0 - 4096]"
    }
}
```

接下来，按如下方式修改 setup.js。

删除现有的 updateGuideBar 函数，改为使用 updateGuideBarMsg。

```js
/// @brief update all string by language code
function updateAllStrByLang()
{
    updateElement();
    updateGuideBarMsg();
}

/// @brief update all element by language code
function updateElement()
{
    let se = setElemByLang;
    se('ip_addr', 'IDS_IpAddr');
    se('port', 'IDS_Port');
    se('sigcode_err', 'IDS_OUtSigcodeErr');
}

/// @brief display guidebar message on clicking widget & update message by langcode
function updateGuideBarMsg()
{
    let sg = setGuideMsgByLang;
    sg('ip_addr', 'IDS_msg_ip_addr');
    sg('port', 'IDS_msg_port');
    sg('sigcode_err', 'IDS_msg_sigcode');
}
```

使用 setElemByLang 和 setGuideMsgByLang 为每个元素和导引栏消息分配字符串 ID。

在重新启动虚拟控制器和 TP 后，翻译后的设置屏幕应正确显示。

![](../../../_assets/image_87.png)
[__SOURCE](3-practice-argosx/6-translation/1-update-setup/3-f-btn.md)
#### 3.6.1.3 翻译 F 按钮 UI

让我们为 F 按钮 UI 添加翻译支持。

<br>

##### 添加字符串数据

为每种语言代码将 F 按钮标签的字符串数据添加到 str_table.json。

```json
"en":
{
    "IDS_msg_lb_all" : "Initialize\nAll",
    "IDS_msg_lb_one" : "Initialize\nOne"
},
"ko":
{
    "IDS_msg_lb_all" : "Initialize\nAll",
    "IDS_msg_lb_one" : "Initialize\nOne"
}
```

每个标签文本通过字符串 ID 注册，以便根据所选语言进行显示。

<br>

##### F 按钮行为

修改在现有的 initButtonBar 函数中定义的 btn_infos 内的标签值，使其使用字符串 ID 而不是固定文本。

setup.js

```js
/// @return f-button infos array
function initButtonBar()
{
    console.log('initButtonBar()');

    var btn_infos = [
        {
            label: "IDS_msg_lb_all",
            script: 'setAllValueAsDef();'
        },
        {
            label: "IDS_msg_lb_one",
            script: 'setSelectedValueAsDef();'
        }
    ];

    return btn_infos;
}
```

将原始硬编码标签替换为相应的字符串 ID。

在重新启动虚拟控制器和 TP 后，F 按钮将使用根据所选语言翻译的文本显示。

![](../../../_assets/image_88.png)
[__SOURCE](3-practice-argosx/6-translation/2-update-panel.md)
#### 3.6.2 监控面板用户界面本地化

接下来，让我们开始监控面板的翻译工作。

<br>

##### 1. 菜单翻译

面板用户界面也需要翻译菜单。

要在监控面板菜单中显示面板屏幕标签，请添加一个 id。  
重用之前定义的 "IDS_title"。

menu.json

将现有的 menu.json 修改如下：

```json
{
    "path": "panels",
    "id": "argosx",
    "icon": "argosx/ui/panel_argosx.png",
    "label": "IDS_title",
    "url": "argosx/ui/panel.html"
}
```

将标签值设置为 "IDS_title" 而不是固定文本 "ArgosX Vision System"。

如果正确应用，翻译后的标签将出现在监控面板菜单中。

![](../../_assets/image_89.png)

<br>

##### 2. 面板布局的更改

要翻译监控屏幕用户界面，首先检查 panel.html。

与设置屏幕一样，添加 str_table.json 和 lang.js 作为脚本文件。

由于这些文件相互依赖，您必须按以下顺序包含它们。

```html
<script src='./str_table.json' type='application/json'></script>
<script src='../../_common/js/lang.js'></script>
```

接下来，检查主体中定义的表格：

```html
<table>
    <th id='name'></th>
    <th id='value'></th>
    <tr>
        <td class='thd' id='lb_ip_addr'></td>
        <td id='ip_addr'></td>
    </tr>
    <tr>
        <td class='thd' id='lb_port'></td>
        <td id='port'></td>
    </tr>
    <tr>
        <td class='thd' id='lb_sigcode_err'></td>
        <td id='sigcode_err'></td>
    </tr>
    <tr>
        <td class='thd' id='lb_n_req'></td>
        <td id='n_req'></td>
    </tr>
    <tr>
        <td class='thd' id='lb_n_res'></td>
        <td id='n_res'></td>
    </tr>
</table>
```

步骤：

1) 为每个表头 (th) 和单元格 (td) 分配一个 id 以进行翻译。  
2) 您可以删除任何固定文本，比如 "IP address"，因为翻译内容将动态插入。

在应用这些更改后，html 文件应如下所示：

panel.html

```html
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
    <script src='./str_table.json' type='application/json'></script>
    <script src='../../_common/js/lang.js'></script>
    <script src='./panel.js'></script>
    <script>
        $(document).ready(init);
    </script>
</head>

<body>
<table>
    <th id='name'></th>
    <th id='value'></th>
    <tr>
        <td class='thd' id='lb_ip_addr'></td>
        <td id='ip_addr'></td>
    </tr>
    <tr>
        <td class='thd' id='lb_port'></td>
        <td id='port'></td>
    </tr>
    <tr>
        <td class='thd' id='lb_sigcode_err'></td>
        <td id='sigcode_err'></td>
    </tr>
    <tr>
        <td class='thd' id='lb_n_req'></td>
        <td id='n_req'></td>
    </tr>
    <tr>
        <td class='thd' id='lb_n_res'></td>
        <td id='n_res'></td>
    </tr>
</table>
</body>
</html>
```

<br>

##### 3. 添加面板翻译行为

现在，让我们为面板屏幕添加翻译行为。

<br>

1) 初始化

在初始化期间，从 str_table.json 加载字符串数据并应用 ${cont_model} 的 lang_code。

panel.js

```js
function init()
{
    parseStrData();
    setLangCode('/apps/argosx/svr_lang_code', updateAllStrByLang);
    updateData();
    setInterval('updateData()', 500);
}
```

如同 setup.js，添加 parseStrData 和 setLangCode。

<br>

2) 根据 lang_code 应用翻译

保持现有字符串数据的使用，并向 str_table.json 添加其他所需元素。

```json
"en":
{
    "IDS_InSigcodeErr" : "sigcode for error",
    "IDS_NReq" : "n.request",
    "IDS_NRes" : "n.response",
    "IDS_Name" : "name",
    "IDS_Value" : "value"
},
"ko":
{
    "IDS_InSigcodeErr" : "error signal assignment number",
    "IDS_NReq" : "request count",
    "IDS_NRes" : "response count",
    "IDS_Name" : "name",
    "IDS_Value" : "value"
}
```

接下来，将 panel.js 修改如下：

```js
function updateAllStrByLang()
{
    let se = setElemByLang;
    se('lb_ip_addr', 'IDS_IpAddr');
    se('lb_port', 'IDS_Port');
    se('lb_sigcode_err', 'IDS_InSigcodeErr');
    se('lb_n_req', 'IDS_NReq');
    se('lb_n_res', 'IDS_NRes');
    se('name', 'IDS_Name');
    se('value', 'IDS_Value');
}
```

由于面板只需要更新元素名称，因此只需在 updateAllStrByLang 中使用 setElemByLang 分配字符串 ID。

重新启动虚拟控制器和 TP 后，翻译后的监控面板屏幕应正确显示。

![](../../_assets/image_90.png)
[__SOURCE](3-practice-argosx/6-translation/3-update-userbar.md)
#### 3.6.3 用户栏 UI 本地化

##### 1. 用户栏布局的变化

要翻译用户栏 UI，首先打开并查看 ubar.html。

如前面的步骤一样，按如下所示添加 str_table.json 和 lang.js 作为脚本文件。

因为这些文件相互依赖，所以必须按照以下顺序包含它们。

另外，由于我们将在 ubar.html 中定义一个名为 init 的初始化函数（之前不存在），请按如下所示编写。

ubar.html

```html
<script src='./str_table.json' type='application/json'></script>
<script src='../../_common/js/lang.js'></script>
<script src='./ubar.js'></script>
<script>
    $(document).ready(init);
</script>
```

接下来，检查在 body 内声明的按钮：

```html
<button id='light-on' class='ubar-bt' onclick='light_onoff(true);'>light<br>on</button>
<button id='light-off' class='ubar-bt' onclick='light_onoff(false);'>light<br>off</button>
```

您可以删除现有文本，例如 "light on"。  
（翻译后的文本将稍后动态插入。）

在应用上述所有更改后，html 文件应如下所示：

ubar.html

```html
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
    <script src='./str_table.json' type='application/json'></script>
    <script src='../../_common/js/lang.js'></script>
    <script src='./ubar.js'></script>
    <script>
        $(document).ready(init);
    </script>
</head>

<body class='ubar'>
    <div class='ubar-title'>argosx</div>
    <button id='light-on' class='ubar-bt' onclick='light_onoff(true);'></button>
    <button id='light-off' class='ubar-bt' onclick='light_onoff(false);'></button>
</body>
</html>
```

<br>

##### 2. 添加用户栏翻译行为

现在让我们添加翻译功能到用户栏界面。

<br>

1) 初始化

在初始化期间，从 str_table.json 加载字符串数据，并将从 ${cont_model} 读取的 lang_code 应用到平台的本地化系统。

ubar.js

```js
function init()
{
    parseStrData();
    setLangCode('/apps/argosx/svr_lang_code', updateAllStrByLang);
}
```

如前所述，添加 parseStrData 和 setLangCode。

<br>

2) 根据 lang_code 应用翻译

将所需的元素添加到 str_table.json。

```json
"en":
{
    "IDS_light_on" : "light on",
    "IDS_light_off" : "light off"
},
"ko":
{
    "IDS_light_on" : "Light ON",
    "IDS_light_off" : "Light OFF"
}
```

接下来，修改 ubar.js 如下：

```js
function updateAllStrByLang()
{
    setElemByLang('light-on', 'IDS_light_on');
    setElemByLang('light-off', 'IDS_light_off');
}
```

由于用户栏只需要更新元素标签，因此只需使用 setElemByLang 分配字符串 ID。

在重启虚拟控制器和 TP 后，翻译后的用户栏界面应正确显示。

![](../../_assets/image_91.png)
[__SOURCE](4-debug/README.md)
# 4. 调试

* 调试 Python 代码
* 调试基于网络的 UI
[__SOURCE](4-debug/1-debug-python.md)
# 4.1 调试 Python 代码

如果我们编写的 Python 代码在 Web UI 中有语法错误或在实现主体中有逻辑错误，则应使用调试器来追踪原因并进行补充。

然而，插件的 Python 源代码是通过 Import 操作、回调操作或通过机器人语言从 ${cont_model} 主机调用的。由于 ${cont_model} 主机中没有内置的 Python 调试器，因此通过在主机的调用中放置断点进行追踪是不可能的。但是，您可以通过使用外部调试器执行插件的入口函数来进行部分调试。

<br>

#### 用于调试的 xhost
xhost 是一个由插件使用的模块，用于调用 ${cont_model} 主机的函数。它由主机创建并注入到插件中。下面的图展示了主机和插件之间的典型操作流程。
<br> ![](../_assets/image_63.png)

但是，当使用 vscode 调试器执行插件时，调用 xhost 方法时会发生执行错误，因为没有实际创建/注入 xhost 模块。因此，应使用用于调试的替代 xhost。在 SDK 的公共文件夹中，有一个替代模块 xhost_dbg。它是一种通过以太网调用 OpenAPI 执行主机操作的代理。

 ![](../_assets/image_64.png)
<br>

您看到以下内容是 apps/ 文件夹中导入 xhost_dbg 的简单 xhost.py。当实际的 ${cont_model} 主机创建/注入 xhost 时，它将覆盖此 xhost 替代模块。

xhost.py
``` python
""" xhost for debug
   This module will be overridden by host.
"""
from _common.py.xhost_dbg import *
```

主机可以是运行在 PC 开发环境中的 ${cont_model} 虚拟控制器或实际控制器。您需要指定 xhost_dbg 应该访问的位置。

在 ${cont_model} 家庭路径的 apps/ 文件夹中有 xhost_remote_ip.py 文件。默认值设置为 PC 开发环境本身（即 ${cont_model} 虚拟控制器），如下所示。

xhost_remote_ip.py
``` python
remote_ip="127.0.0.1"
```

如果您想通过 IP 地址为 "192.168.1.150" 访问实际的 ${cont_model} 控制器进行测试，则可以按如下设置。

xhost_remote_ip.py
``` python
remote_ip="192.168.1.150"
```

#### 使用 Visual Studio Code 和 test.py 调试
如果在<u> 1.5 安装 Visual Studio Code</u> 中正确安装了 Microsoft Python 扩展，则可以使用 Python 调试环境。如果您已经熟悉在 vscode 中调试 Python，则可以跳过此部分。

为了追踪机器人语言或回调操作要调用的函数，我们在 argosx/ 文件夹下定义了测试模块，如下图所示。实现需要测试的例程，然后从主例程中调用 test( ) 函数。

您可以通过单击行号的左侧来切换断点。

test.py
<br></br>
![](../_assets/image_65.png)

当您在 test.py 窗口中按下 F5 键或单击运行 - 开始调试菜单时，将在 Python 运行时和调试模式下执行。
<br></br>
![](../_assets/image_66.png)

对于上一部分中的 ArgosX 插件，您应该执行 argosx_stub 以进行测试。如果您需要运行两个 Python 程序，如此情况，则需要打开两个 vscode 会话，并在每个会话中运行调试器，如下所示。
<br></br>
![](../_assets/image_67.png)

当放置断点时，左侧将打开 VARIABLES、WATCH、CALL STACK 和 BREAKPOINTS 窗口。您可以使用右上角的操作按钮进行跟踪：继续 (F5)、逐步跳过 (F10)、逐步进入 (F11) 和逐步退出 (Shift+F11)。如果单击重启 (Ctrl+Shift+F5) 按钮，则执行将从头开始。如果单击停止 (Shift+F5) 按钮，则调试将停止。

当调用 print( ) 命令时，一个内置的 Python 函数将在底部的 TERMINAL 窗口中打印字符串，可以用于调试。
<br></br>
![](../_assets/image_68.png)

test.py 的操作通过 xhost_dbg 与控制器主机相互联动，因此可以在检查实际操作的同时进行调试，如下所示。
<br></br>
![](../_assets/image_69.png)
[__SOURCE](4-debug/2-debug-ui.md)
# 4.2 调试基于Web的UI

在教学挂件上测试Web UI之前，我们可以先使用Google Chrome浏览器预测试屏幕。

如果我们编写的Web UI在JavaScript中存在语法错误或实现主体中的逻辑错误，则应使用调试器跟踪原因并进行补充。此外，Google Chrome浏览器中有一个名为Chrome开发工具（简称Chrome DevTools）的内置调试器，因此您可以使用它。在本节中，我们将学习如何调试Web UI。如果您已经熟悉使用Chrome DevTools，可以跳过本节。

#### 使用实时服务器执行Web UI

在<u>设置屏幕的布局</u>部分，我们之前已经练习过使用Google Chrome浏览器执行ArgosX插件的setup.html。

让我们再次执行它。

虚拟控制器的ArgosX应该处于正常导入状态。

在vscode中打开setup.html时，您需要单击右下角的Go Live按钮以运行实时服务器。

![](../_assets/image_70.png)

或者，您可以通过右键单击setup.html打开弹出菜单，然后选择“使用实时服务器打开”。

![](../_assets/image_71.png)

如您所见，setup.html已在Chrome浏览器中打开。

![](../_assets/image_72.png)

#### Chrome DevTools

当您按下F12按钮时，Chrome DevTools将在浏览器的右侧打开。在下图中，选择了DevTools顶部的Console菜单。当在JavaScript中使用console.log()调用字符串时，该字符串将打印在此控制台窗口中。在此过程中，调用log( )的源代码位置也会显示出来，您可以单击以跳转到源代码位置，这使调试器在调试时非常有用。

![](../_assets/image_73.png)

从DevTools顶部的菜单中选择Elements可以查看html文件的层次结构。当您在html文件中选择特定元素时，相关元素将在左侧渲染屏幕上高亮显示，而CSS样式将显示在右下角。

![](../_assets/image_74.png)

选择Sources菜单将使JavaScript源代码文件以树状结构出现。您可以选择并打开所需的文件并检查源代码。您还可以通过单击左侧的行号切换断点。

![](../_assets/image_75.png)

单击左侧html渲染屏幕上的Update按钮或按F5键将更新Web UI，并使JavaScript从头开始运行。您可以看到执行游标停在断点处。您还可以在图底部看到Breakpoints和Call Stack窗口，并且可以通过单击相关项目跳转到源代码位置。

右下角的Scope菜单显示局部变量，如果您在Scope菜单右侧选择Watch菜单，您可以添加和观察所需的变量。

对于集成开发环境的用户，如Visual Studio或Eclipse，这个环境将是一个非常熟悉的调试环境。

![](../_assets/image_76.png)

![](../_assets/image_77.png)

当放置断点时，可以执行跟踪。您可以使用Breakpoints窗口上方的操作按钮组执行Resume (F8)、Step over (F10)、Step into (F11) 和 Step out (Shift+F11)操作。

![](../_assets/image_78.png)

#### 修改和重新执行源代码

在修改了html、CSS和JavaScript的源代码后，如果您在Web浏览器中单击Update按钮或按F5键，则将基于修改的内容执行。

此外，由于Web浏览器具有缓存，即使源代码被修改并重新执行，修改也可能未反映。在这种情况下，您可以通过右键单击Update按钮打开弹出菜单，然后选择“清除缓存和强制刷新”菜单来解决问题（此弹出菜单仅在DevTools打开时可用）。

![](../_assets/image_79.png)

刷新也可以在虚拟教学挂件中进行，而不是在Web浏览器中。右键单击Web U/I屏幕将打开弹出窗口。如果您在这里选择Reload菜单，则在源代码中进行的修改将立即反映出来。

![](../_assets/image_80.png)
[__SOURCE](5-installer/README.md)
# 5. 安装程序

(to be written in the future)