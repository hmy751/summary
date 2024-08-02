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

> [!NOTE]
> Positioned Elements는 position 속성이 설정된 요소들을 말한다.
> static은 제외된다.


이 스태킹 컨텍스트를 생성하는 요소는 다음과 같다.
- positioned 이면서 z-index가 auto가 아닌 엘리먼트 경우
- 1 보다 작은 opacity값을 가질 때
- transform 속성이 적용된 경우
- filter, perspective, clip-path, mask 등의 속성을 가지는 경우

그리고 이 같은 스태킹 컨텍스트 안에서 스태킹 오더는 다음의 순서로 쌓이며 첫 번째가 아래로 간다.
- z-index가 음수이면서 positioned인 요소(children 포함)
- 일반 non-positioned 요소
- z-index가 auto이면서 positioned인 요소(children 포함)
- z-index가 양수이면서 positioned인 요소(children 포함)