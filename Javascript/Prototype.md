# Prototype과 인스턴스
---
프로토타입은 자바스크립트에서 상속을 구현하게 해주는 개념으로 프로토타입 객체와 프로토타입 체인으로 가능하게 한다.

먼저 생성자 함수에서 인스턴스를 생성하는 과정에서 prototype이 연결되는 과정을 설명하면
- new를 통해 생성자 함수를 호출하고
- Constructor에서 정의된 내용을 바탕으로 새로운 instance가 생성된다.
- instance에는 proto라는 프로퍼티가 부여되고
- 이 proto 프로퍼티는 Constructor의 객체인 prototype 프로퍼티를 참조하게 된다

이 과정을 통해 인스턴스는 Contructor의 prototype프로퍼티에 연결되어 메서드에 접근이 가능하게 된다.
마지막으로 메서드에 접근 후 this를 인스턴스에 바인딩이 되어야 인스턴스에서 해당 메서드를 활용이 가능하게 되는데 이는 인스턴스의 proto 프로퍼티의 생략으로 이러우지게 된다.

## Constructor 프로퍼티
생성자 함수의 prototype 객체에는 constructor 프로퍼티가 존재해 자기 자신을 명시하는데 이는 인스턴스가 proto프로퍼티를 통해서 생성자에 접근이 가능하도록 해준다.

constructor프로퍼티는 변경이 가능하다.

# Prototype 체인
---
## 메서드 오버라이드
만약 인스턴스에서 프로토타입과 같은 메서드를 생성하면 기존의 메서드는 유지한채 새로운 메서드를 참조하게 되는데 이를 메서드 오버라이드라고 한다.

메서드 오버라이드 상태에서 자바스크립트는 메서드를 조회할 때 가장 가까운 대상부터 찾아나간다.

## 프로토타입 체이닝
proto를 통해서 접근한 생성자 함수의 prototype은 객체이며 이 객체는 또 proto프로퍼티를 통해 객체의 prototype에 접근할 수 잇게된다.
프로토타입 체인은 proto속성을 통해 연쇄적으로 이어진것을 말한다.
프로토타입 체이닝은 이 프로토타입 체인을 통해 검색하는 것을 말한다.

모든 프로토타입 체이닝은 객체의 prototype 으로 이어지게 된다. 왜냐하면 prototype자체도 객체이며 이 객체의 proto는 객체의 prototype을 참조하기 때문이다.

## 상속 방법
Object.create
프로토타입 체인을 통해 


```js

function Person(name) {
	this.name = name;
}

const me = new Person('lee');

const parent = {
	sayHello() {
	}
}

Person.prototype = parent;

me.sayHello();

console.log(me.constructor === Person);


```
## 스태틱 메서드
스태틱 메서드는 생성자 함수 자신의 고유한 메서드로 
해당 인스턴스가 프로토타입 체인을 통해 접근하여 활용하여 생기는 오류를 방지하고자 활용한다.

예를 들어 모든 프로토타입 체인에는 객체 prototype이 연결 되는데 스태틱 메서드로 분리하지 않으면 다른 데이터 타입의 인스턴스에서 오용될 여지가 잇을수 있다.

스태틱 메서드는 프로토타입을 통해 접근하지 못하여 생성자 함수가 직접 인자로 호출하여 활용하는 형태이다.

# 프로토타입(Prototype)
---
프로토타입은 자바스크립트에서 객체 지향 프로그래밍에 필요한 상속을 구현하는 개념이다.
이 프로토타입을 통해서 객체간 상속이 이루어지며 객체 지향 프로그래밍에 메커니즘을 구현할 수 있게된다.

프로토타입을 이용하게 되면 상속을 구현할 수 있게 되고, 상속을 통해 불필요한 중복을 제거할 수 있어 리소스를 낭비하지 않을 수 있다.
```js
function Circle(radius) {
	this.radius = radius;
}

Circle.prototype.getArea = function () {
	return Math.PI * this.radius ** 2;
};

const circle1 = new Circle(1);
const circle2 = new Circle(2);

circle1.getArea();
circle2.getArea();
```
radius와 같이 다른 값을 지니는 상태는 프로퍼티로 부여하고, getArea와 같이 똑같은 메서드는 프로토타입을 통해 한 번만 생성하고 상속하여 리소스를 낭비하지 않을 수 있다.

이러한 프로토타입 상속을 구현하려면 prototype객체와 prototype의 연결이 필요하다.

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
--proto--프로퍼티는 객체가 직접 소유하는 프로퍼티가 아니라 Object.prototype의 프로퍼티다.
모든 객체는 상속을 통해서 --proto--에 접근하게 되는 것이다.

- --proto--로 접근하는 이유
`[Prototype]` 내부 슬롯으로 직접 접근하게 되면 순환 참조와 같은 에러를 막지 못하게 되며 오류를 발생시킬 수 있어 --proto--를 통해서 간접적으로 접근하게 된다.
아래 코드 처럼 --proto--를 통해서 순환 참조가 일어나게 되면 에러를 발생시켜 순환 참조를 방지한다.
```js
const parent = {};
const child = {};

child.__proto__ = parent;
parent.__proto__ = child; // TypeError: Cycle __proto__ value
```

- --proto--를 직접 사용하는 것은 권장되지 않는다.
--proto--는 Object.prototype의 프로퍼티다. 만약 Object.prototype와의 상속이 끊어지게 되면 해당 프로퍼티로 접근이 불가능하게 된다.
--proto--대신 getPrototypeOf를 통해서 prototype객체에 접근할 수 있다.

## 프로토타입 상속 과정과 시점
### 생성자 함수의 프로토타입 상속 과정과 생성 시점
생성자 함수는 다음의 과정을 거치면서 인스턴스와 함께 prototype의 상속이 이루어진다.
- 모든 함수는 생성 되자마자 prototype객체와 prototype프로퍼티, constructor프로퍼티를 갖는다.
- 이 프로퍼티와 객체를 통해서 생성자 함수와 prototype의 연결이 이루어진다.
- 생성자 함수를 new 연산자와 함께 호출한다.
- 해당 호출을 통해 인스턴스가 생성되며 해당 인스턴스에는 `__proto__`프로퍼티를 부여 받아 prototype에 접근이 가능해진다.
- prototype객체에 접근하여 constructor프로퍼티를 통해 생성자 함수를 참조하며 객체간 상속이 이루어진다.

정리하면 함수가 생성되는 시점에 prototype객체를 가지고, new와 함께 호출하는 시점에 인스턴스와 `__proto__`프로퍼티가 생성되며 상속이 이루어진다.

### 리터럴 표기법 방식의 상속과정
리터럴 표기법을 이용하면 new 연산자의 호출 없이 인스턴스를 생성한다.
해당 객체도 프로토타입을 가지며 constructor프로퍼티를 가진다. 차이점은 과정에 미묘한 차이가 있다.




# 객체 생성방식과 프로토타입
---
## 리터럴 방식