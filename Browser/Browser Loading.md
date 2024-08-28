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

- 파싱 과정
파싱 과정은 크게 바이트 -> 문자 -> 토큰 -> 노드 -> 트리 로 구성된다.
브라우저는 서버로 부터 바이트 형태(2진수)로 HTML 문서 데이터를 받는다.
바이트 형태의 HTML 문서는 meta 태그의 charset어트리뷰트에 의해 지정된 인코딩 방식(UTF-8)을 기준으로 문자열로 변환된다.
참고로 charset어트리뷰트는 http response header에 `content-type: text/html, charset=utf-8`로 담겨 응답을 받는다.
브라우저는 이를 확인하고 문자열로 변환한다.

문자열은 문법적 의미를 갖는 코드의 최소 단위인 토큰들로 분해한다.

각 토큰들은 객체로 변환되어 노드들을 생성한다. 이 노드들은 DOM을 구성하는 기본 요소가 된다.

이 노드들은 HTML의 중첩관계를 반영하여 트리 자료구조로 구성되며 DOM트리를 형성한다.

#### DOM 트리 구성
렌더링 엔진에서 HTML데이터를 수신하고 해석, 파싱 과정을 거쳐 DOM을 생성한 후, 각 DOM 객체를 트리 데이터 구조로 연결해 DOM트리를 구성한다.
#### CSSOM 트리 구성
DOM파싱 과정 중 스타일시트 파일을 만나면 CSS를 해석하며 CSSOM 트리를 구성한다.

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
### 크리티컬 렌더링 패스(Critical Rendering Path)
크리티컬 렌더링 패스는 웹 페이지가 유저에게 빠르게 보여지기 위해 최적화 되어야 하는 중요한 경로를 말한다. 주로 초기 렌더링과 관련되어 있으며 경로의 구성은 HTML 파싱, CSS 파싱 => 렌더 트리 생성 => 레이아웃 => 페인트로 구성된다.
브라우저의 렌더링과 차이점은 브라우저의 렌더링은 유저에게 보여지는 모든 과정을 의미하며, 크리티컬 렌더링 패스는 초기 렌더링과 관련되어 있다.

### 렌더 블로킹(Render Blocking), 블록 리소스 최적화
브라우저 렌더링 과정 중에 HTML, CSS, JS파일을 만나면 파싱 과정을 거친다.
먼저 HTML파일을 다운받아 읽어 파싱과정을 거친다. 파싱하는 과정 중간에 script태그나 style, link태그 등을 만나면 HTML파싱을 중단하며 DOM트리 생성이 중단되는데 이를 렌더 블로킹이라고 한다.
https://developer.chrome.com/docs/lighthouse/performance/render-blocking-resources?hl=ko

#### CSS에 의한 렌더 블로킹
HTML 파싱 중간에 link, style태그를 만난 경우 해당 CSS파일을 로드하여 파싱한 후 CSSOM트리를 만든다.
DOM트리 생성 중간에 이를 방지하기 위해 style, link태그는 head태그에 위치하여 미리 로드후 CSSOM을 생성시킨다.

#### JS에 의한 렌더 블로킹
HTML 파싱 중간에 script태그를 만난경우 즉시 JS파일을 로드하여 파싱한 후 실행하게 된다.
DOM트리 생성 전에 자바스크립트가 실행 되므로 아직 요소가 없다면 정상적으로 동작하지 않을 수 있다.
이를 방지 하기 위해 script태그는 body태그에 최 하단에 위치시킨다.
이렇게 하면 블로킹도 방지하고 페이지 로딩시간도 단축 된다.

- async, defer 어트리뷰트
자바스크립트 파싱에 의한 블로킹을 해결하기위해 사용된다.
async와 defer어트리뷰트는 src 어트리뷰트를 통해 외부 자바스크립트 파일을 로드하는 경우에만 사용할 수 있다. src어트리뷰트가 없는 인라인 자바스크립트에는 사용할 수 없다.
둘 다 사용하게 되면 자바스크립트 파일의 로딩이 비동기적으로 HTML파싱과 동시에 진행된다.
하지만 async 어트리뷰트의 경우 자바스크립트의 비동기 로딩이 완료되면 HTML파싱을 중단하고 자바스크립트의 파싱과 실행을 진행한다.
defer 어트리뷰트의 경우 자바스크립트 로딩이 완료돼도 HTML파싱을 계속 진행하고 **HTML의 파싱이 완료된 직후**, 즉 **DOM 생성이 완료**되는 **DOMContentLoaded 이벤트 발생 시점**에 자바스크립트의 파싱과 실행을 진행한다.


### 리플로우(레이아웃)과 리페인트
브라우저의 렌더링은 상황에 따라 반복하여 발생한다. 재 렌더링 과정에서 요소에 기하학적인 영향을 주면 렌더 트리 부터 영향을 미쳐 레이아웃 과정 부터 다시 수행하는데 이를 리플로우라고 한다. 
기하학적인 영향을 주지않는 CSS속성값을 변경하여 리플로우 과정을 건너 뛰고 페인트 부터 수행하는 과정을 리페인트라고 한다.

레이아웃이 다시 발생하면 픽셀 부터 다시 계산해야하므로 리페인트에 비해 부하가 더 크다. 따라서 성능을 향상하려면 가능한 기하학적인 영향을 주지 않는 CSS를 최대한 활용해야 한다.

- 기하학적인 영향을 주는 CSS 속성값
	- height
	- width
	- left
	- top
	- position
	- font-size
	- line-height
- 기하학적인 영향을 주지 않는 CSS 속성값
	- background-color
	- color
	- visibility
	- text-decoration
	- transform
https://docs.google.com/spreadsheets/u/0/d/1Hvi0nu2wG3oQ51XRHtMv-A_ZlidnwUYwgQsPQUg1R2s/pub?single=true&gid=0&output=html

#### 레이아웃 최적화
- 강제 동기 레이아웃
강제 동기 레이아웃은 자바스크립트의 코드 실행중에 DOM의 스타일을 변경한 후 offsetHeight과 같은 계산된 값을 속성으로 읽을 때 강제로 레이아웃을 수행하는 것을 말한다.

원래 자바스크립트 실행 후 스타일 단계를 거쳐서 레이아웃을 진행하지만 스타일을 변경하면 이전 레이아웃이 무효하다고 판단하며, 이때 offsetHeight과 같은 계산된 값을 속성으로 읽으려고 할때 바로 레이아웃 과정을 거쳐 필요한 계산값을 읽어 들이게 된다.

해당 과정은 성능을 저하시키므로 지양해야 합니다.

- 레이아웃 스레싱 피하기
한 프레임 내에서 강제 동기 레이아웃을 연속적으로 발생시키는 것을 레이아웃 스레싱이라고 한다.

해당 현상이 발생하는 경우는 반복문에서 요소를 순회할 때 해당 요소의 값을 읽으면서 변경하는 경우 강제 동기 레이아웃 과정이 계속해서 발생하게 된다.

```jsx
for (let i = 0; i < paragraphs.length; i++) {
	paragraphs[i].style.width = box.offsetWidth + 'px';
}
```

개선 방법은 필요한 값을 변수로 할당하여 반복문 바깥에서 선언하고 할당 과정만 반복하여 레이아웃 스래싱을 피할 수 있다.

```jsx
const boxOffset = box.offsetWidth;

for (let i = 0; i < paragraphs.length; i++) {
	paragraphs[i].style.width = boxOffset + 'px';
}
```

- 가능한 하위 노드의 DOM을 조작하여 스타일을 변경하기
	상위 노드의 스타일을 변경하면 하위에도 영향을 주기 때문에 최대한 하위 노드의 스타일을 변경한다.

- 숨어있는 엘리먼트 수정
	display: none 스타일은 애초에 공간을 차지하지 않기 때문에 변경하여도 리플로우와 리페인트가 발생하지 않지만 visibility: hidden은 보이지만 않기 때문에 변경하게 되면 리페인트는 발생하지 않지만 리플로우는 발생하게 된다.

https://developer.mozilla.org/en-US/docs/Glossary/Render_blocking
https://developer.mozilla.org/en-US/docs/Learn/Performance/CSS
https://developer.mozilla.org/en-US/docs/Learn/Performance/JavaScript

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

https://www.erwinhofman.com/blog/parsing-and-rendering-process-simplified/
https://ui.toast.com/fe-guide/ko_PERFORMANCE

# 브라우저와 네트워크
---
## 브라우저의 요청 과정 

네트워크 관점으로 브라우저의 요청 과정은 크게 다음과 같은 과정을 거친다.
- 입력한 URL 해석
- DNS를 통한 서버의 IP주소 확인
- 확인된 주소에 해당하는 서버에 TCP/TLS 연결 요청
- 서버에 연결 후 웹 페이지 데이터 요청 및 응답
- 브라우저 렌더링

### 1. URL 해석
URL(Uniform Resource Locator)은 웹에서 고유한 리소스의 주소다.
`https://d2.naver.com/helloworld/59361` 예시로 URL을 입력하고 엔터를 치면 URL을 해석하기 시작한다. 
주어진 URL을 해석하면 HTTPS 프로토콜을 이용하여 d2.naver.com에 해당하는 서버에 helloworld/59361 디렉토리의 리소스를 요청한다는 의미다. 추가로 뒤에 파라미터 p에 3232값을 할당하여 보낸다.

[[Browser Network#^08dbd0]]

### 2. DNS 조회
입력한 URL을 바탕으로 서버의 IP주소를 조회해야 하는데 이때 DNS조회를 사용한다.
DNS는 기억하기 어려운 IP 주소를 도메인 이름으로 저장한 장소다.
URL의 호스트명을 이용해서 DNS에 저장한 고유한 IP주소를 찾아낸다. 이때 DNS는 여러 DNS 서버를 찾을때까지 재귀적으로 조회한다. 찾은 IP주소를 웹 브라우저에 응답하여 보내면 웹 브라우저가 IP주소를 가지고 서버에 다시 요청한다.

[[TCP IP 4Layer Protocol#^608261]]

### 3. 브라우저와 서버의 TCP, TLS연결
DNS에서 응답받은 서버 IP주소를 통해 접근하게 된다. 이때 브라우저가 서버에 TCP 연결을 요청한다.여기서 HTTPS 프로토콜인 경우 추가로 TLS 핸드쉐이크 과정을 더 거친다.
[[TCP IP 4Layer Protocol#^35b272]]
[[TCP IP 4Layer Protocol#^cff23e]]
먼저 TCP 요청이 수락되면 바로 TLS 핸드쉐이크 과정을 거쳐 연결한다.
이후에는 별도의 핸드 쉐이크 과정없이 서로 공유한 대칭 키를 통해 효율적으로 메시지를 주고 받는다.

https://aws.amazon.com/ko/what-is/ssl-certificate/

### 4. HTTP 요청 및 응답
웹 브라우저와 서버의 연결이 완료되면 HTTP 요청을 통해 서버에 웹 페이지를 요청 한다.
- 요청 메서드
- 요청된 리소스를 가리키는 경로
- 통신할 HTTP 버전 등
그럼 서버가 해당 HTTP요청을 해석하고 정보를 기반으로 서버가 컨텐츠를 가져와 응답을 생성하여 다시 브라우저에 전송한다.
- 요청 상태
- 응답 처리 방법
- 요청 된 리소스

https://aws.amazon.com/ko/blogs/korea/what-happens-when-you-type-a-url-into-your-browser/