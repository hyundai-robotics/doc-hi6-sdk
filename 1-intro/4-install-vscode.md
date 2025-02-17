# 1.5 Installing Visual Studio Code

Microsoft Visual Studio Code (hereafter referred to as vscode) is a powerful text editor that is available for free. Through the installation of various extensions, vscode provides a development environment for numerous programming languages.

You may use other familiar editors, such as Atom or SublimeText, but this manual provides explanations based on vscode.



## Installing the code
1) Access the link below then download the stable build version for Windows. 

    For Windows: https://code.visualstudio.com/<br>
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
