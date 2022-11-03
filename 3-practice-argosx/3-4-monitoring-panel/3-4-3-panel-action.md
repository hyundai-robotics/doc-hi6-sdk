# 3.4.3 모니터링 panel의 동작


python 코드에서 IP주소, port번호, 에러 할당입력 번호의 변수는 이미 존재합니다.

요청 횟수, 응답 횟수를 관리해야 하므로 아래와 같이 n_req, n_res 변수를 추가해주고, get_general( ) 함수에서 함께 리턴되도록 연결해줍니다.



setup.py
``` python 
이전 생략...
 
 
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
   ret["n_req"] = n_req
   ret["n_res"] = n_res
    
   return ret
 
 
생략...
 
 
gen_def = get_general_def()
ip_addr : str = gen_def['ip_addr']
port : int = gen_def['port']
sigcode_err = gen_def['sigcode_err']
n_req = 0   # request count
n_res = 0   # response count
```

로봇언어 명령문의 구현에 n_req와 n_res의 count-up 동작을 추가해줍니다.



roblang.py
```python 
이전 생략...
 
 
def req(work_no: int) -> int:
   """
   send request command to ArgosX
   e.g. "req 39"
   Args:
      work_no     work#    1~100
 
 
   Returns:
         >=0   the number of bytes sent
         -1    no socket. init() should be called.
   """
   msg = "req " + str(work_no)
   setup.n_req += 1                           # <---------- 추가
   return comm.send_msg(msg)
 
 
생략...
 
 
def _res_cont(addr_on_timeout: int_or_str) -> str:
   """res() implementation for cont-mode"""
   val = 0
   msg = ""
   timeout = _check_timeout_and_branch(addr_on_timeout)
   if timeout:
      val = 1
   else:
      msg = comm.recv_msg()
      print(msg)
      if msg=="res fail":
         val = 1
         msg = ""
      elif msg=="":
         xhost.req_to_continue()
      else:
         msg = get_base_shift_array_from_res(msg)
         setup.n_res += 1                    # <---------- 추가
   xhost.io_set_out_bit(setup.sigcode_err, val)
   return msg
```

ui/panel.js 파일에 아래와 같이 작성합니다. 500msec마다 updateData( ) 함수가 호출되어, main보드로부터 general 데이터를 모두 가져온 후, display( ) 함수를 통해 html 화면에 주입하는 동작입니다.



ui/panel.js
``` js

///@author: Jane Doe, BlueOcean Robot & Automation, Ltd.
///@brief: ArgosX Vision System interface - panel
///@create: 2021-12-07
 
 
 
function init()
{
   updateData();
   setInterval('updateData()', 500);
}
 
 
///@brief
function updateData()
{
   var path = '/apps/argosx/svr_general';
   var url = domainMb()+path;
   $.get(url, function(data) {
      display(data);
   });
}
 
 
///@return     e.g. "http://192.168.1.150:8888"
function domainMb()
{
   var domain = "http://" + window.location.hostname + ":8888";
   console.log(domain);
   return domain;
}
 
 
///@param[in]  data
///@brief      (callback function) form <- data
function display(data)
{
   console.log(data);
 
   $('#ip_addr').text(data.ip_addr);
   $('#port').text(data.port);
   $('#sigcode_err').text(data.sigcode_err);
   $('#n_req').text(data.n_req);
   $('#n_res').text(data.n_res);
}
```

가상 제어기를 재실행한 후, 다시 VS Code의 Go Live를 실행합니다. 이제 아래와 같이 웹 브라우저 화면에 값이 출력되는 것을 볼 수 있습니다.

ArgosX stub를 실행하고 가상 티치펜던트로 job 프로그램을 실행해, 요청과 응답을 수행해봅시다. n.request와 n.response 값이 증가하면 정상입니다.
<br></br>
![](../../_assets/image_53.png)





