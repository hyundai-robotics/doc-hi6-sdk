# 3.1.1 ArgosX와 interface plug-in의 사양

## ArgosX 비전시스템의 사양


### 기본 사양
<table>
  <thead>
    <tr>
      <th style="text-align:left">항목</th>
      <th style="text-align:left">설명</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>기능</td>
      <td>
       - LED 조명을 자체 내장하고 있고, 통신 요청에 의해 켜고 끌 수 있다.</br>
       - 최대 100개의 작업물의 시프트 값을 동시에 측정하며, 통신 요청에 응답해 보고할 수 있다.
      </td>
    </tr>
   <tr>
      <td>통신 iterface</td>
      <td>
       - 로봇제어기와 ArgosX 하드웨어는 이더넷 UDP 통신으로 서로 교신한다.</br>
        - ArgosX 하드웨어 측 IP주소는 192.168.1.XX이다. 끝자리 XX는 dip switch로 설정하기 때문에, 로봇 측에서 이에 맞게 UDP 요청을 보내야 한다.</br>
        - ArgosX 하드웨어 측 port 번호는 54321으로 고정되어 있다. 하지만, 차기 제품은 변경될 수도 있다.</br>
        - ArgosX 하드웨어는 UDP 요청을 받으면 송신자의 IP주소로 응답해준다.
      </td>
    </tr>
    <tr>
      <td>허용 장착 개수</td>
      <td>
       - 로봇제어기에는 ArgosX를 1개만 장착할 수 있다. 즉 ArgosX는 소프트웨어적으로 단일 instance이다.
      </td>
    </tr>
  </tbody>
</table>

### 프로토콜
<table>
  <thead>
    <tr>
      <th style="text-align:left">송신 방향</th>
      <th style="text-align:left">port#</th>
      <th style="text-align:left">문법과 예</th>
      <th style="text-align:left">의미</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>robot → ArgosX</td>
      <td>54321</td>
      <td>req {작업물#}</br>
        e.g. "req 39"</td>
      <td>#번 작업물의 시프트값 요청</br>작업물#는 1~100</td>
    </tr>
   <tr>
      <td>robot ← ArgosX</td>
      <td></td>
      <td>res ({x}, {y}, {z}, {rx}, {ry}, {rz})</br>
            측정에 실패하면 "fail"이란 문자열 전달</br>
            e.g. "res (30, 25.7, 11.9, 31.6, 12.8, -54.6)"</br>
            e.g. "fail"</td>
      <td>#번 작업물의 시프트값 응답</br>
        x~rz는 실수값. 단위는 mm, deg.</td>
    </tr>
    <tr>
      <td>robot → ArgosX</td>
      <td>54321</td>
      <td>light-on</td>
      <td>LED 조명을 켠다.</td>
    </tr>
    <tr>
      <td>robot → ArgosX</td>
      <td>54321</td>
      <td>light-off</td>
      <td>LED 조명을 끈다.</td>
    </tr>
  </tbody>
</table>

## ArgosX 인터페이스 플러그인의 사양


ArgosX를 인터페이스하기 위한 플러그인은 아래와 같은 사양으로 개발하겠습니다.



### 로봇언어
<table>
  <thead>
    <tr>
      <th style="text-align:left">항목</th>
      <th style="text-align:left">문법</th>
      <th style="text-align:left">설명</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>module</td>
      <td>argosx</td>
      <td></td>
    </tr>
   <tr>
      <td rowspan="2">attribute</td>
      <td>ip_addr</td>
      <td>ArgosX 하드웨어의 IP 주소 문자열 (설정가능하다.)</br>e.g. "192.168.1.44"</td>
    </tr>
    <tr>
      <td>port</td>
      <td>ArgosX 하드웨어의 port 번호.</br>(차기 제품에서 변경될 수도 있으므로 설정 가능해야 한다.)</br>e.g. 54321</td>
    </tr>
    <tr>
      <td rowspan="4">function</td>
      <td>init( )</td>
      <td>UDP통신을 위한 socket을 초기화한다.</td>
    </tr>
    <tr>
      <td>req({작업물#})</td>
      <td>작업물#번의 결과 시프트값 요청,</td>
    </tr>
    <tr>
      <td>res( )</td>
      <td>응답 대기하다가 요청 받기.</br>리턴값은 base 좌표계 시프트 배열 문자열.</br>e.g. "[30, 25.7, 11.9, 31.6, 12.8, -54.6, \"base\"]"</td>
    </tr>
    <tr>
      <td>close( )</td>
      <td>UDP통신을 위한 socket을 닫는다.</td>
    </tr>
  </tbody>
</table>

### 조명 기능
- 로봇이 motor ON 되면, ArgosX의 LED 조명도 같이 켜진다.
- 로봇이 motor OFF 되면, ArgosX의 LED 조명도 같이 꺼진다.

### 에러 처리
- ArgosX로부터 "fail"이 수신되면, 미리 설정해 둔 번호의 로봇제어기 범용 I/O 출력신호를 켠다.


### 모니터링
티치펜던트의 ArgosX용 모니터링 panel을 열어, 아래 정보들을 관측 할 수 있다.

- IP주소
- port번호
- 에러 입력할당 번호
- 요청 횟수
- 응답 횟수


### 사용자 막대 (user-bar)
티치펜던트의 ArgosX용 사용자 막대를 열면, 아래와 같은 U/I가 제공된다.

- light-on 버튼 : ArgosX의 LED 조명을 켠다.
- light-off 버튼 : ArgosX의 LED 조명을 끈다.