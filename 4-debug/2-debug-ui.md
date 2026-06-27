# 4.2 调试基于Web的UI

在教学挂件上测试Web UI之前，我们可以先使用Google Chrome浏览器预测试屏幕。

如果我们编写的Web UI在JavaScript中存在语法错误或实现主体中的逻辑错误，则应使用调试器跟踪原因并进行补充。此外，Google Chrome浏览器中有一个名为Chrome开发工具（简称Chrome DevTools）的内置调试器，因此您可以使用它。在本节中，我们将学习如何调试Web UI。如果您已经熟悉使用Chrome DevTools，可以跳过本节。

#### 使用实时服务器执行Web UI

在<u>设置屏幕的布局</u>部分，我们之前已经练习过使用Google Chrome浏览器执行ArgosX插件的setup.html。

让我们再次执行它。

虚拟控制器的ArgosX应该处于正常导入状态。

在vscode中打开setup.html时，您需要单击右下角的Go Live按钮以运行实时服务器。

![](../_assets/image_70.png)

或者，您可以通过右键单击setup.html打开弹出菜单，然后选择“使用实时服务器打开”。

![](../_assets/image_71.png)

如您所见，setup.html已在Chrome浏览器中打开。

![](../_assets/image_72.png)

#### Chrome DevTools

当您按下F12按钮时，Chrome DevTools将在浏览器的右侧打开。在下图中，选择了DevTools顶部的Console菜单。当在JavaScript中使用console.log()调用字符串时，该字符串将打印在此控制台窗口中。在此过程中，调用log( )的源代码位置也会显示出来，您可以单击以跳转到源代码位置，这使调试器在调试时非常有用。

![](../_assets/image_73.png)

从DevTools顶部的菜单中选择Elements可以查看html文件的层次结构。当您在html文件中选择特定元素时，相关元素将在左侧渲染屏幕上高亮显示，而CSS样式将显示在右下角。

![](../_assets/image_74.png)

选择Sources菜单将使JavaScript源代码文件以树状结构出现。您可以选择并打开所需的文件并检查源代码。您还可以通过单击左侧的行号切换断点。

![](../_assets/image_75.png)

单击左侧html渲染屏幕上的Update按钮或按F5键将更新Web UI，并使JavaScript从头开始运行。您可以看到执行游标停在断点处。您还可以在图底部看到Breakpoints和Call Stack窗口，并且可以通过单击相关项目跳转到源代码位置。

右下角的Scope菜单显示局部变量，如果您在Scope菜单右侧选择Watch菜单，您可以添加和观察所需的变量。

对于集成开发环境的用户，如Visual Studio或Eclipse，这个环境将是一个非常熟悉的调试环境。

![](../_assets/image_76.png)

![](../_assets/image_77.png)

当放置断点时，可以执行跟踪。您可以使用Breakpoints窗口上方的操作按钮组执行Resume (F8)、Step over (F10)、Step into (F11) 和 Step out (Shift+F11)操作。

![](../_assets/image_78.png)

#### 修改和重新执行源代码

在修改了html、CSS和JavaScript的源代码后，如果您在Web浏览器中单击Update按钮或按F5键，则将基于修改的内容执行。

此外，由于Web浏览器具有缓存，即使源代码被修改并重新执行，修改也可能未反映。在这种情况下，您可以通过右键单击Update按钮打开弹出菜单，然后选择“清除缓存和强制刷新”菜单来解决问题（此弹出菜单仅在DevTools打开时可用）。

![](../_assets/image_79.png)

刷新也可以在虚拟教学挂件中进行，而不是在Web浏览器中。右键单击Web U/I屏幕将打开弹出窗口。如果您在这里选择Reload菜单，则在源代码中进行的修改将立即反映出来。

![](../_assets/image_80.png)