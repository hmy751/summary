# 데이터 타입의 종류
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

Symbol함수를 호출하여 데이터를 생성한다. new 연산자와는 함께 호출되지 않는다.

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

#### 사용하는 경우
- 객체에서 충돌이 되지않는 유일한 프로퍼티를 만들 때 유용하다.
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

- 프로퍼티를 보호하고자 할때 사용된다.
서드파티의 객체 메서드나 빌트인 객체의 프로토타입 메서드 등을 임시로 변경하고자 할때 해당 메서드를 변경하게 되면 다른 곳에 영향을 줄 가능성이 있다.
이를 방지하기 위해 심벌을 이용하면 해당 이름을 활용하여 구분하면서 유일한 프로퍼티를 생성하여 충돌을 피하며 기존 프로퍼티 메서드도 영향을 주지 않을 수 있다.
```js
Array.prototype[Symbol.for('sum')] = function () {
	...
}

[1, 2][Symbol.for('sum')]();
```

https://pozafly.github.io/javascript/symbol/

- 유일한 상수 값을 사용하려고 할때
```js
const COLOR_BLUE = 'blue';

const MOOD_BLUE = 'blue';
```
두 개의 상수는 다르지만 같은 값을 가진다.
이 때 구분되지 않는데 심벌을 통해 구분할 수 있다.

```js
const COLOR_BLUE = Symbol();
const MOOD_BLUE = Symbol();

```

#### `Symbol.for/Symbol.keyFor`
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

#### 심벌과 프로퍼티
심벌 값으로 프로퍼티를 생성하고 접근하여 사용하려면 Symbol.for메서드를 활용하면 된다.
```js
const obj = {
	[Symbol.for('key')]: 1
}

obj[Symbol.for('key')]; // 1
```
이렇게 심벌을 프로퍼티로 활용하면 다른 프로퍼티와 겹치지 않는 유일한 프로퍼티가 된다.

for in문이나 Object.keys, Object.getOwnPropertyNames 메서드로 Symbol 프로퍼티 값을 조회할 수 없다.
```js
const obj = {
 [Symbol('key')]: 1
}

for (const key in obj) {
	console.log(key); // 출력되지 않는다.
}
```
하지만 getOwnPropertySymbols를 통해서 조회는 가능하다.


#### Well-known Symbol
자바스크립트가 기본 제공하는 빌트인 심벌 값이다. 빌트인 심벌 값은 함수의 프로퍼티에 할당 되어 잇다.
기본 제공하는 빌트인 심벌 값을 Well-known Symbol이라고 한다. ^676c3f

대표적으로 Symbol.iterator가 있다. 
이는 이터러블 객체를 만드는데 활용된다. 이 값들은 Symbol이기 때문에 다른 프로퍼티와 절대로 중복되지 않을 것 이다.

## 참조형(Reference Type)
참조형 데이터는 객체, 배열, 함수, 정규 표현식등 **모든 객체 데이터**를 의미 하며 데이터가 **가변하는 특징**을 갖는다.

# 데이터 타입 확인하기
---
typeof 연산자는 평가 받을 데이터의 자료형을 문자열로 반환한다.

- 참조형 데이터
먼저 대부분의 참조형 데이터는 'object'를 반환한다.
```js
typeof [1, 2, 4] === 'object';
typeof { a: 1 } === 'object';
typeof new Boolean(true) === 'object';
typeof new Date() === 'object';
```

null은 원시형이지만  'object'를 반환한다.
```js
typeof null === 'object';
```

함수는 function을 반환한다.
```js
typeof function () {} === 'function'
typeof class C {} === 'function'
typeof Math.sin === 'function'
```

배열을 확인할 때 typeof값은 object를 반환한다. 만약 배열임을 확인하고 싶다면 Array.isArray메서드로 확인이 가능하다.
```js
Array.isArray([1, 2, 3]) // true
```

- 원시형 데이터
이외의 데이터들은 대부분 원시형으로 자신의 데이터 타입을 반환한다.
```js
typeof "" === 'string';

typeof 1 === 'number';
typeof NaN === 'number';
typeof Infinity === 'number';

typeof 42n === 'bigint';

typeof true === 'boolean';

typeof Symbol() === 'symbol';
typeof Symbol('foo') === 'symbol';
typeof Symbol.iterator === 'symbol';

typeof undefined === 'undefined';
```