# 2.1 hello_world 프로젝트 생성 - 폴더와 메타정보

apps/ 폴더 밑에 hello_world 라는 이름의 폴더를 생성합니다. 폴더의 이름이 곧 프로젝트명이며, apps/ 폴더에서 유일한(unique) 이름이어야 합니다.

탐색기의 hello_world/ 폴더에 마우스 우버튼을 클릭한 후, pop-up 메뉴에서 "Code(으)로 열기"를 클릭하십시오.

![](../_assets/image_13.png)

hello_world/ 폴더를 프로젝트로 하여 vscode가 열립니다. 좌상단의 New File 버튼을 클릭하여 폴더에 새 파일을 만들 수 있습니다. 파일명은 info.json으로 합니다.

![](../_assets/image_14.png)

info.json을 클릭해 열고 아래와 같이 입력합니다.

![](../_assets/image_15.png)

## info.json

``` json
{
   "author" : "Hyundai Robotics",
   "binding" : "plug-in",
   "copyright" : "All right reserved",
   "description" : "First example - hello_world",
   "entry" : "hello_world.py",
   "menu" : "ui/menu.json",
   "startup" : "manual",
   "version" : "v0.9.0"
}
```

</br>
<table>
  <thead>
    <tr>
      <th style="text-align:left">key</th>
      <th style="text-align:left">의미</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>author</td>
      <td>
       저자
      </td>
    </tr>
   <tr>
      <td>binding</td>
      <td>
       Hi6 호스트 소프트웨어와의 결합 형태</br>
       - plug-in: 결한된 형태로 실행됨.</br>
       - stand-alone: 독립된 응용 프로그램(process)으로 실행됨. 
      </td>
    </tr>
    <tr>
      <td>copyright</td>
      <td>
       저작권
      </td>
    </tr>
    <tr>
      <td>description</td>
      <td>
       설명	
      </td>
    </tr>
    <tr>
      <td>entry</td>
      <td>
       실행을 시작할 시작 위치의 파일명</br>
       apps/ 폴더에서 유일한(unique) 이름이어야 하므로, 가급적 아래의 이름들 중 하나로 정함</br>
       - {프로젝트명}.py</br>
       - {프로젝트명}_main.py
      </td>
    </tr>
    <tr>
      <td>menu</td>
      <td>
       사용자 인터페이스의 메뉴 구조	
      </td>
    </tr>
     <tr>
      <td>startup</td>
      <td>
       실행 시작 방식
       - manual: 수동으로 실행 시작
       - boot: 부팅시 자동으로 실행 시작	
      </td>
    </tr>
     <tr>
      <td>version</td>
      <td>
       버전 문자열	
      </td>
    </tr>
  </tbody>
</table>