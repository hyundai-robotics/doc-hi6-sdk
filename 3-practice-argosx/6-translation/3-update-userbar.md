#### 3.6.3 用户栏 UI 本地化

##### 1. 用户栏布局的变化

要翻译用户栏 UI，请先打开并查看 ubar.html。

与之前的步骤一样，将 str_table.json 和 lang.js 作为脚本文件添加，如下所示。

由于这些文件相互依赖，必须按照以下顺序包含它们。

此外，由于我们将在 ubar.html 中定义一个名为 init 的初始化函数（之前不存在），请按如下方式编写。

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
<button id='light-on' class='ubar-bt' onclick='light_onoff(true);'>灯<br>开</button>
<button id='light-off' class='ubar-bt' onclick='light_onoff(false);'>灯<br>关</button>
```

您可以删除现有文本，例如 "灯开"。  
（翻译后的文本稍后会动态插入。）

在应用上述所有更改后，html 文件应该如下所示：

ubar.html

```html
<!DOCTYPE html:5>
<!--
    @author: Jane Doe, BlueOcean Robot & Automation, Ltd.
    @brief: ArgosX 视觉系统接口 - 条
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

##### 2. 添加用户栏翻译功能

现在让我们为用户栏屏幕添加翻译功能。

<br>

1) 初始化

在初始化期间，从 str_table.json 加载字符串数据，并将 ${cont_model} 中读取的 lang_code 应用到平台的本地化系统。

ubar.js

```js
function init()
{
    parseStrData();
    setLangCode('/apps/argosx/svr_lang_code', updateAllStrByLang);
}
```

照常，添加 parseStrData 和 setLangCode。

<br>

2) 根据 lang_code 应用翻译

向 str_table.json 添加所需元素。

```json
"en":
{
    "IDS_light_on" : "light on",
    "IDS_light_off" : "light off"
},
"ko":
{
    "IDS_light_on" : "灯打开",
    "IDS_light_off" : "灯关闭"
}
```

接下来，按如下方式修改 ubar.js：

```js
function updateAllStrByLang()
{
    setElemByLang('light-on', 'IDS_light_on');
    setElemByLang('light-off', 'IDS_light_off');
}
```
由于用户栏仅需要更新元素标签，只需使用 setElemByLang 分配字符串 ID。

在重启虚拟控制器和 TP 后，翻译后的用户栏屏幕应该正确显示。

![](../../_assets/image_91.png)