---
undefined: ""
File: CSS
---
# CSS
---
## Stacking Context
https://dongmin-jang.medium.com/css-stacking-context-172f9bd1af8b
스태킹 컨텍스트(Stacking Context)는 엘리먼트 요소들이 가진 속성으로 인해 화면에 앞 뒤로 쌓이는 순서가 생기는데, 이때 결정되는 기준을 통해 나눠진 그룹을 의미한다.

그리고 나눠진 스태킹 컨텍스트 안에서 쌓이는 순서가 생기는데 이를  스태킹 오더(Stacking Order)라고 한다.

스태킹 컨텍스트는 루트 엘리먼트 부터 형성되고 만약 스태킹 컨텍스트를 나누는 새로운 기준이 있으면 그 요소를 기준으로 또 다시 새로운 스태킹 컨텍스트가 생기며 나뉘게 된다.
이 스태킹 컨텍스트를 생성하는 요소는 
- positioned 엘리먼트, 
- transform, 
