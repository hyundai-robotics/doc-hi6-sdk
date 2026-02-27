# 1.6 在 HRSpace 中测试插件

您可以在 HRSpace 的虚拟控制器和虚拟教学示教器上测试您自己的插件应用程序。

{% hint style="warning" %}   
<b>与实际控制器环境存在差异。</b> 请仅在开发阶段进行简单的功能测试，并确保在应用插件之前在真实环境中进行全面测试。对于未考虑此信息而导致的任何损害或问题，我们不承担任何责任。  
{% endhint %}  

<br>

#### 1.6.1 HRSpace 安装环境
- 操作系统：Windows 64 位

<br>

#### 1.6.2 HRSpace 安装过程

1. 访问 HD 韩华机器人网站，如果尚未注册，请注册。
2. 打开 HRSpace 下载页面。
3. 安装最新版本（截至文档日期：v3.95b10）。
4. 解压已下载的 zip 文件。
5. 运行安装程序 (HRSpace3.msi) → 选择语言 → 选择安装位置 → 完成安装并退出。

<br>

#### 1.6.3 运行 HRSpace

#### a. 加载机器人模型
1. 按 `Windows 键` > 输入 `HRSpace3_eng` > 单击 > 运行程序。
2. 在左侧工作区面板中右键单击 `workspace component` > 单击 `Load Model as a Child...` > 单击 `机器人 (Robot)` 文件夹 > 选择所需模型。  
   
   <img src="../_assets/hrspace/01_select_robot_model.png" height="400hw">


   <p style="background-color: orange; color: black; width:max-content"><b>为避免错误，仅应加载位于 ${HRSpace installation folder}\VRC_${cont_model}\fbrr 的模型。</b></p>

3. 机器人控制器 (RC) 类型选择弹出窗口 > 单击 VRC_${cont_model} > 确认 > 加载机器人。  
   
   <img src="../_assets/hrspace/02_rc_type_popup.PNG" height=250hw>

4. 机器人已加载  

   <img src="../_assets/hrspace/03_robot_loaded.PNG" height=350hw>


#### b. 保存工作空间
1. 
   <img src="../_assets/hrspace/00_save_btn.PNG" height=50vw> 单击任务栏上的 `保存 (Save)`。 

2. 在所需位置创建新文件夹并保存文件。
Example: 点击文件资源管理器地址栏中的“HRSpace3”进行导航 > 右键单击空白区域 > 新建 > 文件夹 > 创建一个临时文件夹 > 创建一个名为temp.hrs的模型 > 保存后，保存的文件名将在标题栏显示 > 点击任务栏上的`保存 (Save)`。  
<img src="../_assets/hrspace/04_temp_hrs.PNG" height=150vw>  


#### c. 运行虚拟教学挂件

1. 右键单击左侧工作区面板中创建的机器人模型 > 点击虚拟教学挂件。
   <img src="../_assets/hrspace/05_start_virtual_tp.PNG" height=500vw>

   <img src="../_assets/hrspace/06_tp_imp.PNG" height=500vw>

2. 退出教学挂件的两种方法

   1. TP 首页 - `[F1: 服务] - 9: 退出 TP 应用程序 ([F1: Service] - 9: Exit TP application)`
    
   2. 右键单击键盘区域 - 点击 `关闭 (close)`

<br>

#### 1.6.4 在虚拟教学挂件上运行插件
1. 创建一个 hello-world 示例插件，并将其注入到 HRSpace 的虚拟教学挂件中。此过程参考了 [HRBook 手册](../2-example-helloworld/README.md)。
   hello_world
    ├── cmds.json
    ├── hello_world.py
    ├── info.json
    └── ui
        ├── lm_hello.png
        ├── menu.json
        └── setup.html

1. 将开发的 hello-world 插件保存在以下路径中。
   ${HRSpace installation folder}\VRC_${cont_model}\apps
   例如) C:\Program Files\HHI Robotics\HRSpace3\VRC_${cont_model}\apps


3. 检查保存插件的位置。  

   <img src="../_assets/hrspace/07_saved_hello_world.PNG" height=200vw>



4. 退出虚拟教学挂件 > 重启 > TP 首页 > `[F2: 系统] - 4: 应用参数 ([F2: system] - 4: Application parameter)` - 检查 `hello, world` 插件   

   <img src="../_assets/hrspace/08_hello_world_menu.PNG" height=500vw>  

   <img src="../_assets/hrspace/09_hello_world_menu_success.PNG" height=500vw>  
<br>

#### 1.6.5 注意事项
1. 如果您修改了 HTML、CSS 或 JavaScript 代码，只需返回 TP 主屏幕并重新进入插件，以使更改生效。
2. <p style="background-color:darkslategrey; color:white;width:max-content">如果您在插件运行时修改 Python 代码，则必须重新启动虚拟控制器以使更改生效。</p>  
   → 右键单击工作区面板中的机器人 > 点击 `VRC Tools...` > 点击 `Reboot`   
   
   <img src="../_assets/hrspace/10_vrc_tools.PNG" height=150vw>
   <img src="../_assets/hrspace/11_vrc_tools_complete.PNG" height=150vw>  

3. 重新启动虚拟控制器后，重新运行 `1.6.4 在虚拟教学挂件上运行插件`。