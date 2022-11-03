# 1.4 python3 개발환경 설치
## python3 설치
아래의 절차에 따라 python v3.8을 설치합니다.



1) 아래 링크는 python v3.8.0 설치 화면으로 연결됩니다. x86 32bit를 설치하십시오.

    (주의! : Hi6 가상제어기는 32bit 애플리케이션이므로 python 런타임도 이와 일치시켜야 합니다. x86-64를 설치하면 안됩니다 !) </br>
    https://www.python.org/downloads/release/python-380/

    ![](../_assets/image_2.png)

2) Add Python 3.8 to PATH 체크 후, Curtomize installation 선택합니다.
    ![](../_assets/image_3.png)

3) 모두 check 하고 Next 클릭합니다.
    ![](../_assets/image_4.png)
4) 모두 check. 설치 경로는 주어진 대로, C:\Program Files (x86)\Python38-32 로 두고 Install 클릭합니다.
    ![](../_assets/image_5.png)
5) Disable path length limit는 안 눌러도 됩니다. Close 클릭합니다.
    ![](../_assets/image_6.png)

6) 윈도우 명령 프롬프트를 엽니다. (Windows + R 누른 후 cmd 타이핑하고, enter키)

python --version을 타이핑한 후 enter 키를 눌러 아래와 같은 버전이 출력되는지 확인합니다.

Python 3.8.0

## python import 검색 경로 추가
1) .pth 라는 파일을 만들고 안에 _common/ 폴더가 위치한 경로를 지정해준다. </br>SDK 내의 .pth 파일을 열어, Hi6 가상제어기의 HOME 경로에 맞게 아래 경로를 지정해줍니다.
 

파일내용 예:    
    D:\Hi6\home_main\apps

편집한 .pth 파일을 python 설치 경로/Lib/site-packages/ 에 배치합니다.

예: C:\Program Files (x86)\Python38-32\Lib\site-packages\.pth


## 동적 라이브러리 배치
SDK 내의 ucrtbased.dll와 vcruntime140d.dll를 python 설치 경로에 배치합니다.

예: C:\Program Files (x86)\Python38-32\ 에 복사.