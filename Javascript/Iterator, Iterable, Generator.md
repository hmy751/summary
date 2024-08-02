# Iterator, Iterable
---
## Iterator
이터레이터(Iterator) 프로토콜은 next 메서드를 소유하고 이 next 메서드에서 호출 시 이터러블 객체를 순회하여 { value: any, done: boolean }의 형태인 이터레이터 result 객체를 반환하는 규칙이다.
이 이터레이터 프로토콜을 만족하는 객체를 이터레이터 객체라고 한다.
이터레이터는 Symbol.iterator 메서드를 통해 반환 되며 next 메서드를 가진다.
[[Data Type#^676c3f]]
![[Javascript/Iterator, Iterable, Generator.excalidraw.md#^area=Ovv8RNkLM25mVvPlQ2eXe|1200]]

## Iterable
이터러블(Iterator) 프로토콜은 호출 시 이터레이터 객체를 반환하는 Symbol.iterator 메서드를 가지는 규칙이다. 
이 이터러블 프로토콜을 지키는 객체를 이터러블 객체라고 한다. 그래서 이터러블을 만족하게 되면 반복 가능하게 되며 for of문을 통해 조회가 가능해진다.
![[Javascript/Iterator, Iterable, Generator.excalidraw.md#^group=Ovv8RNkLM25mVvPlQ2eXe|1200]]

```js
const range = {
	from: 1,
	to: 5
};

range[Symbol.iterator] = function () {
	return {
		current: this.from,
		last: this.to,
		next() {
			if (this.current <= this.last) {
				return { done: fasle, value: this.current++ };
			}
			return { done: true }
		}
	}
}

```

이터러블을 만족하면 스프레드 문법과 배열 디스트럭처링이 가능하다. 
```js
const obj = { a: 1, b: 2};

for (const item of obj) {
	console.log(item);
}

const [a, b] = obj;
```
일반 객체는 이터러블을 만족하지 못 하기 때문에 for of문이나 디스트럭처링 할당이 불가능하다.

예외로 일반 객체는 이터러블 하지 않아도 스프레드 문법은 사용이 가능하다.
```js
const obj = { a: 1, b: 2 };
console.log({... obj});
```

## Symbol.iterator
이터러블한 객체 즉 for of문이 동작하게 하려면 객체에 Symbol.iterator 메서드가 필요하다.
Symbol.iterator가 있어야 이터러블 메서드가 구현이 된다.
Symbol.iterator는 호출 결과로 이터레이터를 반환 한다.

메서드 Symbol.iterator는 for of문에서 자동으로 호출된다.
개발자가 명시적으로 호출하여 활용도 가능하다
```js
const str = 'Hello';

const iterator = str[Symbol.iterator]();

while (true) {
	const result = iterator.next();
	if (result.done) break;
	alert(result.value);
}


```

이터레이터의 next메서드는 호출하면 이터러블을 순차적으로 한 단계씩 순회하며 순회 결과인, 이터레이터 객체를 반환한다.

## 이터러블과 유사 배열 객체
유사 배열 객체는 배열과 비슷하게 프로퍼티가 있어 for 반복문등 가능하지만 이터러블 하지않아 for of문을 사용할 수 없다.
```js
const arrayLike = {
	0: 'Hello',
	1: 'World',
	length: 2
};
```

- Array from
Array.from은 이터러블이나 유사 배열을 받아 진짜 Array를 만들어준다. 따라서 배열의 메서드를 사용할 수 있게된다.
```js
const arrayLike = {
	0: 'value1',
	1: 'value2',
	length: 2
}

const arr = Array.from(arrayLike);
arr.pop(); // 'value2', 제대로 동작한다.
```

## 이터레이션의 필요성
ES6이전에는 배열, 문자열, DOM컬렉션 등 각자 나름의 구조를 가지고 순회할 수 있었다.
이후 순회 가능한 데이터를 이터레이션 프로토콜로 통일하여 순회할 수 있도록 통일했다.
이터레이션을 통해 다양한 데이터를 같은 메서드와 형식을 통해 순회할 수 있도록 규정하여 효율적으로 사용할 수 있도록 했다.

문자열이나 배열 같은 내장 이터러블에도 Symbol.iterator가 구현되어 있다.


## 사용자 정의 이터러블
이터러블을 만족하지 않는 일반 객체도 이터러블, 이터레이터 규칙을 만족하게 구현하면 이터러블하게 된다.
Symbol.iterator 메서드를 가지며, 해당 메서드는 next메서드를 가지고, next메서드에서 이터레이터 객체를 반환하게 하면된다.
for of로 순회할 때마다 next메서드가 호출되며, done이 true가 될때까지 반복한다.
```js
const fibonacci = {
	[Symbol.iterator]() {
		let [pre, cur] = [0, 1];
		let max = 10;

		return {
			next() {
				[pre, cur] = [cur, pre + cur];
				return { value: cur, done: cur >= max };
			}
		}
	}
}

for (const num of fibonacci) {
	console.log(num);
}

```

- 이터러블이면서 이터레이터인 객체
이터러블이면서 이터레이터인 객체를 생성하면 Symbol.iterator를 호출하지 않고 직접 next를 호출하여 이터레이터를 반환할 수 잇다.
```js
const fibonacci = function (max){
	let [pre, cur] = [0, 1];

	return {
		[Symbol.iterator]() { return this; },
		next() {
			[pre, cur] = [cur, pre + cur];
			return { value: cur, done: cur >= max};
		}
	}
}

const iter = fibonacci(10);

iter.next(); // { value: 1, done: false }
iter.next(); // { value: 2, done: false }
```

- 무한 이터러블과 지연 평가
```js
const fibonacci = function (){
	let [pre, cur] = [0, 1];

	return {
		[Symbol.iterator]() { return this; },
		next() {
			[pre, cur] = [cur, pre + cur];
			return { value: cur };
		}
	}
}

for (const num of fibonacci) {
	if (num > 1000) break;
	console.log(num);
}

const [f1, f2, f3] = fibonacci(); // 1, 2, 3
```
보통의 배열이나 문자열은 해당 데이터를 메모리에 저장하고 공급한다.
하지만 이터러블을 위와 같이 이용하면 필요한 시점에만 데이터를 생성하여 사용할 수 있다.
즉 평가 결과가 필요할 때까지 평가를 늦추는 방법을 지연평가라 한다.

# Generator
---
제너레이터는 function* 키워드를 이용하여 선언할 수 있고 하나 이상의 yield표현식을 포함하는 객체 입니다.
generator 함수로 부터 반환 됩니다.
제너레이터 함수는 호출하게 되면 제너레이터 함수의 코드 블록이 실행되는 것이 아니라, 제너레이터 객체를 생성하고 반환한다. 이 제너레이터 객체는 이터러블, 이터레이터 모두를 만족한다.
그래서 제너레이터 객체도 next메서드를 가지며 next메서드를 통해서 순회가 가능하며 이터레이터 리절트 객체인 { value: any, done: boolean }을 반환한다.
```js
function* getFunc() {
	yield 1;
	yield 2;
	yield 3;
}

const generator = getFunc();

generator.next(); // { value: 1, done: false }
generator.next(); // { value: 2, done: false }
generator.next(); // { value: 3, done: false }
generator.next(); // { value: undefined, done: true }

```

## 일시중지, 재개
제너레이터 객체의 next 메서드를 호출하면 yield 표현식까지 실행되고 일시 중지된다. 이때 함수의 제어권이 호출자로 양도된다.
이후 필요한 시점에 호출자가 또다시 next메서드를 호출하면 실행을 재개하고 일시 중지한 다음 함수의 제어권이 다음 호출자로 또 양도된다.
이때 next메서드는 이터레이터 리절트 객체를 반환한다.

### yield는 제너레이터 안밖으로 정보를 교환할 수 있다.
바깥으로 반환 뿐만 아니라 바깥의 인자를 내부로 들여올 수 도 있다.
generator.next(arg)를 호출하면 arg는 yield의 결과가 된다.
```js
function* genFunc() {
	const x = yield 1;
	const y = yield(x + 10);
	return x + y;
}

const generator = genFunc();

console.log(generator.next()); // { value: 1, done: false }
// 첫 번째 호출에서는 인수가 넘어오지 않는다.
console.log(generator.next(10)); // { value: 20, done: false }
// 두 번째 호출에서 인자는 첫 번째 yield의 반환값으로 할당된다. 그래서 x는 인자 10으로 할당되며
// x + 10의 값이 밖으로 반환되어 value에는 20이 나오게 된다.
console.log(generator.next(20)); // { value: 30, done: true }


```

일반 함수와는 다르게 제너레이터와 외부 호출 코드는 next/yield를 이용해 결과를 전달 및 교환한다.

## 제너레이터 활용
제너레이터는 이터러블이다.
제너레이터를 사용하면 보다 쉽게 이터러블을 구현할 수 있다.
```js
const fibonacci = function () {
	let [pre, cur] = [0, 1];

	return {
		[Symbol.iterator]() { return this; },
		next() {
			[pre, cur] = [cur, pre + cur];
			return { value: car}
		}
	}
}

...

const fibonacci = function* () {
	let [pre, cur] = [0, 1];

	while (true) {
		[pre, cur] = [cur, pre + cur];
		yield cur;
	}
}

```
제너레이터는 이터러블을 쉽게 구현하는 것을 염두하고 만들었기 때문에 보다 간결하다.