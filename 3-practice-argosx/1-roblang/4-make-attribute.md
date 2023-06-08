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
print argosx.ip_addr # re-checking
end
```

Check whether the value newly assigned to ip_addr is printed on the guidance frame.
```
192.168.1.172
```
