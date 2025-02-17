# 1.6 Testing plugins in HRSpace

You can test your own plugin app on HRSpace's virtual controller and virtual teaching pendant.

{% hint style="warning" %}   
<b>There are differences from the actual controller environment.</b> Please use it only for simple functional tests during the development stage, and ensure thorough testing in a real environment before applying the plugin. We are not responsible for any damages or issues arising from failure to consider this information.  
{% endhint %}  

<br>

## 1.6.1 HRSpace Installation Environment
- Operating System: Windows 64-bit

<br>

## 1.6.2 HRSpace Installation Process

1. Access the HD Hyundai Robotics website and sign up if not already registered.
2. Open the HRSpace download page.
3. Install the latest version (as of the document date: v3.95b10).
4. Extract the downloaded zip file.
5. Run the installer (HRSpace3.msi) → Select language → Choose installation location → Complete installation and exit.

<br>

## 1.6.3 Running HRSpace

### a. Load the robot model
1. Press the `Windows key` > Type `HRSpace3_eng` > Click > Run the program.
2. Right-click on the `workspace component` in the left Workspace panel > Click `Load Model as a Child...` > Click `Robot` folder > Select the desired model.