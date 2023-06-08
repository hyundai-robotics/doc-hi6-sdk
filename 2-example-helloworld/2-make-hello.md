# 2.2 Implementing the python function hello( )

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
<br></br>

Now, execute the Hi6 virtual controller, which is the main module, and the TP in order.

When starting, the Hi6 controller recognizes the installed apps by reading the info.json from all folders under the apps/ folder.

[service] - 10: Clicking the app will bring up a screen called 10: app - TP. 

Clicking the [location] button twice will change the TP in the title to MAIN via USB. On this screen, hello_world created earlier can be found.

![](../_assets/image_17.png)

We will execute hello_world in the robot language, so press the ESC key to exit the screen.

Now, create a job program using HRScript. Carry out the teaching process as follows.

```
import hello_world
hello_world.hello()
end
```

While leaving the previous screen pane open and turning on the motor, if you perform an execution with the Step FWD or START button, Hello, World! will be printed.

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