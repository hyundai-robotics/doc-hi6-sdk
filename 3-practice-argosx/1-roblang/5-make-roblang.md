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
print argosx.ip_addr # re-checking
 
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
