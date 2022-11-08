# 3.3.4 설명화면 메뉴의 주입

hello_world 예제에서 이미 메뉴 주입을 실습해 본 바 있습니다.

ArgosX 의 설정 화면도 티치펜던트의 시스템 - 응용파라미터 메뉴 밑에 주입해 봅시다.



ui/ 폴더 밑에 아래와 같은 menu.json 파일을 생성합니다.



menu.json
``` json
[
    {
        "path": "system/appl/",
        "id": "argosx",
        "icon": "argosx/ui/lm_argosx.png",
        "label": "ArgosX Vision",
        "url": "argosx/ui/setup.html"
    }
]
```


이번에는 그림 icon을 넣어봅시다. 아래 2개의 site를 이용해서 104x104 투명배경 png icon을 얻었습니다.



Bootstrap Icons (https://icons.getbootstrap.com/#icons) : open 소스 icon 라이브러리. SVG 벡터 파일 형태로 제공.

EZGIFCOM (https://ezgif.com/svg-to-png) : SVG 파일을 원하는 해상도의 PNG 파일로 온라인 변환.

</br></br>
![](../../_assets/lm_argosx.png) lm_argosx.png 의 예 (이 그림을 다운받아 사용해도 됩니다.)


<br></br>

이제 가상 메인보드과 가상 티치펜던트를 재실행 합니다.


시스템 - 응용 파라미터 메뉴로 진입하면 아래와 같이 새로 추가된 ArgosX Vision 메뉴 항목이 보입니다.
</br>
![](../../_assets/image_42.png)
</br>



메뉴를 선택하면 잠시 후, 우리가 작성한 레이아웃이 나타납니다.
</br>
![](../../_assets/image_43.png)
</br>






