# 3.2 실전 프로젝트 : ArgosX - callback


Hi6 제어기의 동작에는 모드 변경, 모터ON, 리셋, 기동, 정지, Accuracy OK 등 주요 이벤트들이 있습니다. 각 plug-in 들은 이러한 이벤트에 대해 고유한 동작을 수행하도록 함수를 등록시켜 둘 수 있습니다.

이러한 함수를 callback 함수라고 합니다. python 코드 내에서 능동적으로 호출하는 것이 아니라, 누군가(Hi6 host)에 의해 호출 당한다는 뜻에서 callback 이라고 하는 것입니다.



* callback 함수 등록
* callback 함수 구현
* callback 함수 참조설명서