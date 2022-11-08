# 1.3 Hi6 가상제어기 설치

(임시)
<br></br>

## 1) 설치 
다운받은 Hi6 controller zip을 해제합니다.

### VRC 개발환경의 설치 
1. D:\util\hi6_vrc\가 되도록 복사합니다.
   (폴더 경로를 변경하려면 hi6main_platform_cfg.json, hi6tp_platform_cfg.json에 기록된 폴더 경로를 수정해야 합니다.)
2. install 폴더의 vcredist_x64.exe를 실행하여 visual studio 2013 재배포 패키지를 설치합니다.
3. install 폴더의 System32와 SysWOW64 폴더의 내용을 C:\Windows\의 각 폴더로 복사합니다.

### Qt 라이브러리의 설치
1.  C:\Qt\Qt5.7.1\5.7 경로를 생성합니다.
2. msvc2013 를 해당 경로 아래에 압축 해제합니다.

### Python 설치 
<u> 1.4 python3 개발환경 설치 </u>의 내용을 참조하시길 바랍니다.

<span style = 'background-color:#ffdce0'> 주의: 반드시 인터넷이 연결 되어있는 상태에서 실행해야 설치를 성공할 수 있습니다.</span>

1. install 폴더의 "python-3.8.0.exe"를 실행합니다. 
2. Add Python 3.8 to PATH 만 체크한 후 Customize installation 클릭.
3. 이 후의 모든 항목들에 대한 체크박스에 빠짐없이 체크한 후 install 시작
4. 'Setup was successful'확인하면 종료.
5. install 폴더의 "ucrtbased.dll"와 "vcruntime140d.dll" 파일을 복사하여 C:\Program Files (x86)\Python38-32\에 복사합니다.


## 2) 실행
1. Debug 폴더 안에 hi6_main.exe를 실행합니다.
2.  Debug 폴더 안에서 PowerShell를 실행하여 아래와 같은 명령어를 통해 TP를 실행합니다. 
   ```
    ./hi6_tp -layout=k
   ```

- hi6_tp를 바로 실행 시키면 <U>TP600</U>으로 실행하게 됩니다.

- 실질적인 Hi6 TP 모델은 <U>TP630</U>이 타겟이기 때문에 커맨드를 통해 '-layout=k'를 입력해서 TP630으로 실행해야 올바르게 실행한 것 입니다. 

<b>TP630</b>&nbsp;![](../_assets/image_81.png)  &nbsp;&nbsp; <b>TP600</b>&nbsp;![](../_assets/image_84.png)

    ### hrspace 연동이 필요한 경우 
    1. https://www.hyundai-robotics.com/customer/customer4.html?p=3 에서 HRSpace를 받아 설치합니다. 
    2. workspace에서 마우스 오른쪽 클릭하여 "모델 불러오기..."로 로봇을 로딩합니다.
    3. robot에서 마우스 오른쪽 클릭하여 "로봇속성..."에서 제어기 연결을 "ENetHi6"를 선택합니다.
    4. PC주소와 로봇제어기의 IP주소를 "127.0.0.1"로 설정합니다.
    5. 시뮬레이션 시작 버튼을 누릅니다.



<br></br>
(제어기의 설치는 추후의 인스톨러와 함께 개선 예정)



 