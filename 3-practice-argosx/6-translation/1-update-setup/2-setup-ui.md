#### 3.6.1.2 Translating the Setup Screen UI

##### Changes in the Setup Layout

To translate the UI of the setup screen, first open and review setup.html.

Add str_table.json and lang.js as script files as shown below.

Because these files depend on each other, you must include them in the following order.

```html
<script src='./str_table.json' type='application/json'></script>
<script src='../../_common/js/lang.js'></script>
<script src='../../_common/js/dst_setup.js'></script>
```

Also, check the contents declared inside the body:

```html
<span class='col0' name='ip_addr'>IP address</span>
```

You may remove the text "IP address".  
(The translated text will be inserted later.)

After applying all the changes above, the html file should look like this:

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

##### Adding Translation Behavior to Setup

Now let's add translation functionality to the setup screen.

<br>

1) Initialization

During initialization, add logic to load data from str_table.json and apply the lang_code read from ${cont_model} to the platform's localization system.

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

parseStrData loads the string data.

setLangCode calls a Python function to read the lang_code configured in ${cont_model}, and sets updateAllStrByLang as the callback.

To support setLangCode, add the get_lang_code function to main.py.

It is added to main.py so that ubar and panel can share the same lang_code later.

main.py

```python
def get_lang_code()->dict:
    """ Get language code from remote

    Returns: data: information of language code
    """
    data = {}
    lang_code = xhost.lang_code()
    print(lang_code)
    data["lang_code"] = lang_code
    return data
```

This function returns the lang_code using xhost.lang_code().

<br>

2) Apply Translation According to lang_code

The callback function updateAllStrByLang calls updateElement and updateGuideBarMsg.  
These functions translate elements and guidebar messages based on the lang_code.

First, add the required elements and guidebar messages to str_table.json.

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

Next, modify setup.js as follows.

Remove the existing updateGuideBar function and use updateGuideBarMsg instead.

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

Use setElemByLang and setGuideMsgByLang to assign string IDs to each element and guidebar message.

After rebooting the virtual controller and TP, the translated setup screen should appear correctly.

![](../../../_assets/image_87.png)
