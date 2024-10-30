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

# WAI-ARIA
---
WAI-ARIA(Web Accessiblity Initiative - Accessible Rich Internet Applications)은 웹 앱의 접근성을 향상시키기 위한 표준이다. 주로 시각적 요소 정보를 스크린 리더와 같은 보조 기술에 더 잘 전달하며, 장애가 있는 사용자도 웹 컨텐츠를 접근할 수 있도록 돕는 역할을 한다.

## 구성요소

- Role
  해당 요소가 어떤 역할을 하는지 나타낸다. HTML5부터는 대부분의 요소들이 각자의 역할에 맞는 요소들이 있기 때문에, 역할에 맞는 요소들이 있다면 명시하지 않고 해당 요소를 그대로 사용하면 된다.
  요소의 역할이 없는 경우나, div태그를 사용해야 하는 경우에는 역할을 명시하여 사용한다.
  ```js
    <div role="button" ...
    <div role="dialog" ...
```
		
- Properties
  속성은 컴포넌트의 특징이나 상황을 정의한다.
- States