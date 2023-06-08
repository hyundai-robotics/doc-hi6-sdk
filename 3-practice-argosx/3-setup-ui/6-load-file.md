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

