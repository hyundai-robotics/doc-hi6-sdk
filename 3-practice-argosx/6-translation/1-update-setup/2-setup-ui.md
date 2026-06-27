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