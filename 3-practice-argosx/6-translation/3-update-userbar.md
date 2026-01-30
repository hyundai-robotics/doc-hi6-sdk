#### 3.6.3 User-bar UI Localization

##### 1. Changes in the User-bar Layout

To translate the user-bar UI, first open and review ubar.html.

As in the previous steps, add str_table.json and lang.js as script files as shown below.

Because these files depend on each other, they must be included in the following order.

Also, since we will define an initialization function named init in ubar.html (which did not exist previously), write it as follows.

ubar.html

```html
<script src='./str_table.json' type='application/json'></script>
<script src='../../_common/js/lang.js'></script>
<script src='./ubar.js'></script>
<script>
    $(document).ready(init);
</script>
```

Next, check the buttons declared inside the body:

```html
<button id='light-on' class='ubar-bt' onclick='light_onoff(true);'>light<br>on</button>
<button id='light-off' class='ubar-bt' onclick='light_onoff(false);'>light<br>off</button>
```

You may remove the existing text such as "light on".  
(The translated text will be inserted dynamically later.)

After applying all the changes above, the html file should look like this:

ubar.html

```html
<!DOCTYPE html:5>
<!--
    @author: Jane Doe, BlueOcean Robot & Automation, Ltd.
    @brief: ArgosX Vision System interface - bar
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

##### 2. Add User-bar Translation Behavior

Now let's add translation functionality to the user-bar screen.

<br>

1) Initialization

During initialization, load string data from str_table.json and apply the lang_code read from ${cont_model} to the platform's localization system.

ubar.js

```js
function init()
{
    parseStrData();
    setLangCode('/apps/argosx/svr_lang_code', updateAllStrByLang);
}
```

As before, add parseStrData and setLangCode.

<br>

2) Apply Translation According to lang_code

Add the required elements to str_table.json.

```json
"en":
{
    "IDS_light_on" : "light on",
    "IDS_light_off" : "light off"
},
"ko":
{
    "IDS_light_on" : "Light ON",
    "IDS_light_off" : "Light OFF"
}
```

Next, modify ubar.js as follows:

```js
function updateAllStrByLang()
{
    setElemByLang('light-on', 'IDS_light_on');
    setElemByLang('light-off', 'IDS_light_off');
}
```

Since the user-bar only needs to update element labels, simply assign string IDs using setElemByLang.

After rebooting the virtual controller and TP, the translated user-bar screen should be displayed correctly.

![](../../_assets/image_91.png)
