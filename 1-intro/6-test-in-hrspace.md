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