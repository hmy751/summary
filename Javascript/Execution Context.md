# 실행 컨텍스트
---
실행 컨텍스트는 실행할 코드에 제공할 환경 정보들을 모아놓은 객체로, 소스코드가 실행하는 데 필요한 환경을 제공하고 코드의 실행 결과를 실제로 관리한다.

처음 코드가 실행될 때, 전역 공간에 대한 실행 컨텍스트가 생성된다. 이후 함수가 호출될 때마다, 해당 함수에 대한 실행 컨텍스트가 평가 시점에 생성되고, 함수의 변수와 식별자 등의 정보를 수집한다.

평가를 마친 실행 컨텍스트는 콜 스택에 담기고, 이 콜 스택은 가장 최신의 자료가 맨 위로 쌓이는 스택 자료구조로 동작한다. 
코드를 실행하는 과정은 크게 다음과 같다.
평가 및 컨텍스트 생성 => 콜 스택 추가 => 콜 스택 최상단 기준 실행 => 실행 종료(콜 스택에서 팝) 및 재개

1. **전역 컨텍스트 생성**: 코드를 처음 실행하면 전역 공간에 대한 실행 컨텍스트가 생성된다.
2. **평가 및 컨텍스트 생성**: 함수 호출부를 만나면 해당 함수에 대한 실행 컨텍스트가 생성되고, 변수와 식별자 정보를 수집한다.
3. **콜 스택에 추가**: 평가를 마친 실행 컨텍스트는 콜 스택에 담기며, 실행을 시작한다.
4. **콜 스택 최상단 기준 실행**: 실행은 콜 스택의 최상단에 있는 실행 컨텍스트를 기준으로 진행된다.
5. **일시중단 및 재개**: 새로운 함수 호출을 만나면 해당 함수의 실행 컨텍스트가 생성되어 콜 스택에 추가되고, 이전 컨텍스트의 실행은 일시 중단된다. 새로운 컨텍스트의 실행이 끝나면 콜 스택에서 제거되고, 이전 컨텍스트의 실행이 재개된다.

이 과정을 반복하면서 코드가 실행됩니다.

정리하면 식별자, 스코프, 코드의 실행 순서등을 실행 컨텍스트를 통해 관리된다.

# 실행 컨텍스트의 구성
---
실행 컨텍스트의 구성은 아래와 같이 3가지로 구성되어 있다.
- VariableEnvironment
  - environmentRecord
  - outerEnvironmentReference
- LexicalEnvironment
  - environmentRecord
  - outerEnvironmentReference
- ThisBinding

- VariableEnvironment
VariableEnvironment는 실행 컨테스트를 생성할 때 만들어지며 스냅샷의 목적으로 변경되지 않고 유지 된다.
VariableEnvironment는 생성된 후 바로 복사하여 LexicalEnvironment를 만든다.

- LexicalEnvironment
LexicalEnvironment는 실행 컨텍스트 내부의 변경 사항이 실시간으로 반영되며 실질적으로 활용되는 객체이다.
환경정보에는 environmentRecord와 outerEnvironmentReference로 구성되어 있다.

- ThisBinding
실행 컨텍스트의 ThisBinding에는 this로 지정된 객체가 저장된다. 실행 컨텍스트 활성화 당시 this가 지정되지 않은 경우 전역 객체가 저장된다.

## environmentRecord, 호이스팅
environmentRecord에는 현재 컨텍스트와 관련된 코드의 식별자 정보들이 저장된다. 이 정보는 컨텍스트 내부의 코드를 처음 부터 끝까지 순서대로 수집하여 저장한다.

실행 컨텍스트는 코드의 실행 전 평가 과정에서 생성되며, 이때 코드를 분석하여 식별자 정보를 environmentRecord에 저장하게 된다. 이 과정에서 실행 전에 모든 식별자 정보를 알게 되며, 이를 통해 호이스팅 현상이 발생한다.

호이스팅은 변수가 선언된 위치와 상관없이 이전에 실행될 코드 위에서 먼저 참조되어 마치 선언된 변수가 상단으로 끌어올려진 것 처럼 해석되는 현상을 말한다.
```js
...

function inner() {
	console.log(a);
	var a = 2;
}

...
```

![[Javascript/Execution Context.excalidraw.md#^group=skQyJrZ9Mq2cwM4b4iwts|1200]]

## outerEnvironmentReference, 스코프 체인
outerEnvironmentReference는 현재 컨텍스트의 함수가 선언되는 시점의 컨텍스트에서 LexicalEnvironment를 담는다. 
정확히 outerEnvironmentReference는 해당 함수 객체 내부 슬롯인`[Environment]`을 통해 함수가 평가되는 시점에 LexicalEnvironment를 참조하여 접근하게 된다.
따라서 현재 컨텍스트 뿐만 아니라 선언되는 시점에, 바깥의 컨텍스트의 식별자 정보를 참조할 수 있게 된다.

> [!NOTE]
> - `[Environment]`슬롯
> 함수가 정의 되고 평가되는 시점에 자신이 정의된 위치를 기준으로 **상위 스코프**의 **렉시컬 환경(LexicalEnvironment)**을 저장하는 내부 슬롯이다.
> `[Environment]`는 실행이 아닌 평가되는 시점을 기준으로 상위 스코프 참조를 결정하므로 정적 스코프 즉 선언 기준으로 스코프가 형성된다.
> 따라서 자바스크립트는 정적 스코프인 렉시컬 스코프를 따르게 된다.
[[Scope#^007731]]
> 

```js
var a = 1;

function outer() {
	function inner() {
		console.log(a);
		var a = 2;
	}

	inner();
	console.log(a);
}

outer();
```
![[Javascript/Execution Context.excalidraw.md#^group=zXnZ0aOzlTrm9zzjemB_S|1200]]
이러한 outerEnvironmentReference의 특성으로 스코프 체인 현상이 발생한다.
스코프 체인은 현재 스코프 뿐만 아니라 바깥의 컨텍스트로 이동하여 차례로 식별자를 검색해 나가는 것을 말한다.
해당 코드에서는 참조하고자 하는 식별자가 없어 outerEnvironmentReference를 통해서 상위 스코프의 LexicalEnvironment를 통해 찾아나간다.

이로써 자바스크립트는 스코프 체인이라는 **식별자 검색을 위한 메커니즘**을 통해 식별자를 참조해 나간다.

# 실행 컨텍스트 생성 과정
---
```js
var a = 1;

function outer() {
	function inner() {
		console.log(a);
		var a = 2;
	}

	inner();
	console.log(a);
}

outer();
console.log(a);

```
![[Javascript/Execution Context.excalidraw.md#^group=BidAZgKtaITlvPNsYGzFt|1200]]

