# 3.2.2 Implementing a callback function
Because we ensured that the callback functions are called well, let's implement the actual operations.

As you can see by checking <U>3.1.1 Specifications of ArgosX and interface plug-ins</U>, you just need to send the "light-on" and "light-off" messages to the ArgosX hardware.



Because the comm module already has a function implemented to send an Ethernet string, you only need to call one as follows.
```
comm.send_msg("light-on")
comm.send_msg("light-off")
```
<br></br>

However, there is one problem with this implementation. If the comm.open( ) function is called through argosx.init, a robot language command, the string will be transmitted normally. However, if the function is not called, the string will not be transmitted.

In addition, transmissions will not occur even when communications are closed because of comm.close( ).

Therefore, it is necessary to define a string transmission function that makes it possible to open communications, if it is in closed state, and carry out transmissions and close communications.

<br></br>
Create a comm_ex.py file, as shown below, in the same folder where comm.py exists.
<br></br>


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


<br></br>
<U>__argosx_stub side (server playing the role of ArgosX)__</U>

Every time the motor OFF and motor ON functions occur, the following strings will be printed on the console.
```
request : light-off
LED light is OFF

request : light-on
LED light is ON
```
