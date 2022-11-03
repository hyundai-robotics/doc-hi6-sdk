# 2.3 python 함수에 매개변수 전달하고 리턴값 전달받기


이제 python 함수에 name과 age라는 2개의 매개변수를 적용해봅시다. 아래와 같이 코드를 수정하여 introduce( ) 함수를 추가합니다.

```python
import xhost
 
def hello():
    print("Hello, world!")
    xhost.printh("Hello, world!")
 
 
def introduce(name: str, age: int) -> str:
    msg = f"Hello, {name}! You are {age} years old."
    return msg
```

- introduce 함수의 :str, :int, -> str 은 python 3.5부터 도입된 type hint라는 문법입니다. type hint는 생략해도 동작에는 지장이 없습니다. 스크립트 실행 시에는 아무 역할을 안 하지만, Pyright 같은 vscode extension이 문법검사를 할 수 있도록 정보를 주기 때문에, python 코딩 시 문법 에러를 범하지 않도록 도움을 줍니다. (https://docs.python.org/3.8/library/typing.html)


job 프로그램에는 introduce( )의 호출을 추가로 교시합니다.

```
import hello_world
hello_world.hello()
var msg=hello_world.introduce("Steve", 25)
print msg
end
```

티치펜던트의 안내프레임에 아래와 같은 문자열이 출력되면 정상 동작한 것입니다.
![](../_assets/image_18.png)

예제에서 본 바와 같이 HRScript의 문자열 값, 정수 값은 python의 문자열 값, 정수 값으로 자연스럽게 전달되며, 반대로 python의 문자열 리턴값도 HRScript의 문자열 값으로 자연스럽게 리턴됩니다. 두 언어의 값은 이와 같이 자동으로 상호 변환됩니다.  아래 표는 두 언어간 data type 맵입니다.

<table>
  <thead>
    <tr>
      <th style="text-align:left">HRScript</th>
      <th style="text-align:left">python</th>
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
      <td>숫자 - 정수(intrger)</td>
      <td>
       long 
      </td>
    </tr>
    <tr>
      <td>숫자 - 실수(real)</td>
      <td>
       float
      </td>
    </tr>
    <tr>
      <td>문자열</td>
      <td>
       str
      </td>
    </tr>
    <tr>
      <td>배열</td>
      <td>tuple</td>
    </tr>
    <tr>
      <td>객체</td>
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

-  python에서 생성한 객체를 HRScript가 전달받은 경우, 이를 특별히 xpy-object라고 지칭합니다. xpy-객체에 대해서는 속성값을 읽거나 method를 호출할 수 있는데, 뒤에서 자세히 알아보도록 하겠습니다.
- python 문법은 함수에서 복수 개의 리턴값을 외부로 전달할 수 있으나, HRScript로 전달받을 수는 없습니다.
