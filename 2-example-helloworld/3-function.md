# 2.3 将参数传递给 Python 函数并接收返回值

现在，让我们将两个参数，name 和 age，应用到一个 Python 函数中。通过以下修改代码添加 introduce( ) 函数。

```python
import xhost
 
def hello():
    print("Hello, world!")
    xhost.printh("Hello, world!")
 
 
def introduce(name: str, age: int) -> str:
    msg = f"你好，{name}! 你 {age} 岁了。"
    return msg
```

- :str, :int 和 -> str 在 introduce 函数中是称为类型提示的语法元素，自 Python 3.5 以来引入。省略类型提示不会影响操作。尽管类型提示在执行脚本时不发挥任何作用，它为 vscode 扩展（例如 Pyright）提供信息，以检查其语法。因此，类型提示有助于在 Python 编码时防止语法错误 (https://docs.python.org/3.8/library/typing.html。)

在工作程序中，您需要额外教它调用 introduce( )。

```
import hello_world
hello_world.hello()
var msg=hello_world.introduce("Steve", 25)
print msg
end
```

如果在教学挂件的指导框架上打印以下字符串，则表示操作正常。
![](../_assets/image_18.png)

如示例所示，HRScript 的字符串值和整数值自然地作为 Python 的字符串值和整数值传递。相反，Python 的字符串返回值也自然地作为 HRScript 的字符串值返回。两种语言的值以这种方式相互自动转换。下表是两种语言的数据类型映射。

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
      <td>数字 - 整数</td>
      <td>
       长整数
      </td>
    </tr>
    <tr>
      <td>数字 - 实数</td>
      <td>
       浮点数
      </td>
    </tr>
    <tr>
      <td>字符串</td>
      <td>
       字符串
      </td>
    </tr>
    <tr>
      <td>数组</td>
      <td>元组</td>
    </tr>
    <tr>
      <td>对象</td>
      <td>
       字典	
      </td>
    </tr>
     <tr>
      <td>xpy-对象</td>
      <td>对象</td>
    </tr>
  </tbody>
</table>

- 当在 Python 中创建的对象转移到 HRScript 时，特别称为 xpy-对象。关于 xpy-对象，可以读取属性值和调用方法。我们会在后面详细讨论。
- 对于 Python 语法，多个返回值可以从函数转移到外部，但不能转移到 HRScript。