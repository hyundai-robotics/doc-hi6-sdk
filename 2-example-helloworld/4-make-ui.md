# 2.4 Creating a simple web-based UI

A web-based UI can be added to set up the app using the teach pendant.

In this example, we will create a simple function that will print Hello, world! on the teach pendant screen.



Create a new folder by clicking the New Folder button at the top left. Set the folder name as ui.

![](../_assets/image_19.png)

Create a new file in the ui/ folder and name it setup.html. The result will appear as such in the figure below.

![](../_assets/image_20.png)

Write the code below into setup.html.
```
<!DOCTYPE html:5>
<html>
 
<head>
    <title>hello, world - setup</title>
    <meta http-equiv=Content-Type content='text/html; charset=utf-8'>
</head>
 
<body>
   <div>
      <div id='contents'>
         <h1>Hello, world!</h1>
      </div>
   </div>
</body>
</html>
```

Now, inject the menu item, which is designed to open this screen, under the System - Application Parameter menu of the teach pendant.

Create a menu.json file under the ui/ folder.
![](../_assets/image_21.png)

``` json
[
    {
        "path": "system/appl/",
        "id": "hello_world",
        "icon": "hello_world/ui/lm_hello.png",
        "label": "hello, world",
        "url": "hello_world/ui/setup.html"
    }
]
```

The meaning of each item is as follows.
<table>
  <thead>
    <tr>
      <th style="text-align:left">Key</th>
      <th style="text-align:left">Meaning</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>path</td>
      <td>
       A menu path for injecting a menu item</br>(system/appl/ means "system/4: application parameter.")
      </td>
    </tr>
   <tr>
      <td>id</td>
      <td>
       The ID of a menu item 
      </td>
    </tr>
    <tr>
      <td>icon</td>
      <td>
       The relative path name and file name (based on the apps/ folder) of the item icon to be displayed on the menu screen
      </td>
    </tr>
    <tr>
      <td>label</td>
      <td>
       The item name to be displayed on the menu screen
      </td>
    </tr>
    <tr>
      <td>url</td>
      <td>The relative path name and file name (based on the apps/ folder) of the html screen to be displayed when selecting a menu item</td>
    </tr>
  </tbody>
</table>

Even when an icon is not designated, its operation will be carried out. However, we will create an icon and practice.

The format of the icon should be a png file that contains the transparency information, namely 104x104 pixels.

![](../_assets/lm_hello.png) Example of lm_hello.png (you can download and use this picture.)

</br>
Creating a png file with a transparent background using Paint in Windows is impossible. We recommend the following software.

For your information, we created the picture in the example with COOLTEXT within just one minute.

Adobe Illustrator (https://www.adobe.com/kr/products/illustrator.html): Illustration software (commercial)

GIMP (http://gimp.org): Photoshop-level image editing software (free)

Medibang Paint Pro (https://medibangpaint.com/pc/):
Easy-to-use graphics tool (free)

COOLTEXT (https://cooltext.com/): A website for creating a text-into-logo image file (free)


Execute the virtual main board and virtual teach pendant again.


When entering the [System] - [Application parameter] menu, you can see the newly added hello, world menu item, as shown below.
![](../_assets/image_22.png)

When you press the menu, the teach pendant will show the setup.html screen, as shown below.

(Initially, loading will take about one or two seconds. In subsequent instances, the cache will load faster.)

![](../_assets/image_23.png)
