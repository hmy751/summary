# 프로토타입 구성요소 와 상속 과정
---
## 프로토타입 상속에 필요한 구성요소
prototype상속을 구현하려면 prototype객체, `__proto__`프로퍼티, prototype의 constructor프로퍼티 등이 필요하다.

prototype 객체는 프로토타입의 상속을 구현해주기 위해 사용되는 객체로서 모든 함수는 prototype객체를 가지며, 이를 참조하는 prototype프로퍼티를 가지게 된다.
또 이 prototype객체에는 consturctor프로퍼티가 존재하며 constructor프로퍼티는 자신의 생성자 함수를 참조하여 생성자 함수와 prototype의 연결이 이루어진다.
> [!NOTE]
> 정확히는 non-constructor인 화살표 함수나 축약 표현은 new를 통해 생성자 함수로 호출될 수 없어 prototype을 가지지 않는다.

그리고 모든 객체는 `[Prototype]`이라는 내부 슬롯을 가지는데,  이`[Prototype]`슬롯을 통해 자신이 상속받은 prototype객체를 바라본다.  `[Prototype]`내부 슬롯에 직접 접근할 수 는 없으며  `__proto__`프로퍼티를 통해서 prototype객체를 바라보는`[Prototype]` 내부 슬롯을 통해 prototype객체에 접근할 수 있게 된다.

### `__proto__` 프로퍼티
`__proto__`프로퍼티는 객체가 직접 소유하는 프로퍼티가 아니라 Object.prototype의 프로퍼티다.
모든 객체는 상속을 통해서 `__proto__`에 접근하게 되는 것이다.

- `__proto__`로 접근하는 이유
`[Prototype]` 내부 슬롯으로 직접 접근하게 되면 순환 참조와 같은 에러를 막지 못하게 되며 오류를 발생시킬 수 있어 `__proto__`를 통해서 간접적으로 접근하게 된다.
아래 코드 처럼 `__proto__`를 통해서 순환 참조가 일어나게 되면 에러를 발생시켜 순환 참조를 방지한다.
```js
const parent = {};
const child = {};

child.__proto__ = parent;
parent.__proto__ = child; // TypeError: Cycle __proto__ value
```

- `__proto__`를 직접 사용하는 것은 권장되지 않는다.
`__proto__`는 Object.prototype의 프로퍼티다. 만약 Object.prototype와의 상속이 끊어지게 되면 해당 프로퍼티로 접근이 불가능하게 된다.
`__proto__`대신 getPrototypeOf를 통해서 prototype객체에 접근할 수 있다.

## 프로토타입 상속 과정과 시점
### 생성자 함수의 프로토타입 상속 과정과 생성 시점
생성자 함수는 다음의 과정을 거치면서 인스턴스와 함께 prototype의 상속이 이루어진다.
- 모든 함수는 생성 되자마자 prototype객체와 prototype프로퍼티, constructor프로퍼티를 갖는다.
- 이 프로퍼티와 객체를 통해서 생성자 함수와 prototype의 연결이 이루어진다.
- 생성자 함수를 new 연산자와 함께 호출한다.
- 해당 호출을 통해 인스턴스가 생성되며 해당 인스턴스에는 `__proto__`프로퍼티를 부여 받아 prototype에 접근이 가능해진다.
- prototype객체에 접근하여 constructor프로퍼티를 통해 생성자 함수를 참조하며 객체간 상속이 이루어진다.

정리하면 함수가 생성되는 시점에 prototype객체를 가지고, new와 함께 호출하는 시점에 인스턴스와 `__proto__`프로퍼티가 생성되며 상속이 이루어진다.
또 모든 생성자 함수는 단독으로 존재하지 않고 반드시 prototype객체를 가지며 쌍으로 구성된다.

### 빌트인 생성자 함수의 프로토타입 생성 시점
Object, String, Number, Function 등 빌트인 생성자 함수도 함수 생성 시점에 프로토타입이 생성된다.
다만 시점은 전역 객체가 생성되는 시점에 모두 생성된다.

### 리터럴 표기법 방식의 상속과정
리터럴 표기법을 이용하면 new 연산자의 호출 없이 인스턴스를 생성한다.
해당 객체도 프로토타입을 가지며 constructor프로퍼티를 가진다. 차이점은 과정의 세부 내용에 차이가 있다.

new를 통해서 객체를 생성하면 내부에는 추상 연산 OrdinaryObjectCreate를 호출한다. 이때 인자에 따라 다른데 null이나 undefined이 전달되면 Object. prototype을 프로토타입으로 갖는 빈 객체를 생성하여 반환하게 된다.
인자를 넣게 되면 해당 인자를 객체로 변환한다.
```js
let obj = new Object();
// Object.prototype을 프로토타입으로 갖는 객체

obj = new Object(123);
// 숫자가 전달되면 Number객체를 생성한다.
```

만약 객체 리터롤 방식을 통해 객체를 생성하면 OrdianryObjectCreate를 호출하여 빈 객체를 생성하고 해당 객체는 Object.prototype의 상속이 되며 프로퍼티를 추가하게 된다.

둘 다 OrdinaryObjectCreate를 통해서 생성되나 new.target의 확인이나 프로퍼티를 추가하는 세부 내용에 차이점이 있다.

결론적으로는 두 방법 모두 같은 constructor를 가지게 된다.

# 프로토타입 체인
---
프로토타입 체인은 객체가 `__proto__`를 통해 연쇄적으로 이어진 것을 말한다. 
그리고 프로토타입 체이닝은 프로퍼티를 검색할 때 가장 가까운 해당 객체부터 프로토타입 체인을 통해 순차적으로 검색해 나가는 것을 말한다.

그리고 모든 prototype은 객체이므로 프로토타입 체인의 최상위 종점에는 언제나 Object.prototype이 된다.

프로토타입 체인은 상속과 프로퍼티 겁색을 위한 메커니즘이다. 
정리하면 프로토타입 객체와 프로토타입 체인을 통해서 자바스크립트에서 객체 지향 프로그래밍 메커니즘을 구현할 수 있게 된다.

## 메서드 오버라이딩, 프로퍼티 쉐도잉
인스턴스가 가지고 있는 프로퍼티를 인스턴스 프로퍼티, 프로토타입이 가지고 있는 프로퍼티를 프로토타입 프로퍼티라고 할 때 같은 이름의 프로퍼티가 프로토타입과 인스턴스에 모두 존재하면 인스턴스의 프로퍼티는 오버라딩하고 프로토타입 프로퍼티는 가려지는 프로퍼티 쉐도잉이라고 한다.
인스턴스에 같은 이름의 메서드를 추가하게 되면 프로토타입의 메서드가 교체되는 것이 아니라 덮어 씌워지게 된다.

## 프로토타입의 교체
프로토타입은 임의의 객체로 변경할 수 있다.

```js
function Person(name) {
	this.name = name;
}

Person.prototype = {
	sayHello() {
		console.log(this.name);
	}
}

const me = new Person('Lee');
```
이렇게 생성자 함수가 prototype을 직접 변경하게 되면 해당 객체는 constructor프로퍼티가 존재하지 않아 생성자 함수를 Person이 아닌 Object를 가리키게 된다.
물론 constructor를 지정하면 된다.
```js
Person.prototype = {
	constructor: Person,
	sayHello() {
		console.log(this.name);
	}
}```

setPrototypeOf메서드를 통해서도 변경이 가능하다
```js
function Person(name) {
	this.name = name;
}

const parent = {
	sayHello() {
		console.log(this.name);
	}
}

Object.setPrototypeOf(me, parent);

const me = new Person('Lee');

```
이렇게 되면 생성자 함수의 연결도 끊어지게 되고 Person 생성자 함수와 parent와의 prototype연결도 깨지게 된다.
연결을 하려면 constructor를 지정하여 연결할 수 있다.
```js
const parent = {
	constructor: Person,
	sayHello() {
		console.log(this.name);
	}
}

Person.prototype = parent;
```

정리하자면 지정을 통해서 다시 연결을 복구할 수 있지만 번거롭고 또 관계가 깨질 가능성이 커서 프로토타입을 직접 교체하는 것은 바람직하지 않다.
인위적으로 설정하려면 직접 상속을 통해서 하는것이 더 안전하고 편리하다.

## 직접 상속
직접 프로토타입을 교체하지 않고 직접 상속을 통해서 교체하는 것이 더 바람직하다.
방법에는 Object.create를 활용이 있다.
Object.create는 해당 인자를 prototype으로 갖는 객체를 반환한다.

```js
function Shape() {
	this.x = 0;
	this.y = 0;
}

Shape.prototype.move = function (x, y) {
	this.x += x;
	this.y += y;
};

function Rectangle(x) {
	Shape.call(this);
	this.x = x;
	this.y = x;
}

Rectangle.prototype = Object.create(Shape.prototype);
Rectangle.prototype.constructor = Rectangle;

var rect = new Rectangle();

console.log(rect instanceof Rectangle); // true
console.log(rect instanceof Shape); // true
rect.move(1, 1);

```
이렇게 하면 Rectangle은 Shape.prototype을 상속받게 된다. 다만 consturctor를 따로 명시하여 지정해줘야 연결이 끊기지 않게 된다.

그래서 상속 관계를 알아보기 위해서는 constructor 보다 instanceof를 사용하는게 더 낫다.

`__proto__`를 이용한 방법도 있지만 해당 프로퍼티는 사용자체를 권장하지는 않아 Object.create로 상속하는 방법이 더 낫다.

## 정적 메서드(스태틱 메서드)\

^972921

정적 메서드는 생성자 함수가 직접 소유하는 고유한 메서드를 의미하며, 인스턴스에서 접근하지 못하는 메서드를 말한다.

해당 메서드가 필요한 이유는 인스턴스가 프로토타입 체인을 통해 접근하여 활용 시 생기는 오류를 방지하기 위함이다.

예를 들어 프로토타입 체인 종점에는 Object.prototype으로 모든 데이터 타입에서 접근이 가능하게 된다. 만약 정적 메서드 없이 프로토타입을 통해 접근이 가능하다면 다른 데이터 타입에서 Object.create등을 활용할 수 있게 되며 오용될 여지가 있다.

정적 메서드의 활용은 생성자 함수가 직접 인자로 호출하여 활용하는 형태이다.

## 연산자
## instanceof
instanceof메서드는 생성자 함수를 찾는 것이 아니라 생성자 함수의 prototype에 바인딩된 객체가 프로토타입 체인 상에 존재하는지 확인한다.
그래서 상속 관계를 확인하기 위해 constructor 프로퍼티를 통해 확인하는 것 보다 instanceof를 사용하는게 더 낫다.
```js
function Person(name) {
	this.name = name;
}

const me = new Person("Lee");
const parent = {};

Object.setPrototypeOf(me, parent);

Person.prototype = parent;

console.log(me.constructor === Person); // true
console.log(me instanceof Person); // false

Person.constructor = Array;

console.log("Person constructor is Array =>", Person.constructor === Array); // true
console.log("Person instanceof Array is =>", Person instanceof Array); // false
```

### 프로퍼티 존재 확인
- in 연산자
in 연산자는 객체의 프로퍼티를 확인할 때 사용한다. 
다만 해당 객체의 프로퍼티 뿐만 아니라 상속받는 모든 프로토타입의 프로퍼티까지 존재한다고 판별한다.

- Object.prototype.hasOwnProperty
해당 객체가 직접 소유하는 프로퍼티만 확인하기 위해서 hasOwnProperty를 통해 확인할 수 있다. 상속받은 프로퍼티에 대해서는 false로 판별한다.

### 프로퍼티 열거
- for in 문
객체의 모든 프로퍼티를 순회하며 열거하는데 상속받은 프로퍼티까지 열거한다. 
그래서 객체 자신의 프로퍼티만 조회하려면 hasOwnProperty를 통해서 유효성검사를 해야한다.

- `Object.keys/values/entries` 메서드
객체 자신의 고유한 프로퍼티만을 활용하러면 `Object.keys/values/entries` 메서드를 활용하는 것이 낫다.