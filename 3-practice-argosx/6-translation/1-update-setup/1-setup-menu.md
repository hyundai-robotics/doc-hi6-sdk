#### 3.6.1.1 Translating the Setup Screen Menu

##### Registering the string table

To register resources for localization, you must create a string table.  
It must be registered in JSON format and added as shown below.

1) Add the file str_table.json inside the ui folder of the argosx project.

    ![](../../../_assets/image_85.png)

2) Content

```json
{
    "en":
    {
        "IDS_title" : "ArgosX Vision System"
    },
    "ko":
    {
        "IDS_title" : "ArgosX Vision System"
    }
}
````

"en" and "ko" are language codes (hereafter referred to as langcode) compatible with ${cont_model}.
"en" means English, and "ko" means Korean.

Each langcode contains members composed of a string id and its corresponding string value.

To add a title in the menu, add string data with the same id "IDS_title" to both "en" and "ko" following the format above.

<br>

##### Translating the menu label

The label on the setup screen menu must also be translated.

Follow the steps below:

1. info.json

Modify the existing info.json file.

Add the created str_table.json file as the value of the "strs" id.

```json
{
    "author" : "BlueOcean Robot & Automation, Ltd.",
    "binding" : "plug-in",
    "cmds" : "cmds.json",
    "copyright" : "All right reserved",
    "description" : "ArgosX Vision System interface",
    "entry" : "main.py",
    "menu" : "ui/menu.json",
    "strs" : "ui/str_table.json",
    "startup" : "boot",
    "version" : "v0.9.0"
}
```

2. menu.json

Modify the existing menu.json file.

```json
{
    "path": "system/appl/",
    "id": "argosx",
    "icon": "argosx/ui/lm_argosx.png",
    "label": "IDS_title",
    "url": "argosx/ui/setup.html"
}
```

Set the label value to "IDS_title" instead of the fixed text "ArgosX Vision System".

<br>

3. Language selection

In the virtual controller environment, you must modify the "lang_code" value in hi6tp_platform_cfg.json.

```json
"lang_code": "en"
```

After changing the lang_code, restart the controller and TP to verify the updated language and menu label.

If applied correctly, the menu should now be displayed in Korean.

![](../../../_assets/image_86.png)
