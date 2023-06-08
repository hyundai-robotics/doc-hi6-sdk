# 2.3 Transferring parameters to a Python function and receiving a return value


Now, let's apply two parameters, name and age, to a Python function. Add the introduce( ) function by modifying the code as follows.

```python
import xhost
 
def hello():
    print("Hello, world!")
    xhost.printh("Hello, world!")
 
 
def introduce(name: str, age: int) -> str:
    msg = f"Hello, {name}! You are {age} years old."
    return msg
```

- :str, :int, and -> str in the introduce functions are elements of grammar called type hint, introduced with Python 3.5. Omitting type hint will not affect the operation. Even though type hint does not play any role while executing scripts, it provides information for vscode extensions, such as Pyright, to check its grammar. Therefore, type hint helps prevent grammar errors when coding in Python (https://docs.python.org/3.8/library/typing.html.)


In a job program, you need to teach it additionally to call introduce( ).

```
import hello_world
hello_world.hello()
var msg=hello_world.introduce("Steve", 25)
print msg
end
```

If the following string is printed on the guidance frame of the teach pendant, it means the operation is normal.
![](../_assets/image_18.png)

As seen in the example, the string values and integer values of HRScript are naturally transferred as the string values and integer values of Python. Conversely, the string return values of Python are also naturally returned as the string values of HRScript. The values of both languages are automatically converted mutually like this. The table below is a data type map of the two languages.

<table>
  <thead>
    <tr>
      <th style="text-align:left">HRScript</th>
      <th style="text-align:left">Python</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>bool</td>
      <td>
       bool
      </td>
    </tr>
   <tr>
      <td>number - integer</td>
      <td>
       long 
      </td>
    </tr>
    <tr>
      <td>number - real</td>
      <td>
       float
      </td>
    </tr>
    <tr>
      <td>string</td>
      <td>
       str
      </td>
    </tr>
    <tr>
      <td>array</td>
      <td>tuple</td>
    </tr>
    <tr>
      <td>object</td>
      <td>
       dictionary	
      </td>
    </tr>
     <tr>
      <td>xpy-object</td>
      <td>object</td>
    </tr>
  </tbody>
</table>

-  When an object created in Python is transferred to HRScript, it is specifically called as xpy-object. When it comes to xpy-objects, it is possible to read attribute values and call methods. We will cover them in more detail later.
- For Python grammar, multiple return values can be transferred from a function to the outside, but they cannot be transferred to HRScript.
