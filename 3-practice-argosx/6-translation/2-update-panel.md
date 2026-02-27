#### 3.6.2 监控面板 UI 本地化

接下来，我们将进行监控面板的翻译工作。

<br>

##### 1. 菜单翻译

面板 UI 也需要对菜单进行翻译。

要在监控面板菜单中显示面板屏幕标签，请添加 id。  
重用之前定义的 "IDS_title"。

menu.json

根据以下内容修改现有的 menu.json：

```json
{
    "path": "panels",
    "id": "argosx",
    "icon": "argosx/ui/panel_argosx.png",
    "label": "IDS_title",
    "url": "argosx/ui/panel.html"
}
```

将标签值设置为 "IDS_title"，而不是固定文本 "ArgosX Vision System"。

如果正确应用，翻译后的标签将出现在监控面板菜单中。

![](../../_assets/image_89.png)

<br>

##### 2. 面板布局的更改

要翻译监控屏幕 UI，首先查看 panel.html。

与设置屏幕一样，添加 str_table.json 和 lang.js 作为脚本文件。

因为这些文件彼此依赖，您必须按照以下顺序包含它们。

```html
<script src='./str_table.json' type='application/json'></script>
<script src='../../_common/js/lang.js'></script>
```

接下来，检查在主体中定义的表格：
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
<br>

##### 3. 添加面板翻译行为

现在，让我们为面板屏幕添加翻译行为。

<br>

1) 初始化

在初始化期间，从 str_table.json 加载字符串数据，并应用来自 ${cont_model} 的 lang_code。

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

与 setup.js 一样，添加 parseStrData 和 setLangCode。

<br>

2) 根据 lang_code 应用翻译

保持现有字符串数据的使用，并向 str_table.json 添加所需的额外元素。

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
接下来，按如下方式修改 panel.js：

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