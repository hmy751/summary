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

### BigInt

자바스크립트는 부동 소수점 방식을 따르며, 52비트의 가수 비트만으로 정수를 표현해야 하기 때문에 -2^53 + 1(-9,007,199,254,740,991)부터 2^53 - 1(9,007,199,254,740,991)까지 수를 표현할 수 있다. 이 범위 밖에서는 근사치로 표현을 하기 때문에 계산에 오차가 발생할 수 있다. 이를 해결하기 위해 BigInt를 활용한다.
BigInt를 사용하면 크기에 상관없이 정수의 손실 없이 표현할 수 있다.

```js
console.log(9007199254740991 + 1); // 9007199254740992 (정확)
console.log(9007199254740991 + 2); // 9007199254740992 (오차 발생)

const smallBigInt = BigInt(-9007199254740991);  // -2^53 + 1
console.log(smallBigInt); // -9007199254740991n, 정확히 표현 가능

const verySmallBigInt = BigInt(-1234567890123456789012345678901234567890);
console.log(verySmallBigInt); // 매우 작은 정수도 정확히 표현 가능
```




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

- NaN, isNaN()
먼저 NaN은 Not-A-Number로 숫자가 아님을 나타낸다. 

# 데이터 비교
---
## 동등, 일치 연산자

동등 연산자`==`는 타입이 아닌 값만 비교하는 연산자로 불리언 결과를 반환하며 1과 '1'을 같다고 판별한다.
일치 연산자`===`는 타입과 값을 모두 비교하는 연산자로 불리언 결과를 반환하며 타입과 값이 모두 같아야 같다고 판별한다.

https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Equality
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Strict_equality


## Object.is()

^ef3ef9
두 개의 인자를 비교하여 같은 값인지 판별하는 메서드이다.
`===` 연산자와 같지만 0, -0의 비교를 false로 처리하고, 서로 다른 NaN은 true 처리하여 좀 더 개발자가 기대하는 방식으로 비교처리를 한다.

Object.is는 `===`연산자의 약점을 보완하고자 하며 해결되지 않는 예외 케이스 0, -0 NaN을 다르게 처리한다.

#### `===`과 동작 방식 차이
자바스크립트의 일치 연산자는 비트 수준보다는 논리적 의미에 더 집중하여 값을 비교한다. 그래서 0, -0의 경우 부호를 제외한 0에 값의 의미 기준을 통해 비교하기 때문에 같다고 판별한다. 또 NaN의 경우 부정확하거나 잘못된 연산 결과를 표시하는 값이므로 서로 다를수 있기때문에 다르다고 판별한다.

Object.is는 비트 수준에서 값을 정밀하게 비교하기 때문에 0, -0의 경우 엄밀히 말해 부호는 다르므로 다른 값으로 판별하고 NaN의 경우 어쨋든 비트 수준 즉 값 자체는 같기 때문에 같다고 판별한다.

## 실수 표현

[Inpa 블로그, 부동 소수점 자료](https://inpa.tistory.com/entry/JAVA-%E2%98%95-%EC%8B%A4%EC%88%98-%ED%91%9C%ED%98%84%EB%B6%80%EB%8F%99-%EC%86%8C%EC%88%98%EC%A0%90-%EC%9B%90%EB%A6%AC-%ED%95%9C%EB%88%88%EC%97%90-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0)
컴퓨터 메모리는 2진수 체계를 따른다. 따라서 프로그래밍에서 사용되는 실수를 실제 메모리에 저장할 때는 2진수에 체계에 맞춰 저장해야 한다. 
만약 소수에서 2진수로 변환하게 되면 분모가 2의 거듭제곱으로 나눠 떨어지느 1/2, 1/4는 상관이 없지만,  0.1과 같이 나눠 떨어지지 않는 소수가 존재한다. 
자바스크립트는 부동 소수점 형식을 따르고 큰 수를 저장할 수 있는 방식이라고 하더라도 소수부분을 무한대로 저장할 수는 없기 때문에 어느 부분에서 끊어 반올림하여 근사치로 저장하게 된다.
그래서 일부 계산 결과에서 오차가 발생하게 된다.
```js
console.log(0.1 + 0.2); // 0.30000000000000004
```

오차 해결 방법은 
- toFixed를 활용하여 특정 자릿수로 반올림하는 방법이 있다.
```js
const result = Number((0.1 + 0.2).toFixed(10)); // 10자릿수로 반올림 console.log(result === 0.3); // true
```
- 오차를 허용하는 비교(Epsilon)
입실론을 사용하여 오차 허용치를 설정하고 두 수의 차이를 이 허용치보다 작으면 같다고 판별하는 방식이 있다.

```js
function isApproximatelyEqual(a, b, epsilon = 1e -10) {
	return Math.abs(a - b) < epsilon;
}

console.log(isApproximatelyEqual(0.1 + 0.2, 0.3)); // true
```

또는 Number.EPSILON을 활용하는 방법이 있다. 자바스크립트에서 표현할 수 있는 가장 작은 값을 의미한다.
```js
function isAlmostEqual(a, b, epsilon = Number.EPSILON) { 
	return Math.abs(a - b) < epsilon; 
} 

console.log(isAlmostEqual(0.1 + 0.2, 0.3)); // true
```
매우 작은값으로 정밀한 소수의 오차를 비교할 때는 좋지만 0.1이나 0.2처럼 상대적으로 큰 오차가 있는 경우에는 적합하지 않을 수 있다.

### 부동 소수점


> 부동 소수점, 고정 소수부
> 고정 소수부는 소수점의 위치가 고정된 방식이며, 부동 소수점은 소수점의 위치가 유동적이기 때문에 더 넓은 수 범위를 표현할 수 있다.



## 데이터 비교 특이 케이스

- NaN
NaN은 자기 자신과 같지않은 특이한 값으로 일치 연산자 비교에서도 false를 반환한다.
그래서 Object.is나 Number.isNaN 메서드를 활용해야 한다.
```js
console.log(NaN === NaN); // false
console.log(Object.is(NaN, NaN)); // true
console.log(Number.isNaN(NaN)); // true
```

- null, undefined
null과 undefined는 `==`비교시 같다고 평가되지만, `===`비교에서는 다르다고 판별된다.
```js
console.log(null == undefined); // true
console.log(null === undefined); // false
```

- +0, -0
0, -0은 일치 연산자는 같다고 판별하지만 Object.is는 다르다고 판별한다.
```js
console.log(Object.is(0, -0)); // false
```

- Infinity, -Infinity
```js
console.log(Infinity === Infinity); // true
console.log(Infinity === -Infinity); // false
```

- 0n, 0
BigInt는 일반 숫자는 같은 0일지라도 다른 타입으로 판별된다.
```js
console.log(0n === 0); // false
console.log(Object.is(0n, 0)); // false
```

- 1/0, 1/-0
두 값은 다르다고 판별된다. 왜냐하면 Infinity, -Infinity로 반환되고 값을 비교하기 때문에 다르다고 판별된다.
```js
console.log(1/0);  // Infinity
console.log(1/-0); // -Infinity
console.log(1/0 === 1/-0); // false
console.log(Object.is(1/0, 1/-0)); // false
```