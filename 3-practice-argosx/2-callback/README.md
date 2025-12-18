# 3.2 Practical project: ArgosX - callback


While operating the ${cont_model} controller, there are main events, such as Mode Change, Motor On, Reset, Start and Accuracy OK. We can register functions into a plug-in for it to perform unique operations for an event.

These functions are callback functions. They are referred to as such because they are called by other parts (${cont_model} host) and not actively called within the Python code.



* Registering a callback function
* Implementing a callback function
* Manual for referring to the callback functions