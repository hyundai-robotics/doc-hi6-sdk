#### 3.6.1.3 Translating the F Button UI

Let's add translation support for the F button UI.

<br>

##### Add String Data

Add string data for the F button labels to str_table.json for each language code.

```json
"en":
{
    "IDS_msg_lb_all" : "Initialize\nAll",
    "IDS_msg_lb_one" : "Initialize\nOne"
},
"ko":
{
    "IDS_msg_lb_all" : "Initialize\nAll",
    "IDS_msg_lb_one" : "Initialize\nOne"
}
```

Each label text is registered with a string ID so it can be displayed according to the selected language.

<br>

##### F Button Behavior

Modify the label values inside btn_infos, which is defined in the existing initButtonBar function, so that they use string IDs instead of fixed text.

setup.js

```js
/// @return f-button infos array
function initButtonBar()
{
    console.log('initButtonBar()');

    var btn_infos = [
        {
            label: "IDS_msg_lb_all",
            script: 'setAllValueAsDef();'
        },
        {
            label: "IDS_msg_lb_one",
            script: 'setSelectedValueAsDef();'
        }
    ];

    return btn_infos;
}
```

Replace the original hard-coded labels with the corresponding string IDs.

After rebooting the virtual controller and TP, the F buttons will be displayed using the translated text based on the selected language.

![](../../../_assets/image_88.png)
