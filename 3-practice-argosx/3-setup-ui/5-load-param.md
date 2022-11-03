# 3.3.5 설정화면의 값 불러오기와 저장하기



설정화면을 열면, python 플러그인 내에 저장되어 있는 설정값들을 javascript의 중간 객체로 불러온 후, 화면의 HTML element로 표시합니다.

사용자가 값을 수정한 후 [OK] 버튼을 클릭하면, 화면의 HTML element가 javascript의 중간 객체로 만들어진 후, python 플러그인 내의 변수에 저장됩니다.



이러한 전달을 수행하기 위해, javascript의 updateData( ) 함수와 python의 setup 모듈의 getter, putter 함수를 구현해야 합니다.
</br>![](../../_assets/image_44.png)</br>






## element ↔ javascript object


setup.js
``` js
이전 생략...
 
 
///@param[in]  data
///@param[in]  to_data     true; element->data, false; element<-data
function updateData(data, to_data)
{
   ddx_edit_ip(data, 'ip_addr', to_data);
   ddx_edit_i(data, 'port', to_data);
   ddx_edit_sig(data, 'sigcode_err', to_data);
}
```


element와 javascript 객체간 값의 양방향 전달을 updateData( ) 함수로 정의했습니다. 매개변수 data는 javascript 객체입니다. to_data는 전달방향을 나타내는 boolean 변수인데, true이면 element → data 방향, false이면 element ← data 방향입니다.

이러한 전달은 DOM API 혹은 jquery를 활용하여 직접 구현해도 됩니다. 하지만, dst_setup.js가 제공하는 ddx (dynamic data exchange) 함수들을 사용하면 좀 더 간결하게 전달을 구현할 수 있습니다.

ddx 함수들 

|함수 signature|HTML element|data type|설명|
|---|---|---|---|
|ddx_edit(data, name, to_data)|```<input type='text'>```|string||
|ddx_edit_i(data, name, to_data)|```<input type='text'>```|integer||
|ddx_edit_sig(data, name, to_data)|```<input type='text'>```|integer|범용 I/O 신호 설정용</br>element값이 sigcode이면 그대로 전달.</br>element값이 fb?.?의 형태이면 sigcode로 변환해 전달.|
|ddx_edit_ip(data, name, to_data)|```<input type='text'>``` x 4개|string|IP address 설정용.</br>name이 'ip'이면 4개의 element의 id는 각기 'ip_0', 'ip_1', 'ip_2', 'ip_3'이어야 한다.</br>data는 "xxx.xxx.xxx.xxx"의 형태로 저장된다.|
|ddx_check(data, name, to_data)|```<input type='checkbox'>```|boolean||
|ddx_radio(data, name, to_data)|```<input type='radio'>```x N개|integer|radio element들은 각자 unique한 value 속성값을 가져야 함.|

</br>

## javascript 객체 ↔ python data


setup.js
``` js
///@author: Jane Doe, BlueOcean Robot & Automation, Ltd.
///@brief: ArgosX Vision System interface - setup general
///@create: 2021-12-06
 
 
 
function init()
{
   setDomPath('/apps/argosx/svr_general');
   setUpdateGuideBar(updateGuideBar);
   setUpdateData(updateData);
   onReady();
}
 
 
...이후 생략
```

초기화 단계에서 setDomPath( ) 함수로 "/apps/argosx/svr_general" 경로를 지정했습니다.

argosx를 비롯해 모든 플러그인은 /apps/ 밑에 배치됩니다. 그리고, 경로 마지막 이름은 "svr_"에 설정 그룹명을 붙인 형태입니다.

</br>

**/apps/{app name}/svr_{setup group name}**

</br>
플러그인은 1개 혹은 여러 개의 설정 화면을 가질 수 있는데, 한 화면의 데이터는 하나의 객체로 다루어지고 이를 설정 그룹(setup group)이라고 합니다. 각 설정 그룹에는 자유롭게 이름을 부여할 수가 있는데, 이것이 설정 그룹명입니다. 이 예제에서는 설정 그룹명을 'general'로 정했습니다.

설정 그룹명에 접두어 "svr_" 대신, "get_"과 "put_"을 붙이면 각각이 getter와 putter 서비스 함수명이 됩니다. 또한, getter 함수명 끝에 '_def'를 붙이면 default값을 얻는 default getter 서비스 함수명이 됩니다.



따라서, 위의 예에서 default getter, getter, putter 서비스 함수명은 각각 get_general_def( ), get_general( ), put_general( ) 입니다.



프로젝트 폴더인 argosx/ 에 setup.py 파일을 추가합니다.
<table>
  <thead>
    <tr>
      <th style="text-align:left"></th>
      <th style="text-align:left">함수</th>
      <th style="text-align:left">호출되는 시점</th>
      <th style="text-align:left">동작</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>default getter</td>
      <td>get_general_def( )</td>
      <td>default 값이 요구될 때,</td>
      <td>default 설정값들을 하나의 객체에 속성값으로 저장하여, 전체 객체를 리턴.</td>
    </tr>
    <tr>
      <td>dgetter</td>
      <td>get_general( )</td>
      <td>	설정 화면이 열릴 때,</td>
      <td>python 플러그인이 가진 설정값들을 하나의 객체에 속성값으로 저장하여, 전체 객체를 리턴.</td>
    </tr>
    <tr>
      <td>putter</td>
      <td>put_general( )</td>
      <td>설정 화면의 값을 [ok] 혹은 [apply] 버튼으로 저장할 때,</td>
      <td>body 매개변수로 전달된 객체의 속성값들을 읽어 python 플러그인에 저장.</td>
    </tr>
  </tbody>
</table>


</br>

이제 각 함수를 구현합시다. 각 속성의 key는 ddx 함수에서 사용한 이름과 동일해야 합니다.

setup.py
``` python

""" robot application - argosx - setup
 
 
@author:    Jane Doe, BlueOcean Robot & Automation, Ltd.
@created:   2021-12-06
"""
 
 
def get_general_def() -> dict:
   """
   Returns:
      default value of setting
   """
   print('def_general_def()')
 
 
   data_def = {
      'ip_addr': '192.168.1.100',
      'port' : 54321,
      'sigcode_err': 5
   }
    
   return data_def
 
 
def get_general() -> dict:
   """
   Returns:
      setting dict.
   """
 
   print('get_general()')
    
   ret = {}
   ret["ip_addr"] = ip_addr
   ret["port"] = port
   ret["sigcode_err"] = sigcode_err
    
   return ret
 
    
def put_general(body: dict) -> int:
   """
   Args:
      body  setting dict.
    
   Returns:
      0
   """
   global ip_addr, port, sigcode_err
   print('put_general()')
 
   ip_addr = body["ip_addr"]
   port = body["port"]
   sigcode_err = body["sigcode_err"]
 
   save_to_setup_file(body) # save to file
    
   return 0
```

setup.py의 전역변수 초기화 부분도 이제 default getter 함수를 활용하는 방식으로 교체하여 중복 코드를 없애 봅시다.



setup.py
``` python
...이전 생략
 
 
gen_def = get_general_def()
ip_addr : str = gen_def['ip_addr']
port : int = gen_def['port']
sigcode_err = gen_def['sigcode_err']
```



## 동작 시험


이제, 가상제어기를 재시작하고, import argosx를 수행한 후, ArgosX의 설정화면으로 진입해봅시다. 아래와 같이 기본 설정값들이 표시됩니다.
</br> ![](../../_assets/image_45.png) </br>




IP address를 192.168.1.172로 변경하고, Failure output signal에 3.4 <enter>키를 눌러 fb3.4로 바꾼 후 [OK] 버튼을 눌러 빠져나갑니다.
</br>

다시 화면에 진입하여 새로 설정한 값들이 잘 나타나면 정상적으로 동작한 것입니다.
</br> ![](../../_assets/image_46.png) </br>






