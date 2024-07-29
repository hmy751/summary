# First Class Object, First Class Function
---
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

콜백함수를 넘겨 받은 함수가 인자에 대해 지정을 해놓으면, 콜백 함수 자체에서 네이밍을 바꿔도 넘겨 받은 함수를 기준으로 결정 된다.
```js
Array.prototype.map(callback[, thisArg]);
callback: function (currentValue, index, array);

const arr = [1, 2, 3].map(function (index, value) {
	console.log(value, index);
});
```

호출 시점은 넘겨 받은 함수가 결정을 하는데 예를 들어 타이머 함수인 setTimeout에서 콜백 함수의 호출 시점을 결정한다.

this의 경우도 넘겨 받은 함수가 결정하게 된다. map의 경우 내부에서 콜백 함수의 this바인딩을 처리하게 된다.
```js

Array.prototype.map = function (callback, thisArg) {

var mappedArr = []

  

for (var i = 0; i < this.length; i++) {

var newValue = callback.call(thisArg || window, this[i], i, thisArg || this)

  

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

var outer1 = outer();

outer1(); // 2
outer1(); // 3
```

클로저가 발생하는 이유는 함수의 실행 컨텍스트가 종료돼 콜 스택에서 제거 되어도 변수가 계속 참조되어지고 있는 상황이면 가비지 컬렉터 해당 렉시컬 환경을 수거하지 않기 때문이다.

## 클로저의 이점
	

## 클로저 활용

클로저는 함수에서 변수를 참조하여 유지하며 사용할 때 유용하다. 전역 변수 같이 해당 함수의 상위 스코프에 변수를 유지할 수 있지만, 이렇게 하면 변수가 해당 함수 뿐만 아니라 다른 공간에도 노출이 되어 불변성이 보장되지 않을 수 있다.
클로저를 이용하면 해당 함수 스코프내에 지역 변수로 남아, 해당 변수를 유지하며 사용하는 것 뿐만 아니라 변수의 안정성도 높일 수 있다.

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

클로저를 통해서 num이라는 변수는 보호 하면서, 값은 유지하면서 활용이 가능하다.
