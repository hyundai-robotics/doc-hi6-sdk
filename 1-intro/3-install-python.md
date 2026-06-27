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