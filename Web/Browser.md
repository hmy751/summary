
# 브라우저의 구조
---
- User Interface - 유저와 상호작용하는 부분을 말한다. 주소 표시줄, 주소창, 뒤로/앞으로 가기 버튼 등이 있으며 개인화가 가능하게 설계되어 있다.
- Browser Engine - 서버와 사용자 상호 작용에서 가져온 웹 콘텐츠를 조정하는 역할을 하며, UI와 렌더링 엔진 사이의 동작을 제어한다.
- Rendering Engine - 요청한 웹 콘텐츠를 해석하고 표시한다. 대부분의 브라우저에서는 브라우저 엔진과 렌더링 엔진이 함께 작동하여 콘텐츠를 보여준다.
- Networking - HTTP 요청과 같이 통신 부분을 처리한다.
- UI Backend - 콤보 박스와 창 같은 기본적인 장치를 그린다. OS에서 이미 자체적으로 가지고 있는 UI들이다.
- Javascript Interpreter - 웹 컨텐츠를 조작하고 웹 페이지에 동적 동작을 가능하게 해주는 요소다.
- Data Storage - 브라우저 동작에 필요한 자료를 저장하는 계층이다.

# 브라우저의 로딩 과정
---
## 브라우저의 렌더링 과정

웹 페이지에 필요한 리소스를 받고 해석하여 여러 과정을 거쳐 콘텐츠를 보여주는 과정을 브라우저의 렌더링 과정이라 한다.
크게 Parsing => Calculate Style  => Layout => Paint => Composite & Render 과정으로 이루어진다.
### Parsing(파싱)
파싱은 브라우저가 코드를 이해하고 사용할 수 있는 구조로 변환하는 작업을 의미한다. HTML파일을 해석하여 리소스 구성에 따라 DOM 트리 및 CSSOM 트리를 구성한다.
파싱 결과는 부모-자식 관계를 가지는 노트 트리 구조이다.
#### DOM 트리 구성
렌더링 엔진에서 HTML데이터를 수신하고 해석, 파싱 과정을 거쳐 DOM을 생성한 후, 각 DOM 객체를 트리 데이터 구조로 연결해 DOM트리를 구성한다.
#### CSSOM 트리 구성
파싱 과정 중 스타일시트 파일을 만나면 CSS를 해석하며 CSSOM 트리를 구성한다.
### Calculate Style(스타일 계산)
스타일 계산 단계에서는 파싱 단계에서 생성된 DOM, CSSOM 트리를 가지고 스타일 매칭 후 렌더 트리를 구성한다. 
### Layout(레이아웃)
레이아웃 단계에서는 노드의 정확한 위치와 크기를 계산한다. 루트 노드 부터 노드를 순회하면서 계산하고, 레이아웃의 결과로 각 노드의 정확한 위치와 크기를 픽셀값으로 렌더 트리에 반영한다.
CSS에서 %값으로 크기를 지정하면 레이아웃 단계를 거쳐 측정 가능한 픽셀 단위로 변환된다.
### Paint(페인트)
레이아웃 단계에서 계산된 값을 이용해 렌더 트리의 각 노드를 화면상의 실제 픽셀로 변환한다.
이때 위치, 크기, 모양과 관계없는 CSS 속성(색상, 투명도 등)을 적용하여 색을 칠하는 순서를 판단한다. transform 속성등을 기준으로 픽셀로 변환된 결과들은 포토샵의 레이어 처럼 생성되어 개별 레이어로 관리된다. 
### Composite & Render(합성 & 렌더)
페인트 단계에서 생성된 레이어를 합성하여 스크린을 업데이트하며 화면에서 웹 페이지를 볼 수 있다.

## 브라우저의 성능 향상
### 리플로우(레이아웃)과 리페인트
브라우저의 렌더링은 상황에 따라 반복하여 발생한다. 재 렌더링 과정에서 요소에 기하학적인 영향을 주면 렌더 트리 부터 영향을 미쳐 레이아웃 과정 부터 수행하는데 이를 리플로우라고 한다. 
기하학적인 영향을 주지않는 CSS속성값을 변경하여 레이아웃 과정을 건너 뛰고 페인트 부터 수행하는 과정을 리페인트라고 한다.

레이아웃이 다시 발생하면 픽셀 부터 다시 계산해야하므로 부하가 더 크다. 따라서 성능을 향상하려면 기하적인 영향을 주지 않는 CSS를 최대한 활용해야 한다.
- 기하적인 영향을 주는 CSS 속성값
	- height
	- width
	- left
	- top
	- position
	- font-size
	- line-height
- 기하적인 영향을 주지 않는 CSS 속성값
	- background-color
	- color
	- visibility
	- text-decoration
	- transform
https://docs.google.com/spreadsheets/u/0/d/1Hvi0nu2wG3oQ51XRHtMv-A_ZlidnwUYwgQsPQUg1R2s/pub?single=true&gid=0&output=html
### 렌더링 최적화

#### 레이아웃 최적화
- 레이아웃 스레싱 피하기
	offsetHeight, offsetTop과 같은 계산된 값을 속성으로 읽을 때 강제로 동기 레이아웃을 수행한다. 이를 피하기 위해 읽는 과정을 최소화 해야한다.
	```js
	for (let i = 0; i < target.length; i++) {
		target[i].style.wiidth = box.offsetWidth;
		...
	}
	
	...
	
	const width = box.offsetWidth;
	
	for (let i = 0; i < target.length; i++) {
		target[i].style.wiidth = width;
		...
	}
	```

- 가능한 하위 노드의 DOM을 조작하여 스타일을 변경하기
	상위 노드의 스타일을 변경하면 하위에도 영향을 주기 때문에 최대한 하위 노드의 스타일을 변경한다.

- 숨어있는 엘리먼트 수정
	display: none 스타일은 애초에 공간을 차지하지 않기 때문에 변경하여도 리플로우와 리페인트가 발생하지 않지만 visibility: hidden은 보이지만 않기 때문에 변경하게 되면 리페인트는 발생하지 않지만 리플로우는 발생하게 된다.
#### 애니메이션 최적화
한 프레임에 16ms(60fps)안에 처리해야 자연스러운 애니메이션 처리가 되며 다음과 같은 방법등을 활용할 수 있다.

- JS 보다는 CSS 애니메이션 활용
	 자바스크립트 실행시간이 10ms 이내에 수행되어야 레이아웃과 페인트 과정을 포함하여 16ms안에 완료될 수 있다. 이때 JS실행 시간을 줄이기 위해 CSS를 활용하여 애니메이션을 처리하는게 권장된다.
	 이때 position: absolute, transform: translate()를 활용할 수 있다.
	 position: absolute는 주변 영역에 여향을 주지 않으며, transform: translate()는 레이어가 분리되며 엘리먼트에 영향을 주지 않기 때문이다.
	 ```css
	 .animation-item {
		 positon: absolute;
		 animation: move 3s ease infinite;
	 }

	@keyframes move {
		50% {
			transform: translate(100px, 100px);
		}
	}
		
		```
	

- requestAnimationFrame 활용
	프레임 속도 60fps를 맞출수 있도록 간격이 설정되며 페인트 단계에서 실행되어 자연스러운 애니메이션 동작을 수행할 수 있도록 해준다.

https://ui.toast.com/fe-guide/ko_PERFORMANCE
# 브라우저의 동시성
---
자바스크립트는 싱글 스레드 기반의 언어다. 싱글 스레드로 동작하기 때문에 [[Async, Promise#^c6e5a8|블로킹]]이 발생할 수 밖에 없고 자바스크립트 엔진 만으로는 브라우저의 동시성(Concurrency)을 지원할 수 없다.
이를 위해 자바스크립트 뿐만 아니라 브라우저의 여러 스레드와 프로세스를 통해서 동시성을 지원할 수 있다.

## Event Loop(이벤트 루프)
브라우저의 동시성을 위해 자바스크립트 엔진(콜 스택, Heap) 외에 이벤트 루프, Task Queue(테스크 큐), Web API등을 이용하여 브라우저가 지원 하여 처리한다.

- Event Loop - 이벤트 루프는 콜 스택과 테스크 큐 사이에 위치 한다. 
  콜 스택이 비어 있는지 감시하며 비어 있을 경우 테스크 큐에 있는 작업을 콜 스택으로 전달하는 역할을 한다.
- Web API - setTimeout, DOM, Fetch, HTTP 등 브라우저와 상호 작용할 수 있도록 하는 API 이다.
- Task Queue - Web API 콜백 또는 이벤트 핸들러를 추후에 실행될 수 있도록 저장하는 공간이다.

https://www.lydiahallie.com/blog/event-loop
### 이벤트 루프 처리 과정
```js
setTimeout(() => {
  console.log('first');
}, 0);

console.log('second');
```
먼저 setTimeout**함수가 호출**되어 **실행 컨텍스트를 생성**하고 **콜 스택에 쌓인다**.
콜 스택에 담기기 때문에  해당 **함수가 실행**되고 **콜백 함수와 타이머 정보**가 **Web API로** 넘긴다. **해당 컨텍스트는 종료되고** **콜 스택에서 팝** 된다.
콜 스택에서 실행이 종료되어 다음 코드로 넘어가고 console.log호출하고 해당 컨텍스트를 생성하여 콜 스택에 담아 실행 한다.
'second'를 출력하고 해당 컨텍스트는 종료되어 콜 스택에서 팝 된다.
그 동안 **Web API에서** 넘겨 받은 해당 콜백 함수는 **지정된 타이머 시간 이후** **테스크 큐로** 해당 콜백 함수를 **넘긴다.**
**이벤트 루프**가 콜 스택과 테스크 큐를 **감시**하며, **콜 스택이 비어 있게 될 때** **테스크 큐의** 콜백 함수를 **콜 스택으로 넘긴다.**
콜 스택에 콜백 함수가 넘어오게 되고 실행되어 'first'를 출력하며 종료된다.

## 마이크로테스크 큐와 프로미스

^c84bba

마이크로테스크 큐는 **프로미스 콜백**을 저장하며 추후에 실행될 수 있도록 저장하는 공간이다. **우선순위**는 **테스크 큐보다 더 높아** 먼저 실행된다.

```js
setTimeout(() => {
	console.log(1)
}, 0);

Promise.resolve()
	.then(() => console.log(2))
	.then(() => console.log(3));
```
setTimeout함수와 Promise가 같이 실행될 때
먼저 setTimeout함수가 호출되어 해당 컨텍스트가 콜 스택에 담겨 실행되고 콜백 함수와 타이머 정보가 Web API에 넘겨진다.
setTimeout함수의 실행 컨텍스트는 종료되고 Promise가 호출된다.
Promise의 실행 컨텍스트가 콜 스택에 담기고 실행된다.
Promise는 실행되어 프로미스 객체를 생성하고 바로 resolve되며 종료된다. 해당 프로미스의 then함수가 다시 실행 컨텍스트를 생성하며 콜 스택에 담기고 실행 되며 해당 콜백 함수를 마이크로테스크 큐에 넘기며 해당 컨텍스트는 종료된다.
이제 테스크 큐와 마이크로테스크 큐에 콜백함수들이 대기하고 이벤트 루프는 콜 스택이 비어있는지 감시한다.
모든 함수가 종료되어 콜 스택이 비어 있으므로 테스크들을 넘기는데 마이크로테스크 큐가 우선순위가 높으므로 해당 테스크들을 모두 콜 스택에 넘긴다.
모두 종료되면 그 뒤에 테스크 큐에 있는 콜백 함수를 넘겨 실행하며 종료된다.

- fetch함수 처리
```js
fetch('https://api...')
  .then((res) => console.log(res));

console.log('end');
```
fetch가 호출 되면 해당 실행 컨텍스트가 생성되고 콜 스택에 담겨 실행된다.
실행 되면 fetch는 Web API로서 프로미스 오브젝트를 생성하고 해당 프로미스 오브젝트는 pending상태와 undefined결과를 가지고 then 콜백 함수가 해당 프로미스 오브젝트의 PromiseReaction에 넘겨지며 해당 컨텍스트는 종료되며 콜 스택에서 팝 된다.
fetch가 외부에 요청하는 동안 console.log('end')가 호출되고 실행되어 end를 출력한다.
요청이 완료되면 해당 프로미스의 오브젝트는 'fullfilled'상태로 되고 해당 결과는 response에 담긴다.
fullfilled상태가 되면 바로 PromiseReations에 저장한 콜백 함수를 마이크로테스크 큐에 넘긴다.
이벤트 루프가 콜 스택이 비어있는 것을 확인하면 해당 콜백 함수를 콜 스택으로 넘기며 실행 된다.
해당 콜백 함수에서 response를 출력하며 종료된다.

```js
console.log("start"); 

setTimeout(() => { 
  console.log("macrotask 1"); 
},  0); 

Promise.resolve() 
  .then(() => { 
    console.log("microtask 1"); 
    }) 
  .then(() => { 
    console.log("microtask 2"); 
  }); 

requestAnimationFrame(() => { console.log("requestAnimationFrame callback"); }); 

console.log("end");

```

## requestAnimationFrame(rAF)
rAF(requestAnimationFrame)은 리페인트 단계에서 애니메이션 함수를 호출할 수 있도록 브라우저에 요청하는 함수로 효율적이고 자연스럽게 그릴수 있게 해준다.

대게 화면의 주사율은 60fps이며 1초에 60번 그려져야 애니메이션이 화면에 자연스럽게 그려지게 된다. 이를 위해 1000 / 60 ms마다 그려져야 하는데 rAF가 그에 맞게 그려질 수 있도록 한다.

### 특징
#### 프레임을 고려한 실행
일반적인 타이머 함수로 애니메이션을 호출하게 되면 브라우저 로딩 과정중에 콜 스택에서 실행하게 되고 다시 리플로우가 발생하게 된다. rAF를 사용하게 되면 프레임을 고려하여 리페인트 단계에서 실행하게 하여 프레임이 지연되는 문제가 해결된다.

#### 백그라운드 동작 중지
일반적인 타이머 함수를 실행하게 되면 브라우저가 다른 탭을 보거나 비활성화 될때에도 실행되어 리소스가 낭비 되는데 rAF는 실행이 중지되어 리소스를 낭비하지 않게 된다.

#### 주사율에 맞게 호출
무조건 1초에 60번 호출하지 않고 디스플레이의 주사율에 맞게 호출한다. 예를들어 144hz이면 1초에 144번 호출할 수 있도록 최적화 하여 호출하게 한다.

## Animation Frames
rAF의 콜백 함수는  테스크 큐나 마이로테스크 큐가 아닌 Animation frames에 댬겨서 처리 된다.

### 사용법
인자로 실행할 콜백 함수를 받으며 재귀 형태로 호출하여 애니메이션을 실행 한다.
```js
const animation = () => {
	...
	
	requestAnimationFrame(animation);
};

requestAnimationFrame(animation);
```