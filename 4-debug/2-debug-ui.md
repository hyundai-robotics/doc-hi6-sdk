# 4.2 Debugging a web-based UI


Before testing a web UI on the teach pendant, we could pretest the screen with the Google Chrome web browser.

If the web UI we have written has a grammatical error in JavaScript or a logical error in the implementation body, a debugger should be used to trace the cause and supplement it. Moreover, there is a built-in debugger called Chrome Development Tools (abbreviated as Chrome DevTools) in the Google Chrome web browser, so you can use it. In this section, we will learn how to debug a web UI. If you are already familiar with using Chrome DevTools, you can skip this section.





## Executing a web UI with Live server


In the <u>Layout of the setup screen</u> section, we have practiced executing the setup.html of the ArgosX plug-ins with the Google Chrome web browser before.

Let's execute it again.



The ArgosX of the virtual controller should be in its normally imported state.

While setup.html is opened in vscode, you need to click the Go Live button on the bottom right to run the Live server.

![](../_assets/image_70.png)




Alternatively, you can open a pop-up menu by right-clicking the mouse on setup.html, then select "Open with Live Server".

![](../_assets/image_71.png)



 As you can see below, setup.html is opened with the Chrome web browser.

![](../_assets/image_72.png)






## Chrome DevTools


When you press the F12 button, Chrome DevTools will open on the right side of the browser. In the picture below, Console, a menu at the top of the DevTools is selected. When a string is called with console.log() in JavaScript, the string will be printed on this console window. During this process, the source code location where log( ) was called is also displayed, allowing you to click to move to the source code location, which makes the debugger useful for debugging.

![](../_assets/image_73.png)



Selecting Elements from the menu at the top of the DevTools enables you to see the hierarchy of an html file. When you select a specific element in the html file, the relevant element will be highlighted on the left rendering screen, while the CSS style will be displayed on the bottom right.


![](../_assets/image_74.png)



Selecting the Sources menu will make JavaScript source code files appear in a tree-like structure. You can select and open the desired file and check the source code. You can also toggle the breakpoint by clicking the line number on the left.

![](../_assets/image_75.png)



Clicking the Update button on the html rendering screen on the left or pressing the F5 key will update the web UI and make JavaScript run from the beginning. You can see that the execution cursor stopped at the breakpoint. You can also see the Breakpoints and Call Stack windows at the bottom of the picture, and you can move to a source code's location by clicking the relevant item.

The Scope menu at the bottom right shows local variables, and if you select the Watch menu on the right side of the Scope menu, you can add and observe the desired variables.

This environment will be a very familiar debugging environment for the users of integrated development environments, such as Visual Studio or Eclipse.



![](../_assets/image_76.png)

![](../_assets/image_77.png)




Tracing can be performed when a breakpoint is placed. You can perform the Resume (F8), Step over (F10), Step into (F11), and Step out (Shift+F11) operations with the operating button group above the Breakpoints window.


![](../_assets/image_78.png)


## Modifying and re-executing source code


After modifying the source codes of html, CSS, and JavaScript, if you click the Update button in the web browser or press the F5 key, the execution will occur based on the modified content.

Moreover, because web browsers have caches, the modifications may not be reflected even if the source code is modified and re-executed. In this case, you can solve the problem by right-clicking the mouse on the Update button and opening a pop-up menu, then selecting the 'Clear Cash and Hard Refresh' menu (This pop-up menu is only available when DevTools is open).


![](../_assets/image_79.png)


Refreshing can also be performed from a virtual teach pendant rather than on a web browser. Right-clicking the mouse on the web U/I screen will open a pop-up window. If you select the Reload menu here, the modifications made in the source code will be reflected immediately.


![](../_assets/image_80.png)




