# 3.5.3 Operating the user bar

## Client side (teach pendant)



Write the following content in the ui/ubar.js file. Every time the button is pressed, the teach pendant will transmit an HTTP post message called "light_onoff" to the main board. Whether it is on or off will be contained in an object that has the onoff attribute, and it will be loaded in the body of the post message and sent. 

- Body in light-on state: { onoff: true }
- Body in light-off sate: { onoff: false }


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




ArgosX stub's output on the console
<br>
![](../../_assets/image_61.png)






