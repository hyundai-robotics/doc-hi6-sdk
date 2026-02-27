### 3.3.7 操作 F 按钮 - 初始化为默认值

如在 <U>3.3.1 ArgosX 设置屏幕用户界面的规格</U> 中所述，实现用于将屏幕设置初始化为默认值的 F 按钮。

将 InitButtonBar( ) 函数添加到 setup.js，如下所示。

setup.js
``` js
...前面的步骤略过
 
 
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
         label: '初始化全部',
         script: 'setAllValueAsDef();'
      },
      {
         label: '初始化一个',
         script: 'setSelectedValueAsDef();'
      }
   ]
   return btn_infos;
}
 
 
..后续步骤略过
```

InitButtonBar( ) 函数返回一个对象数组，定义了 F 按钮的接口。每个对象项由一个属性标签组成，用于指定按钮标签，另一个属性脚本用于指定在点击按钮时执行的 JavaScript 代码。

<br>

InitButtonBar( ) 函数的返回值

``` js
[
   {
      label: {按钮-F1 的标签},
      script: {在按钮-F1 被点击时执行的脚本}
   },
   {
      label: {按钮-F2 的标签},
      script: {在按钮-F2 被点击时执行的脚本}
   },
   ....
]
```
上述代码中指定的脚本分别用于调用 setAllValueAsDef( ) 函数和 setSelectedValueAsDef( ) 函数。这些函数默认由 dst_setup.js 提供。

如果您需要其他操作，您需要自己实现相关函数。

<br>
让我们测试操作。再次运行虚拟控制器并进入 ArgosX 设置屏幕。之后，将设置更改为不同于默认值的值，然后通过单击 [OK] 按钮保存。

再次进入设置屏幕，将光标放在任意元素上。然后，单击初始化一个按钮，检查默认值是否恢复。

此外，单击初始化所有按钮，检查屏幕上的所有值是否恢复为默认值。

![](../../_assets/image_48.png)