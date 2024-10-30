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

https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Roles

- Properties
  속성은 요소에 추가적인 정보를 제공하여 컴포넌트의 특징이나 상황을 정의한다.
  ```js
  aria-label="검색" // 검색 관련임을 의미한다
  aria-labelledby="id" // 다른 요가 이 요소의 레이블임을 명시한다.
``` 
  
- States
  상태는 요소에 상태 정보를 나타내어 동적 변화를 정의한다.
  ```js
  aria-checked="true" // 해당 요소가 체크된 상태임을 나타낸다.
  aria-expanded="false" // 확장 가능한 요소가 닫힌 상태임을 나타낸다.
```

https://developer.mozilla.org/ko/docs/Web/Accessibility/ARIA/Attributes

## 특징

WAI-ARIA의 장점은 
- 접근성 강화: 장애가 있는 사용자에게 요소의 기본적인 역할 또는 정보를 제공하여 웹 앱의 접근성을 강화한다.
- 동적 컨텐츠 지원: 자바스크립트를 이용하여 상태를 쉽게 업데이트 할 수 있다.
- 사용자 경험 향상: 보조기기 사용자가 웹 요소의 상태를 더 정확히 이해하고 상호작용할 수 있다.

주의할 점은 의미가 있는 요소에는 특히 HTML5에서는 기존에 있는 요소를 활용해야 하며 잘못 사용하면 잘못된 정보를 제공할 수 있어 적절하게 사용해야한다.