# 3.5.4 사용자 막대의 주입

ArgosX의 사용자 막대를, 실제 티치펜던트에 주입해봅시다.



ui/menu.json 파일에 아래와 같이 "ubars" path의 항목을 추가합니다.



menu.json
``` json
[
    {
         "path": "system/appl/",
         "id": "argosx",
         "icon": "argosx/ui/lm_argosx.png",
         "label": "ArgosX Vision",
         "url": "argosx/ui/setup.html"
    },
    {
        "path": "panels",
        "id": "argosx",
        "icon": "argosx/ui/panel_argosx.png",
        "label": "ArgosX Vision",
        "url": "argosx/ui/panel.html"
    },
    {
        "path": "ubars",
        "id": "argosx",
        "url": "argosx/ui/ubar.html"
    }
]
```

가상 티치펜던트에서 우측의 사용자키 버튼을 누르면 설치된 응용의 사용자 막대들이 차례로 나타납니다. 이제 우리가 만든 ArgosX용 사용자 막대도 볼 수 있습니다. 버튼을 조작해 동작이 ArgosX stub가 정상적으로 반응하는지 확인하십시오.

![](../../_assets/image_62.png)


