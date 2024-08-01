# This
---
객체는 상태와 메서드를 가지는 논리적 단위인 자료구조로 표현할 수 있다. 이 객체를 구현할 때 메서드는 자신이 속한 객체의 프로퍼티를 참조하여 변경이 필요한데 이때 자신이 속한 객체 또는 생성할 인스턴스를 가리키는  자기 참조 변수를 this라고 한다.

this가 필요한 이유는 
- 일반적인 객체에서 자기 자신을 참조할 때 필요하다.
- 생성자 함수 또는 prototype 메서드에서 아직 생성되지 않은 인스턴스가 자신을 참조하기 위해서 필요하다

this는 자바스크립트 엔진에 의해 자동적으로 생성되어 함수 내부에 전달된다.

## 함수 호출 방식에 따른 this 바인딩
this가 가리키는 값, this 바인딩은 함수 호출 방식에 의해 동적으로 결정된다.
### 일반 함수 호출
일단 기본적으로 this는 전역 객체에 바인딩 된다.
일반 함수 호출에서 this는 전역 객체가 바인딩 되는데, 프로퍼티나 자기 참조 변수이기 때문에 일반 함수에서는 필요하지 않다.
애초에 전역 객체에 바인딩 되고 나서 객체의 프로퍼티로 호출 되지 않기 때문에 바인딩 되지 않고 전역 객체를 나타낸다.
```js
function foo() {
	console.log(this); // window
	function bar() {
		console.log(this); //window
	}

	bar();
}

foo();
```
함수가 중첩되어도 마찬가지다.

만약 strict mode가 적용 되면 전역 객체 대신 undefined이 바인딩 된다.
```js
function foo() {
	'use strict'
	
	console.log(this); // undefined
	function bar() {
		console.log(this); // undefined
	}

	bar();
}

foo();
```

메서드 내에서 호출될지라도 일반 함수로 호출 되면 여전히 전역 객체를 따른다.
```js
const obj = {
	value: 100,
	foo() {
		console.log(this); // { value: 100, foo: f}
		function bar() {
			console.log(this); // window
		}

		bar();
	}
}

obj.foo();
```

콜백 함수도 일반 함수로 호출되면 마찬가지다.
```js
var value = 1;

const obj = {
	value: 100,
	foo() {
		console.log(this); // { value: 100, foo: f}
		setTimeout(function () {
			console.log(this); // window
		}, 100)
	}
}

obj.foo();

```

정리하자면 최종적으로 일반 함수로 호출 되면 this에는 전역 객체가 바인딩 된다.

객체 내 중첩된 함수가 전역 객체를 바인딩 하는 것은 헬퍼 함수로서 동작하기 어렵기 때문에 문제가 있다.
메서드 내부의 일반 함수 호출 시 this를 객체로 바인딩 하는 방법이 있다.

메서드 내부에 this를 변수에 할당하고 쓰는 방법
```js
var value = 1;

const obj = {
	value: 100,
	foo() {
		console.log(this); // { value: 100, foo: f}

		const that = this;
		
		setTimeout(function () {
			console.log(that.value); // 100
		}, 100)
	}
}

obj.foo();
```

apply, call, bind를 통해서 명시하는 방법도 있다.

### 메서드 호출




### 생성자 함수 호출