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