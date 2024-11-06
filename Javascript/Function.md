# First Class Object, First Class Function
---
일급 시민(first-class-citizen)이라는 용어는 프로그래밍 언어 디자인에서 특정 개체가 언어의 기능을 제한 없이 사용할 수 있음을 나타내는 용어이다.
일급 객체는 프로그래밍에서 다음과 같은 특징을 가지는 객체를 말한다.
- 변수에 담을 수 있다.
- 함수의 매개변수로 전달할 수 있다.
- 함수의 반환 값으로 사용할 수 있다.
- 동적으로 프로퍼티 생성이 가능하다.

일급 함수는 일급 객체와 같은 4가지 특징을 가지는 함수다.
- 변수나 자료구조에 담을 수 있다.
- 함수의 매개변수로 전달할 수 있다.
- 함수의 반환 값으로 사용할 수 있다.
- 무명으로 런타임에 생성이 가능하다.

```js
// 무명으로 런타임에 생성이 가능하며, 변수에 담길 수 있다.
const increase = function (num) {
	return ++num;
}

const decrease = function (num) {
	return --num;
}

// 객체에 담길 수 있다.
const aux = { increase, decrease };

// 반환 값으로 사용할 수 있다.
function makeCounter(aux) {
	let num = 0;

	return function () {
		num = aux(num);
		return num;
	}
}
```

자바스크립트의 함수는 일급 함수이며 해당 특징 덕분에
- 콜백 함수
- 클로저
- 고차 함수
등이 가능해진다.

# Callback Function
---
콜백 함수는 다른 함수의 매개변수로 전달되며 **제어권**이 함께 위임된 함수를 말한다.

## 제어권
전달되는 제어권에는 호출 시점, 인자, this등이 있다.

- 인자
콜백함수를 넘겨 받은 함수가 인자에 대해 지정을 해놓으면, 콜백 함수 자체에서 네이밍을 바꿔도 넘겨 받은 함수를 기준으로 결정 된다.
```js
Array.prototype.map(callback[, thisArg]);
callback: function (currentValue, index, array);

const arr = [1, 2, 3].map(function (index, value) {
	console.log(value, index);
});
```

- 호출 시점
호출 시점은 넘겨 받은 함수가 결정을 하는데 예를 들어 타이머 함수인 setTimeout에서 콜백 함수의 호출 시점을 결정할 수 있다.

- this
this의 경우도 넘겨 받은 함수가 결정하게 된다. map의 경우 내부에서 콜백 함수의 this바인딩을 처리하게 된다.
```js

Array.prototype.map = function (callback, thisArg) {
	var mappedArr = []

	for (var i = 0; i < this.length; i++) {
		var newValue = callback.call(thisArg || window, this[i], i, thisArg || this);
		mappedArr[i] = newValue
    }

	return mappedArr
}

```

## 콜백 지옥
콜백 함수를 통해서 비동기 처리를 하게 되면 함수가 중첩되어 코드의 복잡도가 높아져 가독성도 떨어지고 오류를 발생시킬 가능성이 높아진다.

이를 해결하기 위해 프로미스등을 이용하여 해결할 수 있다.

# 클로저
---
클로저란 중첩된 함수에서 상위 함수의 변수를 참조하는 내부함수가 있고 이 내부함수가 외부로 전달되어질 때, 외부 함수의 실행 컨텍스트가 종료된 이후에도 변수를 계속 참조하는 현상을 말한다.

```js
var outer = function () {
  var a = 1;

  var inner = function () {
    return ++a;
  };

  return inner;
};

var innerFunc = outer();

innerFunc(); // 2
innerFunc(); // 3
```

클로저가 발생하는 이유는 함수의 실행 컨텍스트가 종료돼 콜 스택에서 제거 되어도 변수가 계속 참조되어지고 있는 상황이면 가비지 컬렉터 해당 렉시컬 환경을 수거하지 않기 때문이다.

## 클로저와 렉시컬 환경
클로저가 발생할 때 상위 스코프를 기억하고 유지하는 이유는 렉시컬 환경(LexicalEnvironment) 때문이다.
```js
var outer = function () {
  var a = 1;

  var inner = function () {
    return ++a;
  };

  return inner;
};

var innerFunc = outer();

innerFunc(); // 2
innerFunc(); // 3
```
![[Javascript/Function.excalidraw.md#^group=OMG9-Him3GJj65Mab-hzw|1200]]
코드를 예로들면 outer 즉 중첩 함수 바깥의 함수가 종료돼 실행 컨텍스트가 팝되지만 해당 렉시컬 환경은 수집되지 않는다.
왜냐하면 해당 렉시컬 환경이 inner함수의 `[Environment]`슬롯에 의해 참조되고, 또 innerFunc변수에 할당되어 참조 되고 있는 상황이기 때문에 가비지 컬렉터가 이를 수거하지 않는다.
그래서 할당된 innerFunc를 이후에 실행하게 되면 inner함수의 실행 컨텍스트가 생성되어 실행되고 inner 함수의 `[Environment]`슬롯은 여전히 상위 스코프를 기억하고 있기 때문에 스코프 체인을 따라 상위 변수에 접근하게 되며, 또 변경까지 가능하게 된다.

## 클로저의 이점과 단점
클로저는 주로 상태를 안전하게 보호하며 변경하고 유지하기 위해 사용 된다. 특정 함수에게만 변경을 허용한다.
물론 상위 스코프에 변수를 선언하여 참조해도 되지만 해당 변수가 외부에 노출되어 보호받지 못하게 된다.
클로저의 단점은 너무 많이 남용하게 되면 불필요한 메모리를 낭비하는 점이다. 왜냐하면 참조 되어있으면 메모리를 수거하지 않기 때문이다.

### 클로저의 활용
- 모듈 패턴
```js
var Counter = (function () {
  var num = 0;

  return {
    increase() {
      return ++num;
    },
    decrease() {
      return --num;
    },
  };
})();

Counter.num; // undefined;

Counter.increase(); // 1
Counter.increase(); // 2
Counter.decrease(); // 1
```
클로저를 통해서 num이라는 변수의 접근은 제한하면서, 값은 유지하고 변경이 가능해진다.


- 부분 적용 함수
부분 적용 함수는 총 n개의 인자를 받는 함수에 미리 m개의 인자만 넘겨 받아 기억시켰다가 나머지 인자를 받고 실행하는 함수 이다.
즉 미리 필요한 인자들을 채워놓는 함수를 만들어 놓다가 필요한 인자를 받아 실행하는 함수다.
```js
const partial = (a, b, c) => {
	return a + b + c;
}

const add5 = () => partial(5);

console.log(add5(6, 7)); // 18
```

- 커링 함수
커링 함수는 여러개의 인자를 받는 함수를 하나의 인자만 받는 함수로 나눠서 순차적으로 호출되는 형태로 구현한 함수다.
부분 적용 함수와 다른점은 하나의 인자만 받아 차례로 처리한 후 다음 인자를 받아 처리하는 형태다.
```js
const curry = (a) => (b) => a + b;

const add5 = curry(5);

add5(10); // 15
```
