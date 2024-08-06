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
constructor메서드는 클래스의 인스턴스를 생성하고 초기화 하는 특수한 메서드다. 일반적으로 함수의 prototype객체의 constructor 프로퍼티와는 다른 개념이다.
클래스를 호출하더라도 prototype객체와 constructor프로퍼티가 존재는 하지만 클래스 내부에 사용하는 constructor메서드는 특수한 개념으로 클래스의 프로퍼티 자체에 존재하지 않으며, 함수가 생성되면 부여되는 constrctor 프로퍼티와는 역할이 다르다.
인스턴스에 부여할 프로퍼티 생성 및 초기화에 관여한다.

### 프로토타입
클래스 자체에 정의한 메서드는 기본적으로 프로토타입 메서드가 된다.
```js
class Person {
	sayHi() {
	}
}

console.log(new Person());
```
![[Javascript/Class.excalidraw.md#^group=Q29TVmAmSBOt5k58r3UHy]]

클래스는 생성자 함수와 같은 방식으로 인스턴스를 생상한다.

### 정적 메서드
정적 메서드는 인스턴스가 직접 접근할 수 없는 생성자 함수의 메서드이다. 인스턴스가 접근하여 예기치 못한 오류를 발생하고자 활용한다.
[[Prototype#^972921]]
클래스도 정적 메서드를 가지는데 static 키워드를 붙여서 구현한다.
```js
Person.sayHello = function () {
	console.log('Hello');
};

static sayHell() {
	console.log('Hello');
}
```

클래스에서 직접 스태틱 메서드를 활용할 수 있다.
```js
Person.sayHi();
```

정적 메서드는 인스턴스와 상관 없는 전역 환경에서 사용할 유틸리티 메서드를 사용하는데 활용될 수 있다. 
```js
Math.max(1, 2, 3);
Object.is({}, {});
```
이렇게 활용하면 생성자 함수를 네임스페이스로 하고 프로퍼티 충돌 가능성을 줄여주고, 관련 함수들을 구조화할 수 있어 좋다.
이렇게 하면 전역 함수를 정의하지 않고도 활용이 가능하다.

## 프로퍼티
### 접근자 프로퍼티(getter, setter)
getter, setter는 접근자 프로퍼티로 자체적으로는 값을 가지고 있지 않다. 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 사용하는 접근자 함수로 구성된 프로퍼티다.
일반 객체 뿐만 아니라 클래스에서도 사용이 가능하다.
```js
class Person {
	constructor(firstName, lastName) {
		this.firstName = firstName;
		this.lastName = lastName;
	}

	get fullName() {
		return `${this.firstName}${this.lastName}`;
	}

	set fullName(name) {
		[this.firstName, this.lastName] = name.split(' ');
	}
}

cosnt me = new Person();

me.fullName = '명연 함'
console.log(me.fullName); // 함명연
```
접근자 프로퍼티를 사용하면 호출하는 것이 아니라 프로퍼티처럼 참조 형식으로 사용하며 내부적으로 getter, setter가 호출 되는 형태이다.
그래서 내부 구현을 숨길 수 있어 외부에서 객체의 상태를 임의로 변경하거나 접근하는 것을 막을 수 있다.

## 클래스 상속
### 클래스 상속과 생성자 함수 상속의 차이
클래스 상속은 기존 클래스를 상속받아 새로운 클래스를 확장한다.
프토타입체인과 차이점은 프로토타입은 프로토타입만을 연결시켜 상속 받는데 클래스는 프로토타입 뿐만 아니라 클래스 자체도 상속을 구현한다.
![[Javascript/Class.excalidraw.md#^group=YlBVqhUm0RJBoc9U9-95q|1200]]
또 클래스 상속은 코드 재사용 관점에서 유용하다.

### 키워드
#### super
super를 호출하면 수퍼 클래스(super class)의 constructor를 호출한다.
수퍼 클래스의 프로퍼티를 그대로 갖는 서브 클래스를 가질수 있다.
constructor를 생략하면 암묵적으로 super호출을 통해 서브 클래스로 프로퍼티가 전달된다.
만약 constructor를 호출하면 반드시 내부에 super를 호출해야 한다.
```js
class Derived extends Base {
	constructor(a, b, c) {
		super(a, b);
		this.c = c;
	}
}

class Derived extends Base {
	constructor(a, b, c) {
		// ReferenceERror, constructor를 호출하면
		// 반드시 super를 호출해야 한다.
		this.c = c;
	}
}

```

반드시 super를 호출하고 프로퍼티를 선언해야 한다.
```js
class Derived extends Base {
	constructor(a, b, c) {
		this.c = c;
		super();
	}
}
```

super 참조를 통해서 수퍼클래스의 메서드를 참조할 수 있다.
```js
class Base {
	constructor(name) {
		this.name = name;
	}

	sayHi() {
		return `Hi ${this.name}`;
	}
}

class Derived extends Base {
	sayHi() {
		return `${super.sayHi()}`;
	}
}
```