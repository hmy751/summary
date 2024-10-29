# 전역속성
---
## tabindex
tabindex global attribute으로 요소가 포커스 가능함을 나타내며 tab키를 사용하여 어느 순서에 위치할지 지정한다.
즉 우선순위에 관여된다.

대화형 콘텐츠는 기본값이 0이고, 비 대화형 콘텐츠는 -1이 기본값이다.
높을 수록 우선순위가 높다. 
양의 정수일경우 키보드 tab 탐색으로 가능하지만 조작에 순서가 바뀔수 있으니 지양해야한다.
0일 경우 키보드 탐색으로 접근이 가능하다.
음일 경우 키보드 탐색은 불가능하지만 포커스는 가능함을 뜻하여 시각적(클릭) 포커스가 가능함을 의미한다.

https://developer.mozilla.org/ko/docs/Web/HTML/Global_attributes/tabindex