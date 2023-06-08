# Hi6 Software Development Kit (SDK) Manual

{% hint style="warning" %}
The information provided in this product manual is the property of Hyundai Robotics.

It cannot be reproduced or redistributed in part or whole without written consent from Hyundai Robotics, and it cannot be provided to third parties or used for other purposes.



The manual can change without prior notification.



**Copyright ⓒ 2020 by Hyundai Robotics**
{% endhint %}
# 1. Overview of Hi6 SDK

This manual describes how to use the SDK for developing plug-in apps to develop the additional functions of the Hi6 controller.

The functions of apps that can be developed with the SDK are as follows.



You can add new commands or object types to the robot language.
You can add operations that can be performed in various events, such as power on, motor on/off, and start/stop.
You can add operations that can be performed periodically.
You can add user-defined user interfaces (UIs), such as a setup screen for the teach pendant.

# 1.1 Required knowledge

Developing apps using an SDK requires more than basic proficiency in the following technologies.

If you are unfamiliar with the technologies below, studying them first using the appropriate materials is recommended.

<table>
  <thead>
    <tr>
      <th style="text-align:left">Required technology</th>
      <th style="text-align:left">Usage</th>
      <th style="text-align:left">Textbook</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Basic method of using the Hi6 controller</td>
      <td>
       Basic knowledge for operating robots
      </td>
      <td>Hi6 Controller Operation Manual</td>
    </tr>
   <tr>
      <td>HRScript robot language programming</td>
      <td>
       Interlocking between HRScript and apps
      </td>
      <td>Hi6 Controller Function Manual - HRScript </td>
    </tr>
    <tr>
      <td>Python 3 programming</td>
      <td>
       Implementation of app operations
      </td>
      <td>Python tutorials or online/offline training programs</td>
    </tr>
    <tr>
      <td>Web app programming</br>
      (HTML5/CSS/JavaScript, jQuery)</td>
      <td>
       Implementation of app UI
      </td>
      <td>Web development tutorials or online/offline training programs</br>
      (Not required when developing apps without UIs.)</td>
    </tr>
    <tr>
      <td>The basic concept of Ethernet user data protocol (UDP) communication</td>
      <td>
       Understanding the ArgosX examples
      </td>
      <td>Chapters for Ethernet socket communication in Python tutorials</td>
    </tr>

  </tbody>
</table>


		
		
		







		
# 1.2 Concept of Hi6 plug-in apps
An Hi6 app consists of Python 3 scripts that operate in the main module and JavaScript-based web software that execute UI operations in the teach pendant.

For apps without UIs, they may consist of only Python scripts.

Multiple Python files and web app files will be installed under one folder in the main module.

## Python scripts
The scripts can be called through specific robot controller events or periodically. In addition, the designated Python functions can be called using the robot language commands in the .job file.

Through a module called xhost, the robot controller's software objects can be controlled or monitored, and xhost interacts with software objects through a Python interface or OpenAPI.

## Teach pendant UI web apps
This web app is identical to normal web apps used in a PC or mobile environment. It consists of HyperText Markup Language (HTML)/Cascading Style Sheets (CSS)/JavaScript and resource files, such as various images.

This web app is stored in the main module, which will be transferred to the teach pendant to be executed on a web browser engine.

By calling the OpenAPI, the teach pendant web app can call the Python functions and send and receive data.

![](../_assets/image_1.png)

# 1.3 Installing an Hi6 virtual controller 

(temporary)</br>
1) Unzip the downloaded Hi6 controller zip file.
2) Execute according to the 'How to Use.txt' in the unziped folder to adjust the development environment.
3) Execute hi6_main.exe in the Debug folder.
4) At the same location
Input the command argument -layout=k.

# 1.4 Installing the Python 3 development environment
## Installing Python 3 
Install Python v3.8 according to the following procedures.



1) The link below will lead to a Python v3.8.0. installation screen. Install x86 32bit.

    (caution: As the Hi6 virtual controller is a 32bit app, the python runtime should be made to match it. Do not install x86-64) </br>
    https://www.python.org/downloads/release/python-380/

    ![](../_assets/image_2.png)

2) Tick Add Python 3.8 to PATH, then select Customize installation.
    ![](../_assets/image_3.png)

3) Tick all and click Next.
    ![](../_assets/image_4.png)
4) Tick all. Leave the installation path as C:\Program Files (x86)\Python38-32 as is and click Install.
    ![](../_assets/image_5.png)
5) You do not need to press Disable path length limit. Click Close.
    ![](../_assets/image_6.png)

6) Open the Windows Command Prompt (press Windows + R, type in cmd, then press the enter key.)

Type in python --version, then press the Enter key to check whether the version shown below is printed.

Python 3.8.0

## Adding a Python import search path
1) Create a .pth file and designate the path where the _common/ folder is located. </br>Open the .pth file in the SDK and designate the path below to match the HOME path of the Hi6 virtual controller.
 

Example of file contents:    
    D:\Hi6\home_main\apps

Deploy the edited .pth file into the python installation path/Lib/site-packages/.

Example: C:\Program Files (x86)\Python38-32\Lib\site-packages\.pth


## Deploying dynamic libraries
Deploy ucrtbased.dll and vcruntime140d.dll in the SDK to the Python installation path.

Example: Copy to C:\Program Files (x86)\Python38-32\.

# 1.5 Installing Visual Studio Code

Microsoft Visual Studio Code (hereafter referred to as vscode) is a powerful text editor that is available for free. Through the installation of various extensions, vsode provides a development environment for numerous programming languages.

You may use other familiar editors, such as Atom or SublimeText, but this manual provides explanations based on vscode.



Installing the code
1) Access the link below then download the stable build version for Windows.</br>For Windows: https://code.visualstudio.com/</br>
![](../_assets/image_7.png)

2) Execute the downloaded installation file and accept the license agreement.
![](../_assets/image_8.png)

3) While keeping the default values, such as the installation path, continue pressing the Next button. When the screen for selecting additional jobs appears, tick as shown below, then click the Next button.
![](../_assets/image_9.png)

4) Click the Install button. 

5) Upon finishing the installation, try running vscode.

# Installing extensions
This is the advantage vscode possesses that makes installing various extensions available in the marketplace possible.

The Activity Bar is a vertical bar on the far left of the vscode screen where icons are arranged. Among the icons, pressing the one marked in red will open the EXTENSIONS: MARKETPLACE screen, as shown in the figure. When you type the name of the desired extension in the filter window at the top, you can easily find the extension.

![](../_assets/image_10.png)

The extensions you need to install are as follows. Find each of them by using the filter, and install them using the Install button. 



- There may be multiple extensions with the same name. Select the correct extension by checking the author's name.
- When Microsoft's Python is installed, additional extensions, such as Pylance, will be installed automatically. Pylance conflicts with Pyright, which is what we should use. Remove all other extensions except for Python.
- When you edit a job file, it should be recognized as HRScript, not HR-BASIC. Leave HR-BASIC disabled.

![](../_assets/image_11.png)

During the development process, you will handle files with html, css, javascript, and json extensions, in addition to python. However, as the editing functions for these formats are embedded in vscode, you don't need to install the extensions separately.



Now, you are ready to use vscode. Learning about how to use it is recommended by referring to the vscode help menu or online lectures.

![](../_assets/image_12.png)
# 1.6 Installing a web-based UI development environment
The customized UI for the teach pendant should be developed as a web app form of HTML5/CSS/jQuery.



## Installing the Google Chrome web browser

The Google Chrome web browser provides an environment for running and debugging web apps. If you have not installed the browser yet, install it by clicking the link below.

(Microsoft Edge or Mozilla Firefox also provides almost the same functions. However, this manual will provide explanations based on Google Chrome.)

https://www.google.com/intl/ko/chrome/

# 1.7 Installing an SDK in a virtual controller environment

The apps/ folder of the SDK should be copied under the Hi6 home_main folder of the virtual controller.

The _common/ folder under the apps/ folder contains the libraries all apps commonly use.

Apps that will be developed in the future need to be deployed as subfolders of the apps/ folder as follows.

# 2. Very simple project: hello_world
Skip to the end of the metadata
Created by Wonhyeok Choi, last modified on December 24, 2021
Go to the start of the metadata
Let's start by creating an app as a very simple project.

What this app, named hello_world, does is print a Hello, world! string onto the history screen and setup screen.



Creating a hello_world project – Folders and meta-information
Implementing the Python function hello( )
Transferring parameters to a Python function and receiving a return value
Creating a simple web-based UI
# 2.1 Creating a hello_world project – Folders and meta-information

Create a folder named hello_world under the apps/ folder. The folder name should be the project name and must be unique in the apps/ folder.

Right-click the mouse on the hello_world/ folder in the explorer and click "Open using code" in the pop-up menu.

![](../_assets/image_13.png)

Afterward, vscode will open with the hello_world/ folder as the project. You can create a new file in the folder by clicking New File at the top left. The file name should be info.json.

![](../_assets/image_14.png)

Click info.json to open it and input the following.

![](../_assets/image_15.png)

## info.json

``` json
{
   "author" : "Hyundai Robotics",
   "binding" : "plug-in",
   "copyright" : "All right reserved",
   "description" : "First example - hello_world",
   "entry" : "hello_world.py",
   "menu" : "ui/menu.json",
   "startup" : "manual",
   "version" : "v0.9.0"
}
```

</br>
<table>
  <thead>
    <tr>
      <th style="text-align:left">Key</th>
      <th style="text-align:left">Meaning</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>author</td>
      <td>
       Author
      </td>
    </tr>
   <tr>
      <td>binding</td>
      <td>
       The form of binding with the Hi6 host software</br>
       - Plug-in: Will be executed in bound form.</br>
       - Standalone: Will be executed as an independent app (process). 
      </td>
    </tr>
    <tr>
      <td>copyright</td>
      <td>
       Copyright
      </td>
    </tr>
    <tr>
      <td>description</td>
      <td>
       Description	
      </td>
    </tr>
    <tr>
      <td>entry</td>
      <td>
       The name of the file at the execution start location</br>
       The name should be unique in the apps/ folder. If possible, set it as one of the following names below</br>
       - {project name}.py</br>
       - {project name}_main.py
      </td>
    </tr>
    <tr>
      <td>menu</td>
      <td>
       Menu structure of the user interface	
      </td>
    </tr>
     <tr>
      <td>startup</td>
      <td>
       Execution start mode
       - Manual: Manually start the execution
       - Boot: Automatically start the execution while booting
      </td>
    </tr>
     <tr>
      <td>version</td>
      <td>
       Version string	
      </td>
    </tr>
  </tbody>
</table>

# 2.2 Implementing the Python function hello( )

Create a new file using the New File button and name it hello_world.py.
![](../_assets/image_16.png)

You can write hello_world.py as follows.

```python
import xhost
 
def hello():
    print("Hello, world!")
    xhost.printh("Hello, world!")
```

- As you might know if you are familiar with python programming, a line below def should be indented with the tab character.
- xhost is a module that calls the functions of the host (robot controller). You do not have to write an xhost.py file yourself. The subsequent sections will provide detailed explanations, so this is all you need to understand for now.

Now, execute the Hi6 virtual controller, which is the main module, and the TP in order.

When starting, the Hi6 controller recognizes the installed apps by reading the info.json from all folders under the apps/ folder.

[service] - 10: Clicking the app will bring up a screen called 10: app - TP. Clicking the [location] button twice will change the TP in the title to MAIN via USB. On this screen, hello_world created earlier can be found.

![](../_assets/image_17.png)

We will execute hello_world in the robot language, so press the ESC key to exit the screen.

Now, create a job program using HRScript. Carry out the teaching process as follows.

```
import hello_world
hello_world.hello()
end
```

While leaving the previous screen pane open and turning on the motor, if you perform an execution with the Step FWD or START buttons, Hello, World! will be printed.

```
15:05:20.894 ( 968) .import hello_world

15:05:20.894 ( 0)_____[STOP for CmdMode]

15:05:25.413 (4518890)_____[START:StepFwd]__(P2/S0./F1)___

15:05:25.415 Hello, world!

15:05:25.416 ( 2968) .var ret=hello_world.hello()

15:05:25.417 ( 1024)_____[STOP for CmdMode]

15:05:32.157 (6740100)_____[START:StepFwd]__(P2/S0./F2)___

15:05:32.158 ( 970) .end
```

<span style='background-color:#ffdce0'>
Caution: If you modify the Python program, you should run the virtual controller to reflect the modification.
</span>

# 2.3 Transferring parameters to a Python function and receiving a return value


Now, let's apply two parameters, name and age, to a Python function. Add the introduce( ) function by modifying the code as follows.

```python
import xhost
 
def hello():
    print("Hello, world!")
    xhost.printh("Hello, world!")
 
 
def introduce(name: str, age: int) -> str:
    msg = f"Hello, {name}! You are {age} years old."
    return msg
```

- :str, :int, and -> str in the introduce functions are elements of grammar called type hint, introduced with Python 3.5. Omitting type hint will not affect the operation. Even though type hint does not play any role while executing scripts, it provides information for vscode extensions, such as Pyright, to check its grammar. Therefore, type hint helps prevent grammar errors when coding in Python (https://docs.python.org/3.8/library/typing.html.)


In a job program, you need to teach it additionally to call introduce( ).

```
import hello_world
hello_world.hello()
var msg=hello_world.introduce("Steve", 25)
print msg
end
```

If the following string is printed on the guidance frame of the teach pendant, it means the operation is normal.
![](../_assets/image_18.png)

As seen in the example, the string values and integer values of HRScript are naturally transferred as the string values and integer values of Python. Conversely, the string return values of Python are also naturally returned as the string values of HRScript. The values of both languages are converted automatically and mutually like this. The table below is a data type map of the two languages.

<table>
  <thead>
    <tr>
      <th style="text-align:left">HRScript</th>
      <th style="text-align:left">Python</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>bool</td>
      <td>
       bool
      </td>
    </tr>
   <tr>
      <td>number - integer</td>
      <td>
       long 
      </td>
    </tr>
    <tr>
      <td>number - real</td>
      <td>
       float
      </td>
    </tr>
    <tr>
      <td>string</td>
      <td>
       str
      </td>
    </tr>
    <tr>
      <td>array</td>
      <td>tuple</td>
    </tr>
    <tr>
      <td>object</td>
      <td>
       dictionary	
      </td>
    </tr>
     <tr>
      <td>xpy-object</td>
      <td>object</td>
    </tr>
  </tbody>
</table>

-  When an object created in Python is transferred to HRScript, it is specifically called as xpy-object. When it comes to xpy-objects, it is possible to read the attribute values and call methods. We will cover them in more detail later.
- For Python grammar, multiple return values can be transferred from a function to the outside, but they cannot be transferred to HRScript.
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

Medibang Paint Pro (https://medibangpaint.com/pc/): Easy-to-use graphics tool (free)

COOLTEXT (https://cooltext.com/): Easy-to-use graphics tool (free)



Execute the virtual main board and virtual teach pendant again.


When entering the [System] - [Application parameter] menu, you can see the newly added hello, world menu item, as shown below.
![](../_assets/image_22.png)

When you press the menu, the teach pendant will show the setup.html screen, as shown below.

(Initially, loading will take about one or two seconds. In subsequent instances, the cache will load faster.)

![](../_assets/image_23.png)
# 3. Practical project: ArgosX
# 3.1 Practical project: ArgosX - Robot language


Now let's practice with a more realistic example project.

We will learn how to develop an app by developing plug-ins for a virtual vision system named ArgosX to be installed in a robot.


- Specifications of ArgosX and interface plug-ins</br>
- ArgosX stub</br>
- Creating an ArgosX project</br>
- Creating ip_addr and port attributes</br>
- Creating functions for the ArgosX robot language</br>
- Implementing functions for the ArgosX robot language</br>
- Calling the xhost module methods</br>
- Manual for referring to the methods of the xhost module</br>
- Solving the robot language function-blocking problem</br>
# 3.1.1 Specifications of ArgosX and interface plug-ins

## Specifications of the ArgosX vision system


### Basic specifications
<table>
  <thead>
    <tr>
      <th style="text-align:left">Item</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Function</td>
      <td>
       - It contains an embedded LED light, which can be turned on and off via a communication request.</br>
       - It can simultaneously measure the shift values of up to 100 workpieces and report in response to a communication request.
      </td>
    </tr>
   <tr>
      <td>Communication interface</td>
      <td>
       - The robot controller and ArgosX hardware communicate with each other through Ethernet UDP communications.</br>
        - The IP address of the ArgosX hardware is 192.168.1.XX. As the last set of digits, XX, should be set using the dip switch, the robot side should send a UDP request accordingly.</br>
        - The port number on the ArgosX hardware is fixed as 54321. However, it may change in future products.</br>
        - Upon receiving a UDP request, the ArgosX hardware will send a response to the sender’s IP address.
      </td>
    </tr>
    <tr>
      <td>Number of the systems that can be installed</td>
      <td>
       - Only one ArgosX system can be installed in the robot controller. In other words, the ArgosX system is a single instance in terms of software.
      </td>
    </tr>
  </tbody>
</table>

### Protocol
<table>
  <thead>
    <tr>
      <th style="text-align:left">Direction of transmission</th>
      <th style="text-align:left">Port#</th>
      <th style="text-align:left">Grammar and example</th>
      <th style="text-align:left">Meaning</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>robot → ArgosX</td>
      <td>54321</td>
      <td>req {workpiece#}</br>
        e.g. "req 39"</td>
      <td>Request for the shift value of the workpiece #</br>The workpiece number (#) ranges from 1 to 100 </td>
    </tr>
    </tr>
   <tr>
      <td>robot ← ArgosX</td>
      <td></td>
      <td>res ({x}, {y}, {z}, {rx}, {ry}, {rz})</br>
            The string "fail" will be transferred if the measurement fails.</br>
            e.g. "res (30, 25.7, 11.9, 31.6, 12.8, -54.6)"</br>
            e.g. "fail"</td>
      <td>Response regarding the shift value of the workpiece #</br>
        The values of x-rz are real numbers, and their units are mm and deg</td>
    </tr>
    <tr>
      <td>robot → ArgosX</td>
      <td>54321</td>
      <td>light-on</td>
      <td>Turns the LED light on.</td>
    </tr>
    <tr>
      <td>robot → ArgosX</td>
      <td>54321</td>
      <td>light-off</td>
      <td>Turns the LED light off.</td>
    </tr>
  </tbody>
</table>

## Specifications of the interface plug-ins for ArgosX


The interface plug-ins for ArgosX will be developed with the following specifications.



### Robot language
<table>
<table>
  <thead>
    <tr>
      <th style="text-align:left">Item</th>
      <th style="text-align:left">Grammar</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>module</td>
      <td>argosx</td>
      <td></td>
    </tr>
   <tr>
      <td rowspan="2">attribute</td>
      <td>ip_addr</td>
      <td>The IP address string of the ArgosX hardware (it can be set.)</br>e.g. "192.168.1.44"</td>
    </tr>
    <tr>
      <td>port</td>
      <td>The port number of the ArgosX hardware.</br>(setting it should be possible, as there may be changes in future products.)</br>e.g. 54321</td>
    </tr>
    <tr>
      <td rowspan="4">function</td>
      <td>init( )</td>
      <td>Initialize the socket for UDP communication.</td>
    </tr>
    <tr>
      <td>req({workpiece#})</td>
      <td>Request the result shift value of the workpiece #</td>
    </tr>
    <tr>
      <td>res( )</td>
      <td>Receive a request while waiting for a response.</br>The return value is the shift array string based on the base coordinate system.</br>e.g. "[30, 25.7, 11.9, 31.6, 12.8, -54.6, \"base\"]"</td>
    </tr>
    <tr>
      <td>close( )</td>
      <td>Close the socket for UDP communication.</td>
    </tr>
  </tbody>
</table>

### Lighting function
- When the robot is placed in the motor On state, the ArgosX LED light will also be switched on.
- When the robot is placed in the motor Off state, the ArgosX LED light will also be switched off.

### Error handling
- When "fail" is received from ArgosX, the universal I/O output signal of the robot controller corresponding to the preset number will be switched on.


### Monitoring
By opening the ArgosX monitoring panel on the teaching pendant, you can see the following information.

- IP address
- Port #
- Error input assigned number
- Request count
- Response count


### User bar
When you open the ArgosX user bar on the teach pendant, a UI, as shown below, will be provided.

- Light-on button: Turns the ArgosX LED light on.
- Light-off button: Turns the ArgosX LED light off.

# 3.1.2 ArgosX stub

The ArgosX vision system is not real. Therefore, if we want to test the interface plug-ins, we need the test software, namely the stub, to take ArgosX's place.

The Python code below is an ArgosX stub. You do not need to understand the details of its implementation.

argosx_stub.py
``` python
"""ArgosX stub
Test double for ArgosX interface plug-in
 
 
@author:    Jane Doe, BlueOcean Robot & Automation, Ltd.
@created:   2021-12-06
@version:   v1.0
"""
 
import time
from typing import Optional, Union, Dict
import socket
 
 
# const
buf_size = 0x8000    # 32kb ; permitted packet length
port_no = 54321      # port for ArgosX command
sleep_sec = 0        # delay before response
 
# global variables
inaddr_any : str = ""
ip_port_of_req = ("", 0)
sock : Optional[socket.socket] = None
 
# test samples
test_shifts : Dict[str, str]= {
   "5" : "(30, 25.7, 11.9, 31.6, 12.8, -54.6)",
   "39" : "(9, 15.5, 10.3, 11.2, 19.2, 1.3)",
   "98" : "fail",
   "else" : "(0, 0, 0, 0, 0, 0)"
}
 
 
# functions
def init():
   """
   init server
   """
   global sock
   try:
      sock = socket.socket(family=socket.AF_INET, type=socket.SOCK_DGRAM)
      sock.bind((inaddr_any, port_no))
   except socket.error as e:
      print("socket creation or binding error :", e)
      return -1
   return 0
 
 
def close():
   """
   close server
   """
   if sock is None: return
   sock.close()
 
 
def do_service():
   """
   do service loop
   """
   state = 0
    
   while(True):
      msg = recv_msg()
      msg = msg.strip('\x00')
      ret = do_service_sub(msg)
      if ret==1:
         break
 
 
def recv_msg() -> str:
   """
   receive UDP message (blocking)
   IP address and port of sender is stored in ip_port_of_req
   Returns:
         received message     e.g. "req 39"
   """
   global ip_port_of_req
   if sock is None: return ""
   data, ip_port_of_req = sock.recvfrom(buf_size)
   bts = bytearray(data)
   msg = bts.decode()
   return msg
 
 
def do_service_sub(msg: str) -> int:
   """
   do service subroutine
   Args:
         msg   e.g. "req 39"
   Returns:
         1     quit the service
         0     continue the service
         -1    invalid command  
   """
   print('')
   print('request : ', msg)
   strs = msg.split()
       
   n_str = len(strs)
   if n_str < 1:
      return -1
    
   cmd = strs[0]
   param = ""
   if n_str >= 2:
      param = strs[1]
    
   if cmd=="req":
      time.sleep(sleep_sec)
      do_service_req(param)
   elif cmd=="light-on":
      print('LED light is ON')
   elif cmd=="light-off":
      print('LED light is OFF')
   elif cmd=="quit":
      return 1
   else:
      print('invalid command')
      return -1
   return 0
 
 
def do_service_req(param: str) -> int:
   """
   do service for req
   Args:
      param    work#    "1"~"100"
 
   Returns:
         -1    no socket
         >=0   the number of bytes sent
   """
   if sock is None: return -1
   res_value = ""
   try:
      res_value = test_shifts[param]
   except:
      res_value = test_shifts["else"]
   msg = "res " + res_value
   print('response: ', msg)
   bts = bytearray(str.encode(msg))
   return sock.sendto(bts, ip_port_of_req)
    
 
# -----------------------------------------------
# main
print('***** ArgosX stub v1.0 *****')
iret = init()
if iret < 0:
   quit()
print('inaddr_any, port_no=%d' % port_no)
print('server started...')
do_service()
print('closing...')
close()
print('...server ended')
```

Copy the content above and create an argosx_stub.py file under the hello_world/ folder. Next, go to the hello_world/ folder using Windows PowerShell or Command Prompt, then execute it using the command below.
```
python argosx_stub.py
```

Alternatively, if you open vscode and press F5, the execution will be performed in debug mode.

- Rather than opening the argosx/ project in the opened vscode, you need to open the project by executing another vscode session.
- While the debug configuration list may open initially, as shown below, you only need to select the Python File item.

![](../../_assets/image_24.png)

The result will be printed on the TERMINAL window at the bottom. You can hold, resume, or stop debugging by operating the ![](../../_assets/image_25.png) button on the top right.

![](../../_assets/image_26.png)

# 3.1.3 Creating an ArgosX project

Create an ArgosX folder under the apps/ folder.

Right-click the mouse on the argosx/ folder in the explorer and click "Open using code" in the pop-up menu.

This will open vscode with the argosx/ folder as the project. Create info.json under the argosx/ folder as follows.

info.json

```json
{
   "author" : "BlueOcean Robot & Automation, Ltd.",
   "binding" : "plug-in",
   "copyright" : "All right reserved",
   "description" : "ArgosX vision system interface",
   "entry" : "main.py",
   "menu" : "ui/menu.json",
   "startup" : "manual",
   "version" : "v0.9.0"
}
```


First, create a main.py file under the argosx/ folder (you may write your name in @author.)

main.py
```python 
""" ArgosX Vision System interface - main
 
 
@author:    Jane Doe, BlueOcean Robot & Automation, Ltd.
@created:   2021-12-06
"""
```

Then, teach a job file, as shown below, to perform the relevant test.

```
import argosx
end
```
# 3.1.4 Creating ip_addr and port attributes

When looking at <u>3.1.1 Specification of ArgosX and interface plug-ins</u>, you can find a string attribute to designate the ip_addr.



Add the global variables and attr_names( ) function to the argosx_main.py file, as shown below.

We can define and use many global variables in the Python program. However, only the names listed in the tuple returned by attr_names( ) will be exposed in HRScript as ArgosX module attributes. For variables exposed as attributes, the getter and setter functions with get_ and set_ added to their names should be defined.

For this example, let’s define two global variables, ip_attr and port, then expose them as attributes and permit reading and writing them in HRScript.



Create a new setup.py file in the argosx/ project folder, then implement it as follows.




setup.py
``` python 
""" robot application - argosx - setup
 
 
@author:    Jane Doe, BlueOcean Robot & Automation, Ltd.
@created:   2021-12-06
"""
 
 
 
# attributes getter/setter
def get_ip_addr() -> str:
   return ip_addr
 
def set_ip_addr(addr: str):
   global ip_addr
   ip_addr = addr
 
def get_port() -> int:
   return port
 
def set_port(_port: int):
   global port
   port = _port
 
ip_addr : str = "192.168.1.100"
port : int = 54321
```

Import the entire setup module into main.py so that the getter and setter functions can be called from HRScript, then define the attr_name( ) function that returns the tuples of the attribute names.

(The host handles the argosx/ folder as a package. When importing the setup module from the main module, you should explicitly add a . (period) in front of setup, which means the same folder.)



main.py
```python 
""" ArgosX Vision System interface - main
 
 
@author:    Jane Doe, BlueOcean Robot & Automation, Ltd.
@created:   2021-12-06
"""
 
from .setup import *
 
 
def attr_names() -> tuple:
   """Returns the names of the attributes to be exposed."""
   return ("ip_addr", "port")
```

The test job program should be taught and executed as follows.
```
import argosx
print argosx.ip_addr
print argosx.port
end
```

If the values of the Python global variables are displayed in order in the guidance frame, as shown below, it means the operation is normal.

```
192.168.1.100

54321
```


We will test communications between ArgosX and the argosx_stub within the same PC. Let's change the value of ip_addr to the IP address of your PC and check whether it has changed.

Change and execute the job program as follows.

```
import argosx
print argosx.ip_addr
print argosx.port
argosx.ip_addr="192.168.1.172" # your own PC's IP address
print argosx.ip_addr # Rechecking
end
```

Check whether the value newly assigned to ip_addr is printed on the guidance frame.
```
192.168.1.172
```
# 3.1.5 Creating functions for the ArgosX robot language


The specifications to be implemented next are the init( ), req( ), res( ), and close( ) functions.

(Refer to the functions of the Protocol and Robot Language of <u>3.1.1 Specifications of ArgosX and interface plug-ins</u>.)



For now, let's perform a print operation for each function.

Create a roblang.py file under the argosx/ folder as follows.



roblang.py (for testing)
``` python 
""" ArgosX Vision System interface - robot language
 
 
@author:    Jane Doe, BlueOcean Robot & Automation, Ltd.
@created:   2021-12-06
"""
 
 
# functions
def init() -> int:
   print("init()")
   return 0
 
 
def close():
   print("close()")
 
 
def req(work_no: int) -> int:
   msg = "req(" + str(work_no) + ")"
   print(msg)
   return 0
 
 
def res():
   print("res()")
   return "data"
```

Import all names from roblang.py into the main.py.



main.py
```python

""" ArgosX Vision System interface - main
 
 
@author:    Jane Doe, BlueOcean Robot & Automation, Ltd.
@created:   2021-12-06
"""
 
 
from .roblang import *
from .setup import *
 
 
def attr_names() -> tuple:
   """Returns the names of the attributes to be exposed."""
   return ("ip_addr", "port")

```


job file
```
var iret
import argosx
 
print argosx.ip_addr
print argosx.port
argosx.ip_addr="192.168.1.172" # your own PC's name
print argosx.ip_addr # rechecking
 
iret=argosx.init() # initializing the socket
if iret<0
  print "init error"
  stop
endif
 
iret=argosx.req(39) # transmitting the request
if iret<0
  print "req error"
  stop
endif
 
var str=argosx.res() # waiting for a response
print str
 
argosx.close() # closing the socket
end
```

Reboot the virtual controller and execute the job file. If the file was created normally, the following result will be printed on the virtual controller console. This is a Python function called from HRScript.
```
192.168.1.172

init()

req(39)

res()

data

close()
```
# 3.1.6 Implementing functions for the ArgosX robot language

Now let's implement the actual operations of each function.

The operation for UDP client communications may be used in other sections later. Here, we will modularize the operation into a separate .py file.

Add a comm.py file to the project and write the following operations, as shown below.

They are simple operations, such as initializing a UDP socket, sending, receiving, and closing a string message.

- While xhost is a module that calls the functions of the host (robot controller), the main software makes it dynamic (there is no file called xhost.py). Subsequent sections will provide detailed explanations, so this is all you need to understand for now.


comm.py
``` python 
""" ArgosX Vision System interface - comm.
 
 
@author:    Jane Doe, BlueOcean Robot & Automation, Ltd.
@created:   2021-12-10
"""
 
from typing import Optional
import socket
import xhost
 
 
# global variables
raddr : tuple
sock : Optional[socket.socket] = None
buf_size = 0x8000    # 32kb ; permitted packet length
 
 
def is_open() -> bool:
   """
   Returns:
         True     socket is open
         False    socket is not open
   """
   return (sock is not None)
 
 
def open(ip_addr: str, port: int) -> int:
   """
   open socket for UDP communication
   Args:
      ip_addr     ip adddress of remote. e.g. "192.168.1.172"
      port        port# of remote. e.g. "192.168.1.172"
 
   Returns:
         0     ok
         -1    error
   """
   global raddr, sock
   if sock is not None: return -1
   try:
      raddr = (ip_addr, port)
      sock = socket.socket(family=socket.AF_INET, type=socket.SOCK_DGRAM)
   except socket.error as e:
      print("socket creation or binding error :", e)
      return -1
   logd('comm.open: ' + str(raddr))
   return 0
 
 
def close() -> None:
   global sock
   if sock is None: return
   logd('comm.close')
   sock.close()
   sock = None
 
 
def send_msg(msg: str) -> int:
   """
   send msg to sock, raddr
   Args:
      msg
 
   Returns:
         >=0   the number of bytes sent
         -1    no socket. init() should be called.
   """
   if sock is None: return -1
 
   logd('request : ' + msg)
   bts = bytearray(str.encode(msg))
   return sock.sendto(bts, raddr)
 
 
def recv_msg():
   """
   wait msg from sock
   Returns:
      received string
   """
   if sock is None: return ""
 
   try:
      data, ip_port = sock.recvfrom(buf_size)
      bts = bytearray(data)
      msg = bts.decode()
      logd('response: ' + msg)
      return msg
   except Exception as e:
      print('exception from recv_msg(): ' + str(e))
      return ""
 
 
def logd(text: str):
   print(text)
   xhost.printh(text)
```

By importing the comm module, you can simply implement individual functions that are to be called in the robot language.

You also need to import the setup module because referring to the ip_addr and port values is necessary.

get_base_shift_array_from_res() is a function that converts the shift string received from ArgosX into a format that will be interpreted as the shift( ) function of HRScript.


roblang.py
``` python 
""" ArgosX Vision System interface - robot language
 
 
@author:    Jane Doe, BlueOcean Robot & Automation, Ltd.
@created:   2021-12-06
"""
 
from . import comm
from . import setup
 
 
# functions
def init() -> int:
   """
   init socket for UDP communication
 
   Returns:
         0     ok
         -1    error
   """
   return comm.open(setup.ip_addr, setup.port)
 
 
def close():
   """
   close socket
   """
   comm.close()
 
 
def req(work_no: int) -> int:
   """
   send request command to ArgosX
   e.g. "req 39"
   Args:
      work_no     work#    1~100
 
   Returns:
         >=0   the number of bytes sent
         -1    no socket. init() should be called.
   """
   msg = "req " + str(work_no)
   return comm.send_msg(msg)
 
 
def res() -> str:
   """
   wait response from ArgosX
   Returns:
      response string from ArgosX.
      "" if failed.
      e.g. "[30, 25.7, 11.9, 31.6, 12.8, -54.6]"
   """
   msg = comm.recv_msg()
   print(msg)
   msg = get_base_shift_array_from_res(msg)
   return msg
 
 
def get_base_shift_array_from_res(msg: str):
   """
   get base shift array notation string from response string
   Args:
      msg   e.g. "res (30, 25.7, 11.9, 31.6, 12.8, -54.6)"
    
   Returns:
      e.g. '[30, 25.7, 11.9, 31.6, 12.8, -54.6, "base"]'
   """
   tmp = msg.strip('res ')
   tmp = tmp.replace('(', '[')
   tmp = tmp.replace(')', ', "base"]')
   return tmp
```

The job should be corrected as follows.


```
Hyundai Robot Job File; { version: 1.6, mech_type: "780(YL012-0D)", total_axis: 6, aux_axis: 0 }
     var iret
     import argosx
      
     print argosx.ip_addr
     print argosx.port
     argosx.ip_addr="192.168.1.172" # your own PC's name
     print argosx.ip_addr # rechecking
      
     iret=argosx.init() # initializing the socket
     if iret<0
       print "init error"
       stop
     endif
      
     iret=argosx.req(39) # transmitting the request
     if iret<0
       print "req error"
       stop
     endif
      
     var str=argosx.res() # waiting for a response
     print str
     var sft=Shift(str) # converting the shift array string into shift data
     print sft.x, sft.y, sft.z, sft.rx, sft.ry, sft.rz
 
     argosx.close() # closing the socket
     end
```

First, execute the argosx_stub from the Command Prompt or vscode.

Reboot the virtual controller and execute the job file. If the file was created normally, the following operation will occur.



<U>___argosx_stub side (a server playing the role of ArgosX)__ </U>

Every time argosx.req( ) is executed, the following string will be printed on the console.
```
request : req 39
response: res (9, 15.5, 10.3, 11.2, 19.2, 1.3)
```

<U>__argosx interface plug-in side (client)__</U>

Every time the last print command is executed, the guidance frame of the teach pendant will print the following.
```
9.000000 15.500000 10.300000 11.200000, 19.200000 1.300000
```

# 3.1.7 Calling the xhost module methods

xhost is a module containing various methods to call the functions of the host (robot controller).

The virtual controller, which is the main module, will create xhost and inject it into the Python runtime. You can use xhost by importing it, and there is no need to write a xhost.py file for yourself.



Refer to <U>3.1.8 Manual for referring to the methods of the xhost module</U>.



There is also an item regarding error handling in <U>3.1.1 Specifications of ArgosX and interface plug-ins</U>.

When "fail" is received from ArgosX, the universal I/O output signal of the robot controller corresponding to the preset number will be switched on.


The universal I/O output signals of the robot controller can be switched on/off using the method below.
``` python 
def io_set_out_bit(sigcode: int, val: int) -> int
```

sigcode is a code that combines the block number and index of the I/O into one number, as shown below.

sigcode = block number x 10000 + index



For example, the sigcode of fb3.do72 is as follows.

3 x 10000 + 72 = 30072



1 if val is on and 0 if it is off.
<br></br>



Add the output signal assigned number for the ArgosX error as a module variable named sigcode_err, and set its default value to 5 (i.e. fb0.do5.)

(We can also declare it as an attribute to make a change possible in HRScript. However, it will be skipped in this example.)



setup.py
```python 
..previous steps skipped
ip_addr : str = "192.168.1.100"
port : int = 54321
sigcode_err = 5
```

The msg value received in the res( ) function will be compared with "res fail", and an output signal will be transmitted according to the result.



roblang.py
```python
.. previous steps skipped  
  
  
import xhost
  
  
...Skipped
 
 
def res() -> str:
   """
   wait response from ArgosX
   Returns:
      response string from ArgosX
      "" if failed.
      e.g. "[30, 25.7, 11.9, 31.6, 12.8, -54.6]"
   """
   val = 0
   msg = comm.recv_msg()
   print(msg)
   if msg=="res fail":
      val = 1
      msg = ""
   else:
      msg = get_base_shift_array_from_res(msg)
   xhost.io_set_out_bit(setup.sigcode_err, val)
   return msg
```

Execute the virtual controller, and, while leaving the universal output panel of the teach pendant open, execute the job program.

Because there is no failure, the operation will be the same as before, and the fb0.do5 print signal will not be switched on.

argosx_stub.py is designed to unconditionally respond with failures when work #98 is requested. Modify the job so that req(98) can be performed, as shown below, then perform the implementation again.



job
```
...Previous steps skipped
 
 
     iret=argosx.req(98) # transmitting the request
     if iret<0
       print "req error"
       stop
     endif
      
     var str=argosx.res() # waiting for a response
     print str
     if str==""
       print "req error"
       stop
     else
       var sft=Shift(str) # converting the shift array string into shift data
       print sft.x, sft.y, sft.z, sft.rx, sft.ry, sft.rz
     endif
 
     argosx.close() # closing socket
     end
```

If the fb0.do5 print signal is switched on when res( ) is executed, it means the error signal has been printed normally.

![](../../_assets/image_27.png)




Signal #5 should be used only for ArgosX errors. Therefore, it cannot be used for other applications that require it to be an assigned signal.



Using the xhost method below, you can designate a specific sigcode as assigned.
```python 
def io_assign_set_out_bit(sigcode: int) -> int
```

When you define an on_app_init( ) function in the main.py, then input a routine that designates an assignment, as shown below, the execution will occur the moment ArgosX is imported.



main.py
```python 
.. previous steps skipped
 
 
import xhost
 
 
...skipped
 
 
def on_app_init() -> int:
   """(callback) called just after self-diagnosis
   Returns:
      0
   """
   print('[argosx] on_app_init();')
   xhost.io_assign_set_out_bit(setup.sigcode_err)
   return 0
```

Execute the virtual controller again. Then, when Import ArgosX is executed in the job, reopen the universal output panel.

The designated signal will be displayed as assigned (bold).

![](../../_assets/image_28.png)

# 3.1.8 Manual for referring to the xhost module methods

<html>
<body>
<h2>get(url, query)</h2>

<h3>Description:</h3>
OpenAPI GET method.<br><br>
<h3>Args:</h3>
<b>url</b>: str. OpenAPI URL.<br>
<b>query</b>: str. OpenAPI query.<br>

<h3>Returns:</h3>
str. responded value.<br>
<br>

<hr>
<h2>put(url, body)</h2>

<h3>Description:</h3>
OpenAPI PUT method.<br><br>
<h3>Args:</h3>
<b>url</b>: str. OpenAPI URL.<br>
<b>body</b>: str. body of the request.<br>

<h3>Returns:</h3>
str. body of the response.<br>
<br>

<hr>
<h2>post(url, body)</h2>

<h3>Description:</h3>
OpenAPI POST method.<br><br>
<h3>Args:</h3>
<b>url</b>: str. OpenAPI URL.<br>
<b>body</b>: str. body of the request.<br>

<h3>Returns:</h3>
str. body of the response.<br>
<br>

<hr>
<h2>hist_print(msg)</h2>

<h3>Description:</h3>
Same as printh() except the user-param/hist_print_level setting is applied.<br><br>
<h3>Args:</h3>
<b>msg</b>: str. message.<br>

<h3>Returns:</h3>
	
<br>

<hr>
<h2>printh(msg)</h2>

<h3>Description:</h3>
print to history log.<br><br>
<h3>Args:</h3>
<b>msg</b>: str. message.<br>

<h3>Returns:</h3>
	
<br>

<hr>
<h2>issue_alarm(task_no, type, code)</h2>

<h3>Description:</h3>
issue error or warning event.<br><br>
<h3>Args:</h3>
<b>task_no</b>: int. task number (0~7).<br>
<b>type</b>: <br>
&nbsp&nbsp <b>'E'</b>: error<br>
&nbsp&nbsp <b>'W'</b>: warning<br>
<b>code</b>: alarm code number.<br>

<h3>Returns:</h3>
	
<br>

<hr>
<h2>issue_notice(task_no, code, msg, delay_sec)</h2>

<h3>Description:</h3>
issue notice event.<br><br>
<h3>Args:</h3>
<b>task_no</b>: int. task number (0~7).<br>
<b>code</b>: int. alarm code number.<br>
<b>msg</b>: str. notice message.<br>
<b>delay_sec</b>: float. time to delay before hide (sec).<br>

<h3>Returns:</h3>
	
<br>

<hr>
<h2>set_job_state_msg(task_no, msg)</h2>

<h3>Description:</h3>
set the job state message on teach pendant.<br><br>
<h3>Args:</h3>
<b>task_no</b>: int. task number (0~7).<br>
<b>msg</b>: str. state message to show.<br>

<h3>Returns:</h3>
	
<br>

<hr>
<h2>io_set_so(sig_no, val)</h2>

<h3>Description:</h3>
set the system i/o output bit.<br><br>
<h3>Args:</h3>
<b>sig_no</b>: int. signal number (0~959).<br>
<b>val</b>: int. value to set. (1 or 0)<br>

<h3>Returns:</h3>
&nbsp&nbsp <b>0</b>: ok<br>
&nbsp&nbsp <b>-1</b>: index-range exceeded.<br>

<br>

<hr>
<h2>io_get_in_bit(sigcode)</h2>

<h3>Description:</h3>
get the user i/o input bit using sigcode.<br><br>
<h3>Args:</h3>
<b>sigcode</b>: int. signal-code (e.g. 30017 for fb3.di17)<br>

<h3>Returns:</h3>
signal value, 0 or 1.<br>
<br>

<hr>
<h2>io_set_out_bit(sigcode, val)</h2>

<h3>Description:</h3>
get the user i/o output bit using sigcode.<br><br>
<h3>Args:</h3>
<b>sigcode</b>: int. signal-code (e.g. 30017 for fb3.do17)<br>
<b>val</b>: int. value to set. (1 or 0)<br>

<h3>Returns:</h3>
&nbsp&nbsp <b>0</b>: ok<br>
&nbsp&nbsp <b>-1</b>: index-range exceeded.<br>

<br>

<hr>
<h2>io_set_pulse_by_sigcode(sigcode, onoff, count, on_ms, off_ms, lag_ms, non_update)</h2>

<h3>Description:</h3>
make the i/o pulse output<br><br>
<h3>Args:</h3>
<b>sigcode</b>: int. signal-code (e.g. 30017 for fb3.do17)<br>
<b>onoff</b>: <br>
&nbsp&nbsp <b>1</b>: on-pulse<br>
&nbsp&nbsp <b>0</b>: non-pulsed off (lagged-off)<br>
&nbsp&nbsp <b>-1</b>: off-pulse<br>
<b>count</b>: int. pulse count.<br>
<b>on_ms</b>: int. width of on (msec).<br>
<b>off_ms</b>: int. width of off (msec).<br>
<b>lag_ms</b>: int. width of lag (msec).<br>
<b>non_update</b>: <br>
&nbsp&nbsp <b>1</b>: don't update if the pulse is already registered.<br>
&nbsp&nbsp <b>0</b>: reregister the pulse if the pulse is already registered.<br>

<h3>Returns:</h3>
&nbsp&nbsp <b>0</b>: ok<br>
&nbsp&nbsp <b>-2</b>: already registered (when non_update==2).<br>

<br>

<hr>
<h2>io_assign_set_in_bit(sigcode)</h2>

<h3>Description:</h3>
set sigcode as the assigned input i/o<br><br>
<h3>Args:</h3>
<b>sigcode</b>: int. signal-code (e.g. 30017 for fb3.di17)<br>

<h3>Returns:</h3>
0               ok -1              invalid sigcode<br>
<br>

<hr>
<h2>io_assign_set_out_bit(sigcode)</h2>

<h3>Description:</h3>
set sigcode as the assigned output i/o<br><br>
<h3>Args:</h3>
<b>sigcode</b>: int. signal-code (e.g. 30017 for fb3.do17)<br>

<h3>Returns:</h3>
	0               ok -1              invalid sigcode<br>
<br>

<hr>
<h2>io_set_triggout(task_no, fbname, val, ofs, ax_no, type)</h2>

<h3>Description:</h3>
trigger output i/o<br><br>
<h3>Args:</h3>
<b>task_no</b>: int. task number (0~7).<br>
<b>fbname</b>: str. output variable name (e.g. fb3.do17, dob3)<br>
<b>val</b>: int. output value.<br>
<b>ofs</b>: int. offset-time (msec), or offset-distance (mm)<br>
<b>ax_no</b>: int. 0(TCP), 1~(axis-number) ; axis that is observed when the type is OD<br>
<b>type</b>: <br>
&nbsp&nbsp <b>0x01</b>: OT (time-based)<br>
&nbsp&nbsp <b>0x02</b>: OD (distance-based) ; relative distance from the previous position<br>
&nbsp&nbsp <b>0x04</b>: output even if it is an assigned i/o<br>
&nbsp&nbsp <b>0x10</b>: OX (absolute position of X)<br>
&nbsp&nbsp <b>0x20</b>: OY (absolute position of Y)<br>
&nbsp&nbsp <b>0x30</b>: OZ (absolute position of Z)<br>

<h3>Returns:</h3>
&nbsp&nbsp <b>1</b>: buffer full<br>
&nbsp&nbsp <b>2</b>: complete<br>
&nbsp&nbsp <b>0</b>: ok<br>
&nbsp&nbsp <b>-1</b>: the robot axis is locked. (when, ax_no==0)<br>
&nbsp&nbsp <b>-2</b>: the Nth robot axis is locked<br>
&nbsp&nbsp <b>-3</b>: unsupported coordinate system<br>
&nbsp&nbsp <b>-4</b>: the user coordinate system number is unmatched<br>

<br>

<hr>
<h2>io_n_blocks()</h2>

<h3>Description:</h3>
	
<h3>Args:</h3>
	
<h3>Returns:</h3>
int. The number of i/o blocks<br>
<br>

<hr>
<h2>io_size_block_addr()</h2>

<h3>Description:</h3>
	
<h3>Args:</h3>
	
<h3>Returns:</h3>
int. The number of bits (address-space) in a block.<br>
<br>

<hr>
<h2>io_fbname_from_sigcode(sigcode, is_out)</h2>

<h3>Description:</h3>
get the fbname from sigcode.<br><br>
<h3>Args:</h3>
<b>sigcode</b>: int. signal-code (e.g. 30017)<br>
<b>is_out</b>: 1               output 0               input<br>

<h3>Returns:</h3>
the fbname of the sigcode (e.g. fb3.di17)<br>
<br>

<hr>
<h2>solve_expr_as_string(task_no, expr)</h2>

<h3>Description:</h3>
solve the expression and return the result value.<br><br>
<h3>Args:</h3>
<b>task_no</b>: int. task number (0~7).<br>
<b>expr</b>: str. expression of robot language.<br>

<h3>Returns:</h3>
str. the result value of the expression.<br>
<br>

<hr>
<h2>solve_expr_as_int(task_no, expr)</h2>

<h3>Description:</h3>
solve the expression and return the result value as an integer.<br><br>
<h3>Args:</h3>
<b>task_no</b>: int. task number (0~7).<br>
<b>expr</b>: str. expression of robot language.<br>

<h3>Returns:</h3>
int. the resulting integer value of the expression.<br>
<br>

<hr>
<h2>exec_mode()</h2>

<h3>Description:</h3>
	
<h3>Args:</h3>
	
<h3>Returns:</h3>
&nbsp&nbsp <b>True</b>: exec-mode.<br>
&nbsp&nbsp <b>False</b>: not exec-mode.<br>

<br>

<hr>
<h2>cont_mode()</h2>
<h3>Description:</h3>	
<h3>Args:</h3>
	
<h3>Returns:</h3>
&nbsp&nbsp <b>True</b>: cont-mode.<br>
&nbsp&nbsp <b>False</b>: not cont-mode.<br>

<br>

<hr>
<h2>req_to_continue()</h2>

<h3>Description:</h3>
request host to be in continue mode
<h3>Args:</h3>
	
<h3>Returns:</h3>
	
<br>

<hr>
<h2>set_err_code(code)</h2>

<h3>Description:</h3>
set error code.<br><br>
<h3>Args:</h3>
<b>code</b>: int. error code.<br>

<h3>Returns:</h3>
	
<br>

<hr>
<h2>lang_timer()</h2>

<h3>Description:</h3>
	
<h3>Args:</h3>
	
<h3>Returns:</h3>
int. current value of the language-timer.<br>
<br>

<hr>
<h2>set_lang_timer(timeout)</h2>

<h3>Description:</h3>
	set value to the language-timer.<br><br>
<h3>Args:</h3>
<b>timeout</b>: int. initial value (msec)<br>

<h3>Returns:</h3>
	
<br>

<hr>
<h2>branch_to_addr(addr)</h2>

<h3>Description:</h3>
branch to the address.<br><br>
<h3>Args:</h3>
<b>addr</b>: str. address to branch.<br>

<h3>Returns:</h3>
	
<br>

<hr>
<h2>abs_path(name)</h2>

<h3>Description:</h3>
get absolute-path in the file-system.<br><br>
<h3>Args:</h3>
<b>name</b>: str. 'home', 'project', 'log', 'jobs', 'vars', 'backup', 'fbrr', 'module', 'apps_main', or 'help'<br>

<h3>Returns:</h3>
absolute-path<br>
<br>

<hr>
<h2>sci_open(port)</h2>

<h3>Description:</h3>
serial port open<br>        Args:<br>        port (int): serial port number<br><br>
<h3>Args:</h3>
	
<h3>Returns:</h3>
&nbsp&nbsp <b>0</b>: OK<br>
&nbsp&nbsp <b>'<0'</b>: Not OK<br>

<br>

<hr>
<h2>sci_close(port)</h2>

<h3>Description:</h3>
serial port close<br>        Args:<br>        port (int): serial port number<br>        <br>
<h3>Args:</h3>
	
<h3>Returns:</h3>
&nbsp&nbsp <b>0</b>: OK<br>
&nbsp&nbsp <b>-1</b>: already closed.<br>

<br>

<hr>
<h2>sci_send_bytes(port, data)</h2>

<h3>Description:</h3>
serial communication sends byte-type data<br>xhost dbg not supported<br>
<h3>Args:</h3>
<b>port (int)</b>: serial port number<br>
<b>data (bytes)</b>: input data<br>

<h3>Returns:</h3>
&nbsp&nbsp <b>0</b>: OK<br>
&nbsp&nbsp <b>-1</b>: Not OK<br>

<br>

<hr>
<h2>sci_recv_bytes(port, len)</h2>

<h3>Description:</h3>
serial communication receives byte-type data<br>xhost dbg not supported
<h3>Args:</h3>
	
<h3>Returns:</h3>
	
<br>

<hr>
<h2>sci_clear_buf(port)</h2>

<h3>Description:</h3>
	
<h3>Args:</h3>
	
<h3>Returns:</h3>
	
<br>

<hr>
<h2>sci_send(port, data)</h2>

<h3>Description:</h3>
serial communication sends string-type data<br><br>
<h3>Args:</h3>
	
<h3>Returns:</h3>
&nbsp&nbsp <b>0</b>: OK<br>
&nbsp&nbsp <b>-1</b>: Not OK<br>

<br>

<hr>
<h2>sci_recv(port)</h2>

<h3>Description:</h3>
serial communication receives string-type data<br><br>
<h3>Args:</h3>
<b>port (int)</b>: serial port number<br>

<h3>Returns:</h3>
&nbsp&nbsp <b>'str'</b>: receive string data<br>

<br>

<hr>
</body>
</html>

# 3.1.9 Solving the robot language function-blocking problem
## Blocking problem


There is one problem with the recv_msg( ) function implemented in the previous section.

When this function is called, there will be a waiting period for a remote response, and only when the response is received will the operation of the function end. If there is no response because of a problem with the ArgosX system, which is supposed to respond, waiting will continue permanently without the function ending.



Let's check how the job program operates in a situation where there is no response from argosx_sub.py.

For the purposes of testing, find a variable called sleep_sec in argosx_stub.py and change its value to 20. Then, when receiving a "req ~" request, argosx_stub will wait 20 seconds before responding with a "res ~" response.

 

argosx_stub.py
``` python 
# const
buf_size = 0x8000    # 32kb ; permitted packet length
port_no = 54321      # port for ArgosX command
sleep_sec = 20        # delay before response
```


Let's execute argosx_stub.py again, then run the job program line by line with the teach pendant's STEP FWD key.

If you execute argosx.res( ) right after executing argosx.req, it will be impossible to move the cursor for 20 seconds after the req is executed, as you can see below. This state is called blocking, and it is not good in terms of user operability. This specification is problematic because it can cause a situation where you have to turn the robot controller off and on if there is no response permanently.
<br></br>
![](../../_assets/image_29.png)

Which specification would be desirable? In general, when a command is waiting for a certain state, such as the wait command, waiting will occur when the STEP FWD key is pressed, and the cursor can be moved when the key is not pressed. In addition, a timeout and the escape address can be designated as arguments, so when a timeout occurs, the exception-handling operation to branch to the escape address can be performed.
</br>
![](../../_assets/image_30.png)
If a timeout occurs after 10 seconds in the wait-di6 state, branching to *tout will occur. 



## Execution mode and continue mode


Let’s take a look at the flow chart below. There are two modes where the Hi6 host calls the robot language commands: execution mode and continue mode. In continue mode, the host calls the command again.

The host calls commands in execution mode first. In most cases, individual commands perform their operations and end immediately, while the host completes the handling of the commands after confirming that it is not in continue mode.
</br>
![](../../_assets/image_31.png)
</br>

However, some commands have a wait operation (meaning it waits for a certain state or event, such as I/O input, Ethernet data reception, certain periods of time, robot operation completion, etc.) The following procedure will be performed between the host and a plug-in. 

* The plug-in’s wait operation command will check the mode with xhost.exec_mode( ). True means execution mode and false means continue mode. As such, if the mode is confirmed to be execution mode (Yes), the time, transferred to the timeout argument, will be set to the timer for the robot language (set_lang_timer), and the Hi6 host will be requested to call in continue mode next time before the operation ends.
* If the operation ends after the execution of xhost.req_to_continue, it means the mode is continue mode. Accordingly, the Hi6 host calls the relevant command again.
* The plug-in’s wait operation command will check the mode with xhost.exec_mode( ). If the mode is confirmed to be continue mode (No), the timer will be checked. If a timeout has occurred, branching to the escape address (branch_to_addr) will occur, and the operation will end without making a request for continue mode.
* If a timeout has not occurred, whether the wait condition is complete (wait-complete condition?) will be checked. If the wait condition is complete, the operation will end as is because there is no request for continue mode, but if it is not complete, the operation will end without the Hi6 host being requested to call in continue mode (req_to_continue.)
* If there is no request for continue mode in the current call (continue-mode No), the Hi6 host will complete the handling of the relevant command.
 </br>
 ![](../../_assets/image_32.png)
</br>



Now let's improve argosx.res( ) in such a way that it can also have the specifications of the wait operation.



## Making a non-blocking comm module


For the comm module implemented for Ethernet transmission/reception, a socket module is used internally. A socket is in blocking mode by default, meaning that the socket.recvfrom( ) function, a UDP reception function, does not perform any return operation until data is received.

First, we need to change the socket instance, which we used, to non-blocking mode. Insert sock.setblocking(False) into the comm.open( ) function, as shown below.



comm.py
``` python
def open(ip_addr: str, port: int) -> int:
   """
   open socket for UDP communication
   Args:
      ip_addr     ip adddress of remote. e.g. "192.168.1.172"
      port        port# of remote. e.g. "192.168.1.172"
 
 
   Returns:
         0     ok
         -1    error
   """
   global raddr, sock
   try:
      raddr = (ip_addr, port)
      sock = socket.socket(family=socket.AF_INET, type=socket.SOCK_DGRAM)
      sock.setblocking(False)
   except socket.error as e:
      print("socket creation or binding error :", e)
      return -1
   logd('comm.open: ' + str(raddr))
   return 0
```

Now, for the socket.recvfrom( ) function, if there is no data received, a BlockingIOError exception will occur immediately without blocking taking place (if data is received, the return operation will occur immediately).

Insert the handling for returning with an empty string upon the occurrence of the BlockingIOError exception into the comm.recv_msg( ).



comm.py
``` python
def recv_msg():
   """
   wait msg from sock
   Returns:
      received string
   """
   if sock is None: return ""
 
 
   try:
      data, ip_port = sock.recvfrom(buf_size)
      bts = bytearray(data)
      msg = bts.decode()
      logd('response: ' + msg)
      return msg
   except BlockingIOError:
      return ""
   except Exception as e:
      print('exception from recv_msg(): ' + str(e))
      return ""

```


## Implementing a wait operation in the res( ) function


Add two arguments, timeout and addr_on_timeout (escape address), into the res( ) function as follows.

If a timeout is not designated, the default value, -1, will be applied, causing infinite waiting period to occur. If the escape address is not designated, the default value, -1, will be applied, causing the process to go to the next command without branching occurring upon timeout.

Remove the existing implementation. After that, implement a wait operation in a simple form where the mode will be checked with the xhost.exec_mode( ) function. If the mode is execution mode, the res_exec( ) will be called, but if it is in continue mode, the res_cont( ) function will be called.



roblang.py
``` python 
""" ArgosX Vision System interface - robot language
 
 
@author:    Jane Doe, BlueOcean Robot & Automation, Ltd.
@created:   2021-12-06
"""
 
from . import comm
from . import setup
 
import xhost
import typing
 
 
# type
int_or_str = typing.Union[int, str]
 
 
 
Skipped...
 
 
 
def res(timeout: int=-1, addr_on_timeout: int_or_str=-1) -> str:
   """
   wait response from ArgosX
    
   Args:
      timeout: (ms), Default(-1) means infinite.
      addr_on_timeout: branch address on timeout.
         (e.g. 99, "S7", "*TimeOut")
         Default(-1) means no branch.
 
 
   Returns:
      response string from ArgosX.
      "" if failed.
      e.g. "[30, 25.7, 11.9, 31.6, 12.8, -54.6]"
   """
   ret = ""
   if xhost.exec_mode():
      _res_exec(timeout)
   else:
      ret = _res_cont(addr_on_timeout)
   return ret
```



Now, let’s implement the _res_exec( ) right below the res( ) function. For this implementation, the timer for the robot language will be set to the timeout argument value, and the host will be requested to call in continue mode before the operation ends. Isn’t it simple?



roblang.py
``` python
Previous steps skipped...
 
 
def _res_exec(timeout: int) -> None:
   """res() implementation for exec-mode"""
   xhost.set_lang_timer(timeout)
   xhost.req_to_continue()
```

The actual operation will be performed in the _res_cont( ) function for continue mode. The branching upon timeout operation is created as the check_timeout_and_branch( ) function.

If a timeout occurs, the sigcode_err output signal will be turned on, and the operation will end immediately.

If a timeout does not occur, data reception will be performed. If there was no data received while this process occurred, the host will be requested to call in continue mode (xhost.req_to_continue). After that, the operation of returning with an empty string will be performed.




roblang.py
``` python
Previous steps skipped...
 
 
def _res_cont(addr_on_timeout: int_or_str) -> str:
   """res() implementation for cont-mode"""
   val = 0
   msg = ""
   timeout = _check_timeout_and_branch(addr_on_timeout)
   if timeout:
      val = 1
   else:
      msg = comm.recv_msg()
      print(msg)
      if msg=="res fail":
         val = 1
         msg = ""
      elif msg=="":    # No response
         xhost.req_to_continue()
      else:    # Normal response
         msg = get_base_shift_array_from_res(msg)
   xhost.io_set_out_bit(setup.sigcode_err, val)
   return msg
 
 
 
def _check_timeout_and_branch(addr_on_timeout: int_or_str) -> bool:
   """if timeout, do branch.
 
   Returns:
      True     timeout. branched.
      False    not-timeout.
   """
   timer = xhost.lang_timer()
   if timer!=0: return False  # not-timeout
   # timeout!
   if addr_on_timeout==-1:
      return True
   # make as str, unconditionally
   str_addr = str(addr_on_timeout)
   xhost.branch_to_addr(str_addr)
   return True
```



## Testing the non-blocking operation  


Now let's check if the specifications were made as we desired. Execute the virtual controller again and run the job program line by line with the teach pendant's STEP FWD key.

If you execute argosx.req, then execute argosx.res( ) immediately after, the operation will not complete because the response did not come in yet. When you release the STEP FWD key, the step forward indicator will be turned off, allowing you to move the cursor. If you press the STEP FWD key, the waiting state will resume.

If 20 seconds pass after the execution of req( ) while the STEP FWD key is pressed, reception will be completed.
</br>
![](../../_assets/image_33.png)



Now, let's add a timeout argument. Designate it as 3000 ms and execute the operation again. If 3 seconds pass while the STEP FWK key is pressed, it will move to the next command.

job
```
var str=argosx.res(3000) # waiting for a response
 
print str
```

Now, let's add an escape step as well. When you teach it as follows and press the STEP FWD key for more than 3 seconds on the res( ) function, you can see that branching to line number 99 occurs and "timeout" will be executed.



job
```
... Previous steps skipped
      
     var str=argosx.res(3000,99) #waiting for a response
     print str
     if str==""
       print "req error"
       stop
     else
       var sft=Shift(str) # converting the shift array string into shift data
       print sft.x, sft.y, sft.z, sft.rx, sft.ry, sft.rz
     endif
      
     argosx.close()
     end
      
  99 print "timeout"
     end
```

# 3.2 Practical project: ArgosX - callback


While operating the Hi6 controller, there are main events, such as Mode Change, Motor On, Reset, Start and Accuracy OK. We can register functions into a plug-in for it to perform unique operations for an event.

These functions are callback functions. They are referred to as such because they are called by other parts (Hi6 host) and not actively called within the Python code.



* Registering a callback function
* Implementing a callback function
* Manual for referring to the callback functions

# 3.2.1 Registering a callback function


The method of registering a callback function is very simple. The callback function name is determined already for each event, so, by defining a callback function in the plug-in code using the relevant callback function name, the callback function will be registered automatically when the plug-in is imported.


We need to refer to the other functions of <U>3.1.1 Specifications of ArgosX and interface plug-ins</U> again.

Depending on whether the robot is in the motor ON or motor OFF state, ArgosX's LED light should be also switched on or off accordingly. Specifically, the commands "light-on" or "light-off" should be transmitted to ArgosX.



Let's create the callback functions in a separate file (Python module) as follows. For now, we will just print the strings for testing without performing specific operations.



callback.py (for testing)
```python
def on_motor_on() -> int:
   """(callback) on motor-on
   Returns: 0
   """
   print('on_motor_on')
   return 0
 
 
def on_motor_off() -> int:
   """(callback) on motor-off
   Returns: 0
   """
   print('on_motor_off')
   return 0
```

Import the callback module from the entry file.



main.py
```python
Previous steps skipped...
 
from . import setup
from .roblang import *
from .setup import *
from .callback import *
 
import xhost
 
Subsequent steps skipped...

```
Reboot the controller, turn on the motor, and import ArgosX using the Step FWD button.

In this state, if the result is printed on the console window as follows every time the motor is turned on or off, it means the callback function is well defined.
```
on_motor_on

on_motor_off
```
# 3.2.2 Implementing a callback function
Because we ensured that the callback functions are called well, let's implement the actual operations.

As you can see by checking <U>3.1.1 Specifications of ArgosX and interface plug-ins</U>, you just need to send the "light-on" and "light-off" messages to the ArgosX hardware.



Because the comm module already has a function implemented to send an Ethernet string, you only need to call one as follows.
```
comm.send_msg("light-on")
comm.send_msg("light-off")
```

However, there is one problem with this implementation. If the comm.open( ) function is called through argosx.init, a robot language command, the string will be transmitted normally. However, if the function is not called, the string will not be transmitted.

In addition, transmissions will not occur even when communications are closed because of comm.close( ).

Therefore, it is necessary to define a string transmission function that makes it possible to open communications, if it is in closed state, and carry out transmissions and close communications.


Create a comm_ex.py file, as shown below, in the same folder where comm.py exists.



comm_ex.py

``` python
""" ArgosX Vision System interface - main
 
 
@author:    Jane Doe, BlueOcean Robot & Automation, Ltd.
@created:   2021-12-07
"""
 
 
from . import setup
from . import comm
 
 
def send_msg_once(msg: str) -> int:
   was_closed = (comm.is_open()==False)
   if was_closed: comm.open(setup.ip_addr, setup.port)
   iret = comm.send_msg(msg)
   if was_closed: comm.close()
   return iret

```

Now, we can simply implement the callback functions, as shown below, by importing the comm_ex module.



callback.py

``` python
""" ArgosX Vision System interface - callback functions
 
 
@author:    Jane Doe, BlueOcean Robot & Automation, Ltd.
@created:   2021-12-07
"""
 
from . import comm_ex
 
 
def on_motor_on() -> int:
   """(callback) on motor-on
   Returns: 0
   """
   print('on_motor_on')
   return comm_ex.send_msg_once("light-on")
 
 
def on_motor_off() -> int:
   """(callback) on motor-off
   Returns: 0
   """
   print('on_motor_off')
   return comm_ex.send_msg_once("light-off")
```

First, execute argosx_stub from the command prompt or vscode.

Reboot the virtual controller, then run the job file up to argosx.init( ). If the operation was performed as follows in this state, it means the normal lighting function's operation was checked.



<U>__argosx_stub side (server playing the role of ArgosX)__</U>

Every time the motor OFF and motor ON functions occur, the following strings will be printed on the console.
```
request : light-off
LED light is OFF

request : light-on
LED light is ON
```
# 3.2.3 Manual for referring to the callback functions

<table>
  <thead>
    <tr>
      <th style="text-align:left">Python callback functions</th>
      <th style="text-align:left">Point in time when calling occurs inside the main software</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>on_app_init()</td>
      <td>
       After self-diagnosis
      </td>
    </tr>
   <tr>
      <td>on_before_self_diagnosis_proc()</td>
      <td>
       Before self-diagnosis
      </td>
    </tr>
    <tr>
      <td>on_mot_servoerror_detect()</td>
      <td>
       When detecting a servo error
      </td>
    </tr>
    <tr>
      <td>on_mot_on_ready_check_remote_auto()</td>
      <td>
       Motor on ready, remote/auto
      </td>
    </tr>
    <tr>
      <td>on_entry_teach_mode()</td>
      <td>When entering teaching mode</td>
    </tr>
    <tr>
      <td>on_entry_auto_mode()</td>
      <td>
        When entering auto mode
      </td>
    </tr>
     <tr>
      <td>on_system_status_chk_proc()</td>
      <td>When checking the system for any abnormalities (10 ms)</td>
    </tr>
    <tr>
      <td>on_period_low()</td>
      <td>
       When calling based on the lowest priority cycle (5 ms)
      </td>
    </tr>
   <tr>
      <td>on_motor_on()</td>
      <td>
       Motor on
      </td>
    </tr>
    <tr>
      <td>on_motor_off()</td>
      <td>
       Motor off
      </td>
    </tr>
    <tr>
      <td>on_stop(task_no)</td>
      <td>
       When stopping
      </td>
    </tr>
    <tr>
      <td>on_restart(task_no)</td>
      <td>When starting</td>
    </tr>
    <tr>
      <td>on_reset0()</td>
      <td>
       reset 0	
      </td>
    </tr>
     <tr>
      <td>on_cur_job_selected_by_tp(task_no)</td>
      <td>When the TP selects the job program.</td>
    </tr>
    <tr>
      <td>on_step_func_no_change_for_clear(task_no,..)</td>
      <td>
       When changing the program counter
      </td>
    </tr>
    <tr>
      <td>on_job_end(task_no)</td>
      <td> Executing the job end command
      </td>
    </tr>
    <tr>
      <td>init_signal_output_status()</td>
      <td>When initializing the application signal output</td>
    </tr>
    <tr>
      <td>set_ext_io_sig_proc()</td>
      <td>
       When handling assigned signals	
      </td>
    </tr>
    <tr>
      <td>is_assigned_input(sigcode) /</br>
      is_assigned_output(sigcode)</td>
      <td>When returning whether sigcode is assigned for the input/output signals</td>
    </tr>
    <tr>
      <td>update_input_assign_info() / </br>
      update_output_assign_info()</td>
      <td>When setting up the look-up table for the host's assigned input/output signals</td>
    </tr>
  </tbody>
</table>

# 3.3 Practical project: Developing an ArgosX setup screen UI

If there are various settings in a plug-in, we need a setup screen that shows the setup values to the user and allows the user to change them to new values when necessary.

In this section, let’s practice implementing an ArgosX setup screen UI and deploy specific menu items to their locations.
</br>


* Specifications of the ArgosX setup screen user interface
* Layout of the setup screen
* Operating the setup screen
* Injecting a menu into the description screen
* Loading and saving the values of the setup screen
* Loading and saving the setup files
* Operating the F buttons - Initializing to default values
# 3.3.1 Specifications of the ArgosX setup screen user interface

Let’s create the user interface of the setup screen with the following specifications.

* In the Setup - Application Parameter menu, there is a menu to enter the ArgosX setup screen.
* In the ArgosX setup screen, you can set the IP address and port number of the ArgosX system
* The content of the setup will be saved into the argosx.json file as a json file.
* An F button (Initialize All) that sets the entire screen to default values is provided.
* An F button (Initialize One) that sets only the currently selected items to default values is provided.

![](../../_assets/image_34.png)





Regardless of whether ArgosX is imported to the job program, we want to make opening the setup screen and checking/changing the setup values possible.

Change the startup item in the argosx/info.json file from "manual" to "boot". Now, as ArgosX will be imported while the controller is booting, you do not need to import ArgosX to the job program.

(You can still carry out the setups in HRScript.)
# 3.3.2 The layout of the setup screen


Open vscode for the apps/ folder that is the parent of the ArgosX folder.

(This step is performed because the ArgosX user interface needs to refer to the files in apps/_common/. A folder containing all the files referred to by the Live server should be opened as a top-level folder in the workspace.)
</br>
![](../../_assets/image_35.png)
</br>



Creating a ui/ folder by clicking the New Folder button.
</br>
![](../../_assets/image_36.png)
</br>




Create a ui/setup.html file.
</br>
![](../../_assets/image_37.png)
</br>



Write the content as follows. 




setup.html
``` html
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
</head>
  
<body>
   <div>
      <div id='contents'>
         <span class='col0' name='ip_addr'>IP address</span>
         <input class='col1' type='text' name='ip_addr' id='ip_addr_0' size='3'/>
         .
         <input class='col1' type='text' name='ip_addr' id='ip_addr_1' size='3'/>
         .
         <input class='col1' type='text' name='ip_addr' id='ip_addr_2' size='3'/>
         .
         <input class='col1' type='text' name='ip_addr' id='ip_addr_3' size='3'/>
         <br>
         <span class='col0' name='port'>Port#</span>
         <input class='col1' type='text' id='port' size='5'/>
         <br>
         <span class='col0' name='fail_out_sig'>Failure output signal</span>
         <input class='col1' type='text' id='fail_out_sig' size='5'/>
      </div>
   </div>
</body>
</html>
```

While setup.html is opened, click the Go Live button on the bottom right to run the Live server.
</br>
![](../../_assets/image_38.png)
</br>




If a security warning about vscode appears, tick all items under Permit Communications and click the Allow Access button.
</br>
![](../../_assets/image_39.png)
</br>




Alternatively, you should open a pop-up menu by right-clicking the mouse on setup.html, then select "Open with Live Server."
</br>
![](../../_assets/image_40.png)
</br>




When the Google Chrome browser opens, we can check the sketchy layout.



Sketchy layout of setup.html
</br>
![](../../_assets/image_41.png)
</br>



# 3.3.3 Operating the setup screen

As shown below, add the scripts into the head of setup.html.

The setup.js file is a script file designed to implement operations that are to be applied only on this setup screen. Additional descriptions will be provided below.



ui/setup.html
``` html
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
   <script src='../../_common/js/dst_setup.js'></script>
   <script src='./setup.js'></script>
   <script>
      $(document).ready(init);
   </script>
</head>
  
<body>
   <div>
      <div id='contents'>
         <span class='col0' name='ip_addr'>IP address</span>
         <input class='col1' type='text' name='ip_addr' id='ip_addr_0' size='3'/>
         .
         <input class='col1' type='text' name='ip_addr' id='ip_addr_1' size='3'/>
         .
         <input class='col1' type='text' name='ip_addr' id='ip_addr_2' size='3'/>
         .
         <input class='col1' type='text' name='ip_addr' id='ip_addr_3' size='3'/>
         <br>
         <span class='col0' name='port'>Port#</span>
         <input class='col1' type='text' id='port' size='5'/>
         <br>
         <span class='col0' name='sigcode_err'>Failure output signal</span>
         <input class='col1' type='text' id='sigcode_err' size='5'/>
      </div>
      <div id='guidebar'></div>
   </div>
</body>
</html>
```



Add the setup.js file into the ui/ folder and write the following content.



ui/setup.js
``` js
///@author: Jane Doe, BlueOcean Robot & Automation, Ltd.
///@brief: ArgosX Vision System interface - setup
///@create: 2021-12-06
 
 
 
function init()
{
   setDomPath("/apps/argosx/svr_setup");
   setUpdateGuideBar(updateGuideBar);
   setUpdateData(updateData);
   onReady();
}
 
 
///@return     f-button infos array
function initButtonBar()
{
   console.log('initButtonBar()'); 
   var btn_infos = [
   ]
   return btn_infos;
}
 
 
///@brief      have guidebar display message on clicking widget
function updateGuideBar()
{
   let sg = setGuideBarMsg;
   let msg_ip_addr = 'Enter the IP address of ArgosX.'
   let msg_port = 'Enter the port # of ArgosX.'
   let msg_sigcode = 'Enter the number of the signal to assign.[0 - 4096]';
    
   sg('ip_addr', msg_ip_addr);
   sg('port', msg_port);
   sg('sigcode_err', msg_sigcode);
}
 
 
///@param[in]  data
///@param[in]  to_data     true; widget->data, false; widget<-data
function updateData(data, to_data)
{
   ddx_edit_ip(data, 'ip_addr', to_data);
   ddx_edit_i(data, 'port', to_data);
   ddx_edit_sig(data, 'sigcode_err', to_data);
}
```

When the cursor is located on the input element, the updateGuideBar( ) function will call the setGuideBarMsg( ) function, then it will designate the message to be displayed in the guidance frame.



__setGuideBarMsg(The ID or name of the element and the message to be displayed)___



For example, sg('ip_addr', msg_ip_addr); in the example above refers to the setting that displays the msg_ip_addr string in the guidance frame when the cursor is located on the input element with the name 'ip_addr.'



When the setup screen is opened initially, the current setup value needs to be loaded. Upon pressing the [OK] button, the value should be saved.

Operations related to these settings are to be implemented by calling setDomPath("/apps/argosx/svr_setup"); and defining the updateData() function.

More detailed descriptions will be provided in the subsequent sections.
# 3.3.4 Injecting a menu into the description screen

We have already practiced injecting a menu through the hello_world example.

Let's inject the ArgosX setup screen under the System - Application Parameter menu of the teach pendant.



Create a menu.json file, as shown below, under the ui/ folder.



menu.json
``` json
[
    {
        "path": "system/appl/",
        "id": "argosx",
        "icon": "argosx/ui/lm_argosx.png",
        "label": "ArgosX Vision",
        "url": "argosx/ui/setup.html"
    }
]
```


Let’s inject a picture icon this time. Using the two websites below, we obtained a png icon with a transparent 104 x 104-pixel background.



Bootstrap Icons (https://icons.getbootstrap.com/#icons): An open source icon library. Icons are provided in an SVG vector file format.

EZGIFCOM (https://ezgif.com/svg-to-png): Performs online conversions of SVG files into PNG files with desired resolutions.

</br></br>
![](../../_assets/lm_argosx.png) Example of lm_argosx.png (You can download and use this picture.)



Now, we should run the virtual mainboard and virtual teach pendant again.


When entering the System _ Application Parameter menu, you can find the newly added ArgosX Vision menu item, as shown below.
</br>
![](../../_assets/image_42.png)
</br>



When you select the menu, the layout we wrote will appear shortly.
</br>
![](../../_assets/image_43.png)
</br>






# 3.3.5 Loading and saving the values of the setup screen



When the setup screen is opened, the setup values saved in the Python plug-ins will be loaded as intermediate JavaScript objects, then displayed as HTML elements on the screen.

When the user modifies the values and clicks the [OK] button, the HTML elements on the screen will be created as intermediate JavaScript objects and saved into the variables within the Python plug-ins.



To perform the transfer, implement JavaScript's updateData( ) function and the getter and putter functions of Python's setup module.
</br>![](../../_assets/image_44.png)</br>






## element ↔ javascript object


setup.js
``` js
Previous steps skipped...
 
 
///@param[in]  data
///@param[in]  to_data     true; element->data, false; element<-data
function updateData(data, to_data)
{
   ddx_edit_ip(data, 'ip_addr', to_data);
   ddx_edit_i(data, 'port', to_data);
   ddx_edit_sig(data, 'sigcode_err', to_data);
}
```


The two-way transfer of values between an element and a JavaScript object is defined with the updateData( ) function. The parameter data is a JavaScript object, while to_data is a boolean variable and indicates the direction of the transfer. If it is true, the direction will be from element to data, while if it is false, the direction will be from data to element.

We can also implement the transfer directly using the document object model application programming interface (DOM API) or jQuery. However, the transfer can be implemented more concisely by using the dynamic data exchange (DDX) functions provided by dst_setup.js.


# 3.2.3 Manual for referring to the callback functions
|Function signature|HTML element|Data type|Description|
|---|---|---|---|
|ddx_edit(data, name, to_data)|```<input type='text'>```|string||
|ddx_edit_i(data, name, to_data)|```<input type='text'>```|integer||
|ddx_edit_sig(data, name, to_data)|```<input type='text'>```|If the value of the element for setting the universal I/O signals is sigcode, it will be transmitted as is.</br>If the value is in the form of fb?.?, it will be converted to sigcode and transmitted.|
|ddx_edit_ip(data, name, to_data)|```<input type='text'>``` x 4 (units)|string|This is for setting an IP address.</br>If the name is 'ip', each ID of the four elements should be 'ip_0', 'ip_1', 'ip_2', and 'ip_3.'</br>The data will be saved as "xxx.xxx.xxx.xxx."|
|ddx_check(data, name, to_data)|```<input type='checkbox'>```|boolean||
|ddx_radio(data, name, to_data)|```<input type='radio'>```x N (units)|integer|Each radio element should have a unique value attribute.|

</br>

## javascript object ↔ python data


setup.js
``` js
///@author: Jane Doe, BlueOcean Robot & Automation, Ltd.
///@brief: ArgosX Vision System interface - setup general
///@create: 2021-12-06
 
 
 
function init()
{
   setDomPath('/apps/argosx/svr_general');
   setUpdateGuideBar(updateGuideBar);
   setUpdateData(updateData);
   onReady();
}
 
 
...Subsequent steps skipped
```

During the initialization step, the "/apps/argosx/svr_general" path was designated with the setDomPath( ) function.

All plug-ins, including ArgosX, will be deployed under /apps/, and the last name of the path is "svr_" with the setup group name attached to it.

</br>

**/apps/{app name}/svr_{setup group name}**

</br>
A plug-in can have one or more setup screens, and the data on one screen will be treated as one object and referred to as a setup group. Each setup group can be named freely, and that name becomes the setup group name. In this example, the setup group name is determined as 'general.'

If you append the prefix "get_" or "put_", instead of "svr_", to a setup group name, it will become the getter or putter service function name, respectively. Moreover, if you add '_def' at the end of the getter function name, it will become the name of the default getter service function that acquires the default value.



Accordingly, in the example above, the default getter, getter, and putter service function names will become get_general_def( ), get_general( ), and put_general( ), respectively.



Add the setup.py file into the project folder argosx/.
<table>
  <thead>
    <tr>
      <th style="text-align:left"></th>
      <th style="text-align:left">Function</th>
      <th style="text-align:left">Point in time when calling occurs</th>
      <th style="text-align:left">Operation</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>default getter</td>
      <td>get_general_def( )</td>
      <td>When the default value is requested.</td>
      <td>Each default setup value will be saved into an object as an attribute value, and all of the objects will be returned.</td>
    </tr>
    <tr>
      <td>dgetter</td>
      <td>get_general( )</td>
      <td>When the setup screen is opened.</td>
      <td>>Each default setup value that a Python plug-in has will be saved into an object as an attribute value, and all of the objects will be returned.</td>
    </tr>
    <tr>
      <td>putter</td>
      <td>put_general( )</td>
      <td>When the values of the setup screen are saved with the [OK] or [Apply] buttons.</td>
      <td>The attribute values transferred to the body parameter will be read and saved in the Python plug-ins.</td>
    </tr>
  </tbody>
</table>


</br>

Now, let's implement individual functions. Each attribute's key should be the same name used in the DDX function.

setup.py
``` python

""" robot application - argosx - setup
 
 
@author:    Jane Doe, BlueOcean Robot & Automation, Ltd.
@created:   2021-12-06
"""
 
 
def get_general_def() -> dict:
   """
   Returns:
      default value of setting
   """
   print('def_general_def()')
 
 
   data_def = {
      'ip_addr': '192.168.1.100',
      'port' : 54321,
      'sigcode_err': 5
   }
    
   return data_def
 
 
def get_general() -> dict:
   """
   Returns:
      setting dict.
   """
 
   print('get_general()')
    
   ret = {}
   ret["ip_addr"] = ip_addr
   ret["port"] = port
   ret["sigcode_err"] = sigcode_err
    
   return ret
 
    
def put_general(body: dict) -> int:
   """
   Args:
      body  setting dict.
    
   Returns:
      0
   """
   global ip_addr, port, sigcode_err
   print('put_general()')
 
   ip_addr = body["ip_addr"]
   port = body["port"]
   sigcode_err = body["sigcode_err"]
 
   save_to_setup_file(body) # save to file
    
   return 0
```

Regarding the initialization of the global variables in setup.py, let's change the current method to another that uses the default getter function so that the code will not be duplicated.



setup.py
``` python
...Previous steps skipped
 
 
gen_def = get_general_def()
ip_addr : str = gen_def['ip_addr']
port : int = gen_def['port']
sigcode_err = gen_def['sigcode_err']
```



## Operation test


Now, let’s restart the virtual controller, import ArgosX, then enter the ArgosX setup screen. The default setup values will be shown as follows.
</br> ![](../../_assets/image_45.png) </br>




Change the IP address to 192.168.1.172, type 3.4 into Failure output signal, then press <Enter> to change its value to fb3.4. After that, press the [OK] button to exit the screen.
</br>

When you enter the screen again, if the newly set values are displayed well, it means the operation is normal.
</br> ![](../../_assets/image_46.png) </br>






# 3.3.6 Loading and saving the setup files

In the previous section, we practiced saving the setup values into the Python variables and loading them back.

To keep this setting, even when we turn the controller off and on, it should be saved into a file.



Add the save_to_setup_file( ) function, which is used for saving the settings to a file, and the load_from_setup_file( ) function, used for loading the settings from the file, to setup.py as follows.




setup.py
``` python 

""" robot application - argosx - setup
 
 
@author:    Jane Doe, BlueOcean Robot & Automation, Ltd.
@created:   2021-12-06
"""
 
import xhost
import json
 
 
fname_setup = 'argosx.json'
 
 
def get_general_def() -> dict:
   """
   Returns:
      default value of setting
   """
   print('def_general_def()')
 
   data_def = {
      'ip_addr': '192.168.1.100',
      'port' : 54321,
      'sigcode_err': 5
   }
    
   return data_def
 
 
def get_general() -> dict:
   """
   Returns:
      setting dict.
   """
 
   print('get_general()')
    
   ret = {}
   ret["ip_addr"] = ip_addr
   ret["port"] = port
   ret["sigcode_err"] = sigcode_err
    
   return ret
 
    
def put_general(body: dict) -> int:
   """
   Args:
      body  setting dict.
    
   Returns:
      0
   """
   global ip_addr, port, sigcode_err
   print('put_general()')
 
   ip_addr = body["ip_addr"]
   port = body["port"]
   sigcode_err = body["sigcode_err"]
 
   save_to_setup_file(body) # save to file
    
   return 0
 
 
def load_from_setup_file() -> int:
   """
   load setup file
   Returns:
         0     ok
         -1    error
   """
   global ip_addr, port, sigcode_err
 
   pathname = xhost.abs_path('project') + fname_setup
   try:
      with open(pathname, 'r') as file:
         data = json.load(file)
         ip_addr = data['ip_addr']
         port = data['port']
         sigcode_err = data['sigcode_err']
   except:
      print('file not found: ', pathname)
      return -1
   return 0
 
 
def save_to_setup_file(body: dict) -> int:
   """
   save to setup file
   Returns:
         0     ok
         -1    error
   """
   pathname = xhost.abs_path('project') + fname_setup
   try:
      with open(pathname, 'w') as file:
         json.dump(body, file, indent='\t')
   except:
      print('failed in file writing: ', pathname)
      return -1
   return 0
 
 
 
# attributes getter/setter
def get_ip_addr() -> str:
   return ip_addr
 
 
def set_ip_addr(addr: str):
   global ip_addr
   ip_addr = addr
 
def get_port() -> int:
   return port
 
def set_port(_port: int):
   global port
   port = _port
 
gen_def = get_general_def()
ip_addr : str = gen_def['ip_addr']
port : int = gen_def['port']
sigcode_err = gen_def['sigcode_err']
```
</br>

We added calling save_to_setup_file(body) as the last operation of the put_general( ) function for saving the values of the setup screen into the Python variables.

In addition, we added calling setup.load_from_setup_file( ) into the function on_app_init( ) of main.py as follows. The moment the ArgosX plug-in is imported, setup.load_from_setup_file( ) will be called to load the settings.


</br>

main.py
```python 
""" ArgosX Vision System interface - main
 
 
@author:    Jane Doe, BlueOcean Robot & Automation, Ltd.
@created:   2021-12-06
"""
 
from . import setup
from .roblang import *
from .setup import *
from .callback import *
 
import xhost
 
 
def attr_names() -> tuple:
   """Returns the names of the attributes to be exposed."""
   return ("ip_addr", "port")
 
 
def on_app_init() -> int:
   """(callback) called just after self-diagnosis
   Returns:
      0
   """
   print('[argosx] on_app_init();')
   setup.load_from_setup_file()
   xhost.io_assign_set_out_bit(setup.sigcode_err)
   return 0
```
</br>


Here, let's repeat the test we performed in the previous section.

Open the ArgosX setup screen, change the setup values, ​​such as the IP address and output assigned signal, and save them by pressing the [OK] key.

Check whether an argosx.json file is created in the project/ folder of the virtual controller, as shown below.

</br>
argosx.json (An example of changing the IP address to 192.168.1.172 and the output assigned signal for an error to fb3.4)

```json
{
    "port": 54321,
    "ip_addr": "192.168.1.172",
    "sigcode_err": 30004
}
```


</br>
Run the main software again and open the ArgosX setup screen. Check whether the setup values saved in the file are loaded normally.

![](../../_assets/image_46.png)

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

The InitButtonBar( ) function's return values

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

The scripts designated in the code above are for calling the setAllValueAsDef( ) function and the setSelectedValueAsDef( ) function, respectively. These functions are provided by dst_setup.js by default.

If you want other operations, you just need to implment the relevant functions yourself.

</br>
Let's test the operation. Run the virtual controller again and enter the ArgosX setup screen. After that, change the settings to values ​​different from the default values, then save them ​by clicking the [OK] button.

Enter the setup screen again and place the cursor on an arbitrary element. After that, click the Initialize One button and check whether the default values ​​are restored.

In addition, click the Initialize All button and check whether all values of the screen are restored to the default values.

![](../../_assets/image_48.png)
# 3.4 Practical project: Developing an ArgosX monitoring panel user interface

There should be a monitoring panel to show the current state of the plug-ins to the user through a split screen.

In this section, let's practice developing a web-based ArgosX monitoring panel.
<br></br>

* Specifications of the ArgosX monitoring panel user interface
* Layout of the monitoring panel
* Operating the monitoring panel
* Injecting a panel item into the panel menu 
# 3.4.1 Specifications of the ArgosX monitoring panel user interface

Let's create a monitoring panel user interface to monitor information, as shown below.

* IP address
* Port number
* Error input assigned number
* Request count
* Response count

The update cycle should be set to 500 msec.
<br></br>
![](../../_assets/image_49.png)

# 3.4.2 Layout of the monitoring panel

Open vscode for the apps/ folder that is the parent of the ArgosX folder.


Create ui/panel.html and panel.js files.

![](../../_assets/image_50.png)


Write the content as follows, which is about a simple layout consisting of one table. The first column of the table is given a class called 'thd' (abbreviation for table header), and this class is defined in the common style.css with black characters on a gray background. If you want to change the style, you can define a class using a separate local css and apply it.



panel.html
``` html
<!DOCTYPE html:5>
<!--
   @author: Jane Doe, BlueOcean Robot & Automation, Ltd.
   @brief: ArgosX Vision System interface - panel
   @create: 2021-12-07
-->
<html>
  
<head>
   <title>ArgosX Vision System</title>
   <meta http-equiv=Content-Type content='text/html; charset=utf-8'>
   <link rel='stylesheet' href='../../_common/css/style.css' type=text/css rel=stylesheet>
   <script src='../../_common/js/jquery-3.6.0.min.js'></script>
   <script src='./panel.js'></script>
   <script>
      $(document).ready(init);
   </script>
</head>
  
<body>
   <table>
      <th>name</th>
      <th>value</th>
      <tr>
         <td class='thd'>IP address</td>
         <td id='ip_addr'></td>
      </tr>
      <tr>
         <td class='thd'>port#</td>
         <td id='port'></td>
      </tr>
      <tr>
         <td class='thd'>sigcode for error</td>
         <td id='sigcode_err'></td>
      </tr>
      <tr>
         <td class='thd'>n.request</td>
         <td id='n_req'></td>
      </tr>
      <tr>
         <td class='thd'>n.response</td>
         <td id='n_res'></td>
      </tr>
   </table>
</body>
</html>
```

While panel.html is open, if you execute a Live server by clicking the Go Live button at the bottom right, the Google Chrome browser will open.

![](../../_assets/image_51.png)
<br></br>
Even though there is no content in panel.js yet, we can check whether the layout is normal.
![](../../_assets/image_52.png)




# 3.4.3 Operating the monitoring panel


The Python code already contains the variables for the IP address, port number, and error-assigned input number.

Because it is necessary to manage the request count and response count, we need to add the n_req and n_res variables, as shown below, and connect them to ensure that they can be returned together in the get_general( ) function.



setup.py
``` python 
Previous steps skipped...
 
 
def get_general() -> dict:
   """
   Returns:
      setting dict.
   """
 
   print('get_general()')
    
   ret = {}
   ret["ip_addr"] = ip_addr
   ret["port"] = port
   ret["sigcode_err"] = sigcode_err
   ret["n_req"] = n_req
   ret["n_res"] = n_res
    
   return ret
 
 
Skipped...
 
 
gen_def = get_general_def()
ip_addr : str = gen_def['ip_addr']
port : int = gen_def['port']
sigcode_err = gen_def['sigcode_err']
n_req = 0   # request count
n_res = 0   # response count
```

Add the count-up operations of n_req and n_res to the implementation of the robot language commands.



roblang.py
```python 
Previous steps skipped...
 
 
def req(work_no: int) -> int:
   """
   send request command to ArgosX
   e.g. "req 39"
   Args:
      work_no     work#    1~100
 
 
   Returns:
         >=0   the number of bytes sent
         -1    no socket. init() should be called.
   """
   msg = "req " + str(work_no)
   setup.n_req += 1                           # <---------- add
   return comm.send_msg(msg)
 
 
Skipped...
 
 
def _res_cont(addr_on_timeout: int_or_str) -> str:
   """res() implementation for cont-mode"""
   val = 0
   msg = ""
   timeout = _check_timeout_and_branch(addr_on_timeout)
   if timeout:
      val = 1
   else:
      msg = comm.recv_msg()
      print(msg)
      if msg=="res fail":
         val = 1
         msg = ""
      elif msg=="":
         xhost.req_to_continue()
      else:
         msg = get_base_shift_array_from_res(msg)
         setup.n_res += 1                    # <---------- add
   xhost.io_set_out_bit(setup.sigcode_err, val)
   return msg
```

Write the following content into the ui/panel.js file. The content is about calling the updateData( ) function every 500 msec, getting all the general data from the main board, and injecting them into the html screen through the display( ) function.



ui/panel.js
``` js

///@author: Jane Doe, BlueOcean Robot & Automation, Ltd.
///@brief: ArgosX Vision System interface - panel
///@create: 2021-12-07
 
 
 
function init()
{
   updateData();
   setInterval('updateData()', 500);
}
 
 
///@brief
function updateData()
{
   var path = '/apps/argosx/svr_general';
   var url = domainMb()+path;
   $.get(url, function(data) {
      display(data);
   });
}
 
 
///@return     e.g. "http://192.168.1.150:8888"
function domainMb()
{
   var domain = "http://" + window.location.hostname + ":8888";
   console.log(domain);
   return domain;
}
 
 
///@param[in]  data
///@brief      (callback function) form <- data
function display(data)
{
   console.log(data);
 
   $('#ip_addr').text(data.ip_addr);
   $('#port').text(data.port);
   $('#sigcode_err').text(data.sigcode_err);
   $('#n_req').text(data.n_req);
   $('#n_res').text(data.n_res);
}
```

Run the virtual controller again, then execute Go Live on vscode again. Now, you can see the value printed on the web browser's screen as follows.

Let's execute requests and responses by executing an ArgosX stub and running the job program with the virtual teach pendant. If the n.request and n.response values increase, it means the operation is normal.
<br></br>
![](../../_assets/image_53.png)





# 3.4.4 Injecting a panel item into the panel menu

Let's inject the ArgosX monitoring function into the panel menu.

Add a panel item into the ui/menu.json file as follows.

menu.json
``` json
[
   {
       "path": "system/appl/",
       "id": "argosx",
       "icon": "argosx/ui/lm_argosx.png",
       "label": "ArgosX Vision",
       "url": "argosx/ui/setup.html"
   },
   {
      "path": "panels",
      "id": "argosx",
      "icon": "argosx/ui/panel_argosx.png",
      "label": "ArgosX Vision",
      "url": "argosx/ui/panel.html"
  }
]
```

Let’s make a picture icon that will be injected into the panel menu. A png icon with a transparent 40 x 40-pixel background would be sufficient.

Here, we just made panel_argosx.png by scaling down the existing lm_argosx.png properly and adjusting the colors.


![](../../_assets/panel_argosx.png)
An example of panel_argosx.png (you can download and use this picture.)



Now, let's run the virtual mainboard and virtual teach pendant.


When you open the panel menu to add a new panel item, you can see a newly added ArogsX Vision menu item.
![](../../_assets/image_54.png)




When you select the menu, the monitoring panel we made will appear shortly.
![](../../_assets/image_55.png)




Let's execute requests and responses by operating the virtual teach pendant. If the n.request and n.response values increase, it means the operation is normal.

# 3.5 Practical project: Developing an ArgosX user bar UI

In the previous chapters, we learned how to perform the functions implemented in the plug-ins using a robot language.

However, in some cases, it may be necessary to provide a function for the user to directly operate the UI to call the functions of a plug-in. These operating buttons can be provided on the setup window or monitoring window. However, a user bar would be suitable for quick operations on the teaching screen.
<br></br>
In this chapter, let's practice developing a web-based ArgosX user bar UI.

* Specifications of the ArgosX user bar user interface
* Layout of the user bar
* Operating the user bar
* Injecting a user bar
# 3.5.1 Specifications of the ArgosX user bar user interface

Pressing the User Key button multiple times will switch the user bar.

Let’s create a user bar UI to provide a UI as follows.

- Light-on button: Turns ArgosX's LED light on.
- Light-off button: Turns ArgosX's LED light off.
<br>
<br>
![](../../_assets/image_56.png)

# 3.5.2 Layout of the user bar

Open vscode for the apps/ folder that is the parent of the ArgosX folder.

Create ui/ubar.html and ui/ubar.js files.
<br>![](../../_assets/image_57.png)

Write the content below, which is about a simple layout consisting of two buttons. The first column of the table is given a class called 'ubar-bt', which is defined in the common style.css. It automatically recognizes TP600 and TP630 and gives the button’s sizes and colors similar to the default UI. If you want to change the style, you can define a class using a separate local css and apply it.

ubar.html
``` html
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
    <script src='./ubar.js'></script>
</head>
  
<body class='ubar'>
   <button id='light-on' class='ubar-bt' onclick='light_onoff(true);'>light<br>on</button>
   <button id='light-off' class='ubar-bt' onclick='light_onoff(false);'>light<br>off</button>
</body>
</html>
```

While ubar.html is open, if you execute Live server by clicking the Go Live button at the bottom right, the Google Chrome browser will open.
<br>![](../../_assets/image_58.png)




Even though there is no content in ubar.js yet, we can check whether the layout is normal.
<br>![](../../_assets/image_59.png)

# 3.5.3 Operating the user bar

## Client side (teach pendant)


Write the following content in the ui/ubar.js file. Every time the button is pressed, the teach pendant will transmit an HTTP post message called "light_onoff" to the main board. Whether it is on or off will be contained in an object that has the onoff attribute, and it will be loaded in the body of the post message and sent. 

- Body in the light-on state: { onoff: true }
- Body in the light-off sate: { onoff: false }


ui/ubar.js
``` js
///@author: Jane Doe, BlueOcean Robot & Automation, Ltd.
///@brief: ArgosX Vision System interface - setup general
///@create: 2021-12-07
 
 
 
///@return     e.g. "http://192.168.1.150:8888"
function domainMb()
{
   var domain = "http://" + window.location.hostname + ":8888";
   console.log(domain);
   return domain;
}
 
 
///@param[in]  onoff    true or false
///@return
///      -  0     ok
///@brief      request light-on/off
function light_onoff(onoff)
{
   var url = domainMb()+"/apps/argosx/svr_light_onoff";
   var args = { onoff: onoff };
 
   args = JSON.stringify(args);
   $.ajax({
      url: url,
      type: 'post',
      dataType: 'json',
      contentType : "application/json; charset=utf-8",
      data: args,
      success: function(res) {
         console.log('success' + res);
      },
      error : function(res) {
         console.log('error' + res);
      }
   });
 
   return 0;
}
```



## Server side (mainboard)


Now, the ArgosX plug-in of the mainboard needs to receive this message and send the "light-on" and "light-off" messages to the actual ArgosX device (testing with a stub).



Write the ubar.py file into the argosx/ folder as follows. Regarding its implementation, the comm_ex module will be utilized similarly to how we performed the callback implementations in a previous chapter.
``` python
""" ArgosX Vision System interface - main
 
 
@author:    Jane Doe, BlueOcean Robot & Automation, Ltd.
@created:   2021-12-07
"""
 
 
from . import comm_ex
 
 
def post_light_onoff(body: dict) -> int:
   """
   Args:
      onoff
    
   Returns:
      0
   """
   onoff = body['onoff']
   msg = 'light-' + ('on' if onoff else 'off')
   print(msg)
       
   return comm_ex.send_msg_once(msg)
```

As the final step, import all functions of ubar.py to main.py.



main.py
``` python
""" ArgosX Vision System interface - main
 
 
@author:    Jane Doe, BlueOcean Robot & Automation, Ltd.
@created:   2021-12-06
"""
 
from . import setup
from .roblang import *
from .setup import *
from .callback import *
from .ubar import *
 
import xhost
 
 
Subsequent steps skipped...
```

Reboot the virtual controller, then run the job file up to argosx.init( ). Execute Go Live again to bring up the user bar screen on the web browser.

Execute an ArgosX stub then operate the buttons on the web browser. If the stub’s output on the console is displayed as “light-on" and "light-off", it means the operation is normal.
<br>
![](../../_assets/image_60.png)




ArgosX stub's ouput on the console
<br>
![](../../_assets/image_61.png)






# 3.5.4 Injecting a user bar

Let's inject the ArgosX user bar into the actual teach pendant.



Add the "ubars" path item into the ui/menu.json file, as shown below. 




menu.json
``` json
[
    {
         "path": "system/appl/",
         "id": "argosx",
         "icon": "argosx/ui/lm_argosx.png",
         "label": "ArgosX Vision",
         "url": "argosx/ui/setup.html"
    },
    {
        "path": "panels",
        "id": "argosx",
        "icon": "argosx/ui/panel_argosx.png",
        "label": "ArgosX Vision",
        "url": "argosx/ui/panel.html"
    },
    {
        "path": "ubars",
        "id": "argosx",
        "url": "argosx/ui/ubar.html"
    }
]
```

Pressing the User Key button on the right side of the virtual teach pendant will bring up the user bars of the installed applications in sequence. Now you can also see the user bar we created for ArgosX. Operate the buttons to check whether the ArgosX stub responds normally.

![](../../_assets/image_62.png)


# 4. Debugging

* Debugging Python code
* Debugging a web-based UI
# 4.1 Debugging Python code

If the Python code we wrote has grammatical errors in the web UI or logical errors in the implementation body, a debugger should be used to trace the cause and supplement it.

However, the Python source code of the plug-ins is called from the Hi6 host through the Import operation, callback operation, or by a robot language. As there is no built-in Python debugger in the Hi6 host, tracing by placing a breakpoint to a call from the host is impossible. However, you can perform partial debugging by executing the entry function of a plug-in with an external debugger.


<br>

## xhost for debugging
xhost is a module to be used by plug-ins to call the functions of the Hi6 host. It is created and injected into the plug-in by the host. The figure below shows a typical operational flow between the host and a plug-in.
<br> ![](../_assets/image_63.png)




However, when a plug-in is executed with the vscode debugger, an execution error will occur when the xhost method is called because there is no actual xhost module created/injected. Therefore, an alternative xhost for debugging should be used. In the common folder of the SDK, there is an alternative module, xhost_dbg. It is a kind of proxy that performs the host’s operations by calling an OpenAPI via Ethernet.
<br> ![](../_assets/image_64.png)
<br>





What you see below is a simple xhost.py in the apps/ folder that imports xhost_dbg. When the actual Hi6 host creates/injects xhost, it will override this xhost alternative module.



xhost.py
``` python
""" xhost for debug
   This module will be overridden by host.
"""
from _common.py.xhost_dbg import *
```

The host can be a Hi6 virtual controller running in a PC development environment or an actual controller. You need to designate where xhost_dbg should access.

There is xhost_remote_ip.py file in the apps/ folder of the Hi6 home path. The default value is set as the PC development environment itself (namely, the Hi6 virtual controller) as follows.



xhost_remote_ip.py
``` python
remote_ip="127.0.0.1"
```

If you want to run tests by accessing the actual Hi6 controller with an IP address of "192.168.1.150", you can set it as follows.



xhost_remote_ip.py
``` python
remote_ip="192.168.1.150"
```



## Debugging with Visual Studio Code and test.py
If Microsoft Python extensions are properly installed in<u> 1.5 Installing the Visual Studio Code</u>, the Python debugging environment can be used. If you are already familiar with debugging Python in vscode, you can skip this section.



For tracing the functions to be called by a robot language or a callback operation, we defined the test module under the argosx/ folder, as shown in the figure below. Implement the routine that needs to be tested, then call the test( ) function from the main routine.  

You can toggle the breakpoint by clicking the left side of the line number.



test.py
<br>
![](../_assets/image_65.png)



When you press the F5 key in the test.py window or click the Run - Start Debugging menu, the execution will occur in Python runtime and in debugging mode.
<br>
![](../_assets/image_66.png)





For the ArgosX plug-ins in a previous section, you should execute the argosx_stub for the purpose of testing. If you need to run two Python programs, like in this case, you need to open two vscode sessions and run the debugger in each, as shown below.
<br>
![](../_assets/image_67.png)





When a breakpoint is placed, the VARIABLES, WATCH, CALL STACK, and BREAKPOINTS windows will open on the left. You can perform tracing with the operating buttons on the top right: Continue (F5), Step Over (F10), Step Into (F11), and Step Out (Shift+F11). If you click the Restart (Ctrl+Shift+F5) button, the execution will restart from the beginning. If you click the Stop (Shift+F5) button, debugging will stop.

When the print( ) command, a built-in Python function, is called, a string will be printed in the TERMINAL window at the bottom, and it can be used for debugging.
<br>
![](../_assets/image_68.png)





Operating test.py is interlocked with the controller host by xhost_dbg, so debugging is possible while checking the actual operation as follows. 
<br>
![](../_assets/image_69.png)




# 4.2 Debugging a web-based UI


Before testing a web UI on the teach pendant, we could pretest the screen with the Google Chrome web browser.

If the web UI we have written has a grammatical error in JavaScript or a logical error in the implementation body, a debugger should be used to trace the cause and supplement it. Moreover, there is a built-in debugger called Chrome Development Tools (abbreviated as Chrome DevTools) in the Google Chrome web browser, so you can use it. In this section, we will learn how to debug a web UI. If you are already familiar with using Chrome DevTools, you can skip this section.





## Executing a web UI with Live server


In the <u>Layout of the setup screen</u> section, we have practiced executing the setup.html of the ArgosX plug-ins with the Google Chrome web browser before.

Let's execute it again.



The ArgosX of the virtual controller should be in its normally imported state.

While setup.html is opened in vscode, you need to click the Go Live button on the bottom right to run the Live server.

![](../_assets/image_70.png)




Alternatively, you can open a pop-up menu by right-clicking the mouse on setup.html, then select "Open with Live Server".

![](../_assets/image_71.png)



As you can see below, setup.html is opened with the Google Chrome web browser.

![](../_assets/image_72.png)






## Chrome DevTools


When you press the F12 button, Chrome DevTools will open on the right side of the browser. In the picture below, Console, a menu at the top of the DevTools is selected. When a string is called with console.log() in JavaScript, the string will be printed on this console window. During this process, the source code location where log( ) was called is also displayed, allowing you to click to move to the source code location, which makes the debugger useful for debugging.

![](../_assets/image_73.png)



Selecting Elements from the menu at the top of the DevTools enables you to see the hierarchy of an html file. When you select a specific element in the html file, the relevant element will be highlighted on the left rendering screen, while the CSS style will be displayed on the bottom right.


![](../_assets/image_74.png)



Selecting the Sources menu will make JavaScript source code files appear in a tree-like structure. You can select and open the desired file and check the source code. You can also toggle the breakpoint by clicking the line number on the left.

![](../_assets/image_75.png)



Clicking the Update button on the html rendering screen on the left or pressing the F5 key will update the web UI and make JavaScript run from the beginning. You can see that the execution cursor stopped at the breakpoint. You can also see the Breakpoints and Call Stack windows at the bottom of the picture, and you can move to a source code's location by clicking the relevant item.

The Scope menu at the bottom right shows local variables, and if you select the Watch menu on the right side of the Scope menu, you can add and observe the desired variables.

This environment will be a very familiar debugging environment for the users of integrated development environments, such as Visual Studio or Eclipse.



![](../_assets/image_76.png)

![](../_assets/image_77.png)




Tracing can be performed when a breakpoint is placed. You can perform the Resume (F8), Step over (F10), Step into (F11), and Step out (Shift+F11) operations with the operating button group above the Breakpoints window.


![](../_assets/image_78.png)


## Modifying and re-executing source code


After modifying the source codes of html, CSS, and JavaScript, if you click the Update button in the web browser or press the F5 key, the execution will occur based on the modified content.

Moreover, because web browsers have caches, the modifications may not be reflected even if the source code is modified and re-executed. In this case, you can solve the problem by right-clicking the mouse on the Update button and opening a pop-up menu, then selecting the 'Clear Cash and Hard Refresh' menu (This pop-up menu is only available when DevTools is open).


![](../_assets/image_79.png)


Refreshing can also be performed from a virtual teach pendant rather than on a web browser. Right-clicking the mouse on the web U/I screen will open a pop-up window. If you select the Reload menu here, the modifications made in the source code will be reflected immediately.


![](../_assets/image_80.png)




# 5. Installer
