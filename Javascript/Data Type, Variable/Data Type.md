# 데이터 타입
---
데이터 타입은 데이터의 종류를 말하며 크게 **2가지**로 **원시형(primitive)**과 **참조형(reference)**으로 나뉜다.

## 원시형(Primitive Type)
원시형 데이터는 아래와 같이 7가지가 존재하며, 값이 **불변한다는 특징**이 있다.

- number
- string
- undefined
- null
- boolean
- symbol
- bigint

### truthy, falsy
자바스크립트는 **boolean 문맥**에서 **자동으로 형 변환**을 사용한다.
falsy값은 boolean 문맥에서 **false로 평가**되는 값을 말하며 아래와 같이 8가지가 있다.

- false
- 0
- -0
- 0n
- ""
- null
- undefined
- NaN
truthy값은 boolean 문맥에서 **true로 평가**되는 값을 말하며 **falsy값을 제외한 모든 값**을 말한다.

### undefined, null
undefined, null 모두 **값이 없음을 나타내는** 데이터다.
**차이점은** undefined의 경우 개발자가 직접 명시할 수 도 있지만 **자바스크립트 엔진이** 데이터가 없는 경우 **자동적으로 부여**하는 경우가 있다.
null은 자바스크립트 엔진이 **자동적으로 부여할 수 없다.**

따라서 두 경우를 구분하기 위해 데이터가 없음을 직접 명시하려는 경우 null을 사용하는게 낫다.

### Symbol
ES6에 도입된 데이터 타입으로 변경 불가능하고 다른 값과 중복되지 않는 유일한 값을 가지는 원시형 데이터다.

Symbol함수를 호출하여 데이터르 생성한다.
new 연산자와 함께 호출되지 않는다.

Symbol은 호출될 때마다 유일한 데이터 값을 가진다.
매개변수로 문자열을 전달할 수 있는데 이는 설명 용도로 쓰이며 이 문자열 값으로 구분되지 않는다.
```js
const symbol1 = Symbol('mySymbol');
const symbol2 = Symbol('mySymbol');

console.log(symbol1 !== symbol2); // true
```

Symbol('key')를 통해 객체 프로퍼티로 저장하더라도 값을 읽어 낼때 Symbol('key')호출 되는 순간 다른 데이터를 생성한다.
즉 전달되는 인수로 Symbol이 구별되지 않는다.
```js
const obj = {
	[Symbol('key')]: 'value'
}

obj[Symbol('key')]; // undefined
```

- 사용처
객체에서 충돌이 되지않는 유일한 프로퍼티를 만들 때 유용하다.
```js
const obj = {
	[Symbol('key')]: 'value1',
	[Symbol('key')]: 'value2'
};

// {
//	[Symbol('key')]: 'value1',
//	[Symbol('key')]: 'value2'
// }

const obj2 = {
	key: 'value1',
	key: 'value2'
};

// { key: 'value2' }
```
key는 같은 값으로 인식하여 중복된 키에대해 덮어쓴다.

일시적으로 동적 프로퍼티를 추가할 때 사용가능하다.
```js
Array.prototype[Symbol.for('sum')] = function () {
	...
}

[1, 2][Symbol.for('sum')]();
```
기존에 sum메서드의 내용을 건드리지 않고 변경할 수 있다.


- `Symbol.for/Symbol.keyFor`
Symbol메서드는 호출될 때마다 유일한 데이터를 생성한다. 실제로는 자바스크립트 엔진이 심벌 값 저장소인 전역 심벌 레지스트리에서 심벌 값을 검색할 수 있는 키를 지정하지 못해서다.
Symbol.for 메서드를 사용하면 전역 심벌 레지스트리에 검색을 통해 공유가 가능하다.
```js
const obj = {
	[Symbol.for('key')]: 1
};

obj[Symbor.for('key')]; // 1
```

Symbol.keyFor는 저장된 심벌 값의 키를 추출할 수 있다.
```js
const symbol = Symbol.for('key');
Symbol.keyFor(symbol); // key
```

- 심벌과 프로퍼티
for in문이나 Object.keys, Object.getOwnPropertyNames 메서드로 Symbol 프로퍼티 값을 조회할 수 없다.
하지만 getOwnPropertySymbols를 통해서 조회는 가능하다.

## 참조형(Reference Type)
참조형 데이터는 객체, 배열, 함수, 정규 표현식등 **모든 객체 데이터**를 의미 하며 데이터가 **가변하는 특징**을 갖는다.