# Class, Instance, Object
---
클래스(Class)는 어떤 사물이나 집단의 공통 속성을 모아 정의한 추상적인 개념이며, 객체를 구현할 설계도와 같은 개념이다. 
인스턴스(Instance)는 클래스의 속성을 지니는 클래스의 구체적인 사례이며 객체이다. 클래스의 사례를 나타내는 의미가 좀 더 크다.
객체(Object)는 구체적인 실체이며, 하나의 단위로 형식화 한 것이다. 인스턴스와 비슷하지만 하나의 실질적인 사례의 의미가 더 크다.

객체와 인스턴스의 개념이 비슷한데 인스턴스는 클래스가 만들어낸 구체적인 사례라는 의미에 더 가깝고, 객체는 그 사례이면서 하나의 구체적인 단위에 더 가깝다.
```js
class ModelY {};

const myCar = new ModelY();
```
여기서 ModelY가 클래스이고 new ModelY은 클래스의 인스턴스이며, myCar는 인스턴스이자 객체이다.

# Class
---
자바스크립트에서 클래스는 객체 지향 프로그래밍을 구현하게 하는 객체 생성 메커니즘과 비슷하지만, 실제로는 프로토타입 기반의 패턴을 클래스 처럼 사용할 수 있도록 하는 문법적 설탕(syntactic sugar)이다.
클래스로 인스턴스를 생성하는 측면에서는 기존의 객체 생성 메커니즘과 유사하게 동작한다.

생성자 함수와 클래스의 구성 방식을 비교하면 다음과 같다.
생성자
```js
function Person(name) {
	this.name = name;
}

constructor(name) {
	this.name = name;
}
```
프로토타입 메서드
```js
Person.prototype.sayHi = function () {
	console.log(this.name);
};

sayHi() {
	console.log(this.name);
}
```
정적 메서드
```js
Person.sayHello = function () {
	console.log('Hello');
};

static sayHell() {
	console.log('Hello');
}
```

```js
Class Person() {
	constructor(name) {
		this.name = name;
	}
	static sayHell() {
		console.log('Hello');
	}
	sayHi() {
		console.log(this.name);
	}
}
```

## 호이스팅
클래스는 함수로 평가된다.
```js
class Person {}
console.log(typeof Person); // function
```

함수이기 때문에 선언 이전에 참조가 가능한 호이스팅은 발생하지 않는다.
```js
console.log(Person); // Reference Error

class Person {}
```

## 메서드
### constructor
