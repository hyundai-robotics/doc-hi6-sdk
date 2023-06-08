# 3.3.7 Operating the F buttons - Initializing to default values



As described in <U>3.3.1 Specifications of the user interface of the ArgosX setup screen </U>,  implement the F buttons used for initializing the settings of the screen to default values.

Add the InitButtonBar( ) function to setup.js as follows.

setup.js
``` js
...Previous steps skipped
 
 
function init()
{
   setDomPath(domPath);
   setUpdateGuideBar(updateGuideBar);
   setUpdateData(updateData);
   onReady();
}
 
 
///@return     f-button infos array
function initButtonBar()
{
   console.log('initButtonBar()'); 
   var btn_infos = [
      {
         label: 'Initialize All',
         script: 'setAllValueAsDef();'
      },
      {
         label: 'Initialize One',
         script: 'setSelectedValueAsDef();'
      }
   ]
   return btn_infos;
}
 
 
..Subsequent steps skipped
```

The InitButtonBar( ) function returns an array of objects that define the interfaces for the F buttons. Each object item consists of an attribute label that designates the button label and an attribute script that designates code in JavaScript to be executed when the button is clicked.

</br>

InitButtonBar( ) function's return values

``` js
[
   {
      label: {label of button-F1},
      script: {script to execute on button-F1 clicked}
   },
   {
      label: {label of button-F2},
      script: {script to execute on button-F2 clicked}
   },
   ....
]
```

The scripts designated in the code above are for calling the setAllValueAsDef( ) function and setSelectedValueAsDef( ) function, respectively. These functions are provided by dst_setup.js by default.


If you want other operations, you need to implment the relevant functions yourself.

</br>
Let's test the operation. Run the virtual controller again and enter the ArgosX setup screen. After that, change the settings to values ​​different from the default values, then save them ​by clicking the [OK] button.

Enter the setup screen again and place the cursor on an arbitrary element. After that, click the Initialize One button and check whether the default values ​​are restored.

In addition, click the Initialize All button and check whether all values of the screen are restored to the default values.

![](../../_assets/image_48.png)