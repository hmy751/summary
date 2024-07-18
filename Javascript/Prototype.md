프로토타입은 자바스크립트에서 상속을 구현하게 해주는 개념으로 프로토타입 객체와 프로토타입 체인으로 가능하게 한다.

# prototype의 구성요소와 연결

먼저 상속하는 과정을 설명하자면
생성자 함수에서 인스턴스를 생성 하면서 상속이 된다.
- new를 통해 생성자 함수를 호출하고
- Constructor에서 정의된 내용을 바탕으로 새로운 instance가 생성된다.
- instance에는 proto라는 프로퍼티가 부여되고
- 이 프로퍼티는 Constructor의 객체인 prototype 프로퍼티를 참조하게 된다

상속에 더해 인스턴스에서 직접 prototype의 프로퍼티에 접근하여 사용하려면 
- proto의 prototype참조, 
- 인스턴스의 proto 생략 가능으로 this 바인딩
등을 통해 이루어진다.

# Constructor 프로퍼티
생성자 함수의 prototype 객체에는 Contructor 프로퍼티가 존재하여 construcor에 접근할 수 있게된다

그리고 constructor는 변경이 가능하다


# 프로토 타입 체인
## 메서드 오버라이드
만약 인스턴스에서 프로토타입과 같은 메서드를 생성하면 기존의 메서드는 유지한채 새로운 메서드를 참조하게 되는데 이를 메서드 오버라이드라고 한다.

## 프로토타입 체인
proto를 통해서 접근한 생성자 함수의 prototype은 객체이며 이 객체는 또 proto프로퍼티를 통해 객체의 prototype에 접근할 수 잇게된다.
프로토타입 체인은 proto속성을 통해 연쇄적으로 이어진것을 말한다.
프로토타입 체이닝은 이 프로토타입 체인을 통해 검색하는 것을 말한다.

모든 프로토타입 체이닝은 객체의 prototype 으로 이어지게 된다. 왜냐하면 prototype자체도 객체이며 이 객체의 proto는 객체의 prototype을 참조하기 때문이다.

## 스태틱 메서드
스태틱 메서드는 생성자 함수 자신의 고유한 메서드로 
해당 인스턴스가 프로토타입 체인을 통해 접근하여 활용하여 생기는 오류를 방지하고자 활용한다.

예를 들어 모든 프로토타입 체인에는 객체 prototype이 연결 되는데 스태틱 메서드로 분리하지 않으면 다른 데이터 타입의 인스턴스에서 오용될 여지가 잇을수 있다.

스태틱 메서드는 프로토타입을 통해 접근하지 못하여 생성자 함수가 직접 인자로 호출하여 활용하는 형태이다.

Object.create

function (name) {
	this.name = name
}

function Student(number) {
	this.number = number;
}

Student.prototype = Object.create(Human.prototype);

const john = new Student(1);


# 프로토 타입 연결 과정
