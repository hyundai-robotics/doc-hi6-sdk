# 3.1.3 Creating an ArgosX project

Create an ArgosX folder under the apps/ folder.

Right-click the mouse on the argosx/ folder in the explorer and click "Open using code" in the pop-up menu.

This will open vscode with the argosx/ folder as the project. Create info.json under the argosx/ folder as follows.

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


 First, create a main.py file under the argosx/ folder (you may write your name in @author.)

main.py
```python 
""" ArgosX Vision System interface - main
 
 
@author:    Jane Doe, BlueOcean Robot & Automation, Ltd.
@created:   2021-12-06
"""
```

Then, teach a job file, as shown below, to perform the relevant test.

```
import argosx
end
```
