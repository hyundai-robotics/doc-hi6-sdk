# 2.3 将参数传递给 Python 函数并接收返回值

现在，让我们将两个参数，姓名和年龄，应用于 Python 函数。通过如下修改代码添加 introduce( ) 函数。

```python
import xhost
 
def hello():
    print("Hello, world!")
    xhost.printh("Hello, world!")
 
 
def introduce(name: str, age: int) -> str:
    msg = f"Hello, {name}! You are {age} years old."
    return msg
```

- :str, :int 和 -> str 在 introduce 函数中是称为类型提示的语法元素，从 Python 3.5 开始引入。省略类型提示不会影响操作。虽然类型提示在执行脚本时并没有角色，但它为 vscode 扩展（例如 Pyright）提供了检查其语法的信息。因此，类型提示有助于在 Python 编码时防止语法错误 (https://docs.python.org/3.8/library/typing.html.)

在一个工作程序中，您需要额外教它调用 introduce( )。

```
import hello_world
hello_world.hello()
var msg=hello_world.introduce("Steve", 25)
print msg
end
```

如果以下字符串在教学挂件的指导框中打印出来，则意味着操作正常。  
![](../_assets/image_18.png)

如示例所示，HRScript 的字符串值和整数值自然地作为 Python 的字符串值和整数值传递。相反，Python 的字符串返回值也自然返回为 HRScript 的字符串值。两种语言的值以这种方式自动相互转换。下面的表格是两种语言的数据类型映射。

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

- 当一个在 Python 中创建的对象传递到 HRScript 时，被特别称为 xpy-object。对于 xpy-object，可以读取属性值并调用方法。我们将在后面更详细地讨论它们。
- 对于 Python 语法，可以从函数传递多个返回值到外部，但不能传递到 HRScript。