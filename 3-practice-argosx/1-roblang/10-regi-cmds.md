# 3.1.10 로봇언어 명령입력 등록

실제 TP를 사용하다보면 로봇언어를 직접 입력하는 것은 번거로운 일입니다.

따라서 [명령입력]을 통해 로봇 언어를 좀더 쉽게 작성할 수 있습니다.

이를 위해 명령입력 안에 로봇언어를 등록해주어야 합니다.

먼저, info.json에 "cmds" 라벨 을 추가하고, 이에 상응하는 값은 cmds의 내용을 정리한 json을 지정해줍니다.

info.json
```json
{
	"author" : "BlueOcean Robot & Automation, Ltd.",
	"binding" : "plug-in",
	"cmds" : "cmds.json",
	"copyright" : "All right reserved",
	"description" : "ArgosX Vision System interface",
	"entry" : "main.py",
	"menu" : "ui/menu.json",
	"startup" : "boot",
	"version" : "v0.9.0"
}
```

argosx 폴더 안에 cmds.json을 추가하여 해당 파일 안에 명령어의 속성들을 정의해줍니다.

``` json
{
	"fmts" : [
		{
			"name": "init"
		},
		{
			"name": "req",
			"samples": "req 39",
			"props": [
				{
					"guide": "work no.",
					"range": "[1~100]"
				}
			]
		},
		{
			"name": "res"
		},
		{
			"name": "close"
		}
	]
}

```

|항목|의미|예제|
|---|---|---|
|fmts|로봇언어 모듈명|"fmts"|
|name|함수명 |"name": "init"|
|samples|샘플(로봇언어 입력형태)|"samples": "req 39"|
|props|함수의 입력 속성|"props"|
|guide|입력 인자 가이드 메세지|"guide": "work no."|
|range|입력 인자 값의 범위|"range": "[1~100]"|

각 항목들의 의미하는 바는 위의 표와 같으며, 입력인자가 많은 경우는 "props" 안에 입력인자 수 대로 속성들을 묶음으로 추가하면 됩니다.


<br></br>
추가된 명령어는 TP의 하단의 [명령입력] 버튼을 눌러 확인할 수 있습니다.
![](../../_assets/image_82.png)

[명령입력]-[argosx]
![](../../_assets/image_83.png)