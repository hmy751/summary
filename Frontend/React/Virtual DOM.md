# Virtual DOM
---
리액트에서 Virtual DOM은 브라우저의 DOM과는 다르다. 우선 브라우저의 DOM은 객체 기반의 구조화된 문서 표현이며 브라우저 렌더링에 표현되는 HTML요소들을 나타내고 이를 조작할 수 도 있다.

리액트의 Virtual DOM은 브라우저 DOM과 유사하며 DOM을 복사한 형태로 자바스크립트 객체로 이를 나타내고 메모리에 저장한다. 실제 브라우저에서 직접 접근하여 UI를 조작하는데 사용되지 않고, 단지 리액트에서 리렌더링 시 UI변화를 비교하기 위해 사용된다.

리액트에서 리렌더링을 유발하는 요소는 state와 props의 변화가 있다. state 또는 props의 변화로 리렌더링이 발생하면 기존의 가상돔(Virtual DOM)을 두고 변화된 버츄어 돔을 만든다.
두 가상돔을 비교하여 어떤 부분이 변화됐는지 확인하고 리액트가 실제 변경에 대해 준비가 완료되면, 변화된 부분을 실제 브라우저 DOM에 알려 화면의 변화된 요소를 다시 그리게 된다.

가상 돔을 비교하는 과정을 재조정(Reconciliation)이라고 하며, 여기에는 Diffing 알고리즘과 Fiber가 관여한다.



https://callmedevmomo.medium.com/virtual-dom-react-%ED%95%B5%EC%8B%AC%EC%A0%95%EB%A6%AC-bfbfcecc4fbb