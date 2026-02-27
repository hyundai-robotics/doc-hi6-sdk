#### 3.1.10 注册机器人语言命令输入

在使用教学挂件时直接输入机器人语言是不方便的。

因此，我们需要确保可以通过[命令输入]更轻松地编写机器人语言。

为此，我们需要注册机器人语言。

首先，我们需要在info.json中添加一个"cmds"标签。对于相应的值，我们需要指定一个包含cmds组织内容的json。

info.json
```json
{
	"author" : "BlueOcean Robot & Automation, Ltd.",
	"binding" : "plug-in",
	"cmds" : "cmds.json",
	"copyright" : "版权所有",
	"description" : "ArgosX视觉系统接口",
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
|项目|含义|示例|
|---|---|---|
|fmts|机器人语言模块名称|"fmts"|
|name|功能名称|"name": "init"|
|samples|示例（机器人语言输入类型）|"samples": "req 39"|
|props|功能的输入属性|"props"|
|guide|输入参数指南消息|"guide": "work no."|
|range|输入参数值的范围|"range": "[1-100]"|

上表中每个项目的含义如下。如果输入参数较多，请将属性分组，数量与输入参数数量相同，然后添加到"props"中。


<br></br>
对于添加的命令，您可以通过按下教导挂件底部的[命令输入]按钮来检查。
![](../../_assets/image_82.png)

[命令输入]-[argosx]
![](../../_assets/image_83.png)