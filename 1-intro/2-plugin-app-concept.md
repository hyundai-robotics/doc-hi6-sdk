# 1.2 Concept of Hi6 plug-in apps
An Hi6 app consists of Python 3 scripts that operate in the main module and JavaScript-based web software that execute UI operations in the teach pendant.

For apps without UIs, they may consist of only Python scripts.

Multiple Python files and web app files will be installed under one folder in the main module.

## Python scripts
The scripts can be called through specific robot controller events or periodically. In addition, the designated Python functions can be called using the robot language commands in the .job file.

Through a module called xhost, the robot controller's software objects can be controlled or monitored, and xhost interacts with software objects through a Python interface or OpenAPI.

## Teach pendant UI web apps
This web app is identical to normal web apps used in a PC or mobile environment. It consists of HyperText Markup Language (HTML)/Cascading Style Sheets (CSS)/JavaScript and resource files, such as various images.

This web app is stored in the main module, which will be transferred to the teach pendant to be executed on a web browser engine.

By calling the OpenAPI, the teach pendant web app can call the Python functions and send and receive data.

![](../_assets/image_1.png)