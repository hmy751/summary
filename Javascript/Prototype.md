프로토타입은 자바스크립트에서 상속을 구현하게 해주는 개념으로 프로토타입 객체와 프로토타입 체인으로 가능하게 한다.

# Prototype과 인스턴스
---
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

function Human(name) {
	this.name = name;
	
}

Human.prototype.getName = () => {
	console.log('getName');
}

function Student(number) {
	this.number = number;
}

Student.prototype = Human.prototype;
    
const john = new Student(1);

Student.prototype.getName = () => {
    console.log('changed method');
}

const human = new Human('human');

human.getName();



```
## 스태틱 메서드
스태틱 메서드는 생성자 함수 자신의 고유한 메서드로 
해당 인스턴스가 프로토타입 체인을 통해 접근하여 활용하여 생기는 오류를 방지하고자 활용한다.

예를 들어 모든 프로토타입 체인에는 객체 prototype이 연결 되는데 스태틱 메서드로 분리하지 않으면 다른 데이터 타입의 인스턴스에서 오용될 여지가 잇을수 있다.

스태틱 메서드는 프로토타입을 통해 접근하지 못하여 생성자 함수가 직접 인자로 호출하여 활용하는 형태이다.
