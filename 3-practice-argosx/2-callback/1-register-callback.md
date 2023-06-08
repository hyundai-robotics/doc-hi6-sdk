# 3.2.1 Registering a callback function


The method of registering a callback function is very simple. The callback function name is determined already for each event, so, by defining a callback function in the plug-in code using the relevant callback function name, the callback function will be registered automatically when the plug-in is imported.


We need to refer to the other functions of <U>3.1.1 Specifications of ArgosX and interface plug-ins</U> again.

Depending on whether the robot is in the motor ON or motor OFF state, ArgosX's LED light should also be switched on or off accordingly. Specifically, the commands "light-on" or "light-off" should be transmitted to ArgosX.



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
