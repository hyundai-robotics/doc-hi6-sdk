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