# 3.1.3 argosx 프로젝트 생성

apps/ 폴더 밑에 argosx 라는 이름의 폴더를 생성합니다.

탐색기의 argosx/ 폴더에 마우스 우버튼을 클릭한 후, pop-up 메뉴에서 "Code(으)로 열기"를 클릭하십시오.

argosx/ 폴더를 프로젝트로 하여 vscode가 열립니다. argosx/ 밑에 info.json을 아래와 같이 만듭니다.

info.json

```json
{
   "author" : "BlueOcean Robot & Automation, Ltd.",
   "binding" : "plug-in",
   "copyright" : "All right reserved",
   "description" : "ArgosX vision system interface",
   "entry" : "main.py",
   "menu" : "ui/menu.json",
   "startup" : "manual",
   "version" : "v0.9.0"
}
```


우선 argosx/ 폴더 밑에 main.py 파일을 생성합니다. (@author에는 본인 이름을 적어도 됩니다.)

main.py
```python 
""" ArgosX Vision System interface - main
 
 
@author:    Jane Doe, BlueOcean Robot & Automation, Ltd.
@created:   2021-12-06
"""
```

그리고 아래와 같이 이를 시험할 job 파일을 하나 교시합니다.

```
import argosx
end
```
