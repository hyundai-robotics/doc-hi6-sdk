# 3.3.1 Specifications of the ArgosX setup screen user interface

Let's create the setup screen user interface with the following specifications.

* In the Setup - Application Parameter menu, there is a menu to enter the ArgosX setup screen.
* On the ArgosX setup screen, you can set the IP address and port number of the ArgosX system
* The content of the setup will be saved into the argosx.json file as a json file.
* An F button (Initialize All) that sets the entire screen to the default values is provided.
* An F button (Initialize One) that sets only the currently selected items to default values is provided.
<br></br>

![](../../_assets/image_34.png)





Regardless of whether ArgosX is imported to the job program, we want to make opening the setup screen and checking/changing the setup values possible.

Change the startup item in the argosx/info.json file from "manual" to "boot". Now, as ArgosX will be imported while the controller is booting, you do not need to import ArgosX to the job program.

(You can still carry out the setups in HRScript.)