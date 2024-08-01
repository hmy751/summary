# Iterator, Iterable
- Iterator
이터레이터(Iterator) 프로토콜은 next 메서드를 소유하며 next 메서드를 호출하면 이터러블을 순회해 value, done 프로퍼티를 가지는 이터레이터 객체를 반환하게 하는 규칙이다.
```js
[Symbol.iterator] = function () {
	...
	return {
		next() {
			...
			if (this.current <= this.last) {
				return { done: fasle, value: this.current++ };
			}
			return { done: true }
		}
	}
}

```

- Iterable
이터러블 객체는 이터러블(Iterable) 프로토콜을 준수한 객체다. 
이터러블 객체는 반복 가능한 객체로 Symbol.iterator메서드를 가지며 이 메서드를 통해 이터레이터(Iterator)프로토콜을 준수하는 이터레이터를 반환하므로서 이터러블을 만족한다.
이터러블을 만족하면 for of문을 통해 순회할 수 있다.

이터러블(Iterable)은 반복 가능한 객체로 배열을 일반화한 객체다. for of  반복문을 적용할 수 있다.
이터레이터를 통해서 for of를 사용할 수 있는 객체를 이터러블 객체라고 한다.

이터러블 객체의 핵심은 관심사 분리다. 즉 프로퍼티에 Symbol.iterator를 가지지 않는다.

이터러블을 만족하면 스프레드 문법과 배열 디스트럭처링이 가능하다. 
예외로 일반 객체는 이터러블 하지 않아도 스프레드 문법 사용이 가능하다.
```js
const obj = { a: 1, b: 2 };
console.log({... obj});
```


- Symbol.iterator
이터러블한 객체 즉 for of문이 동작하게 하려면 객체에 Symbol.iterator 메서드가 필요하다.

Symbol.iterator가 있어야 이터러블 메서드가 구현이 된다.
Symbol.iterator결과를 이터레이터라고 부른다.
이터레이터는 반복과정을 처리한다.

이터레이터에는 객체 { done: boolean, value:  any }를 반환하는 메서드 next가 구현되어 있어야 한다.

메서드 Symbol.iterator는 for of에 의해 자동으로 호출된다.
개발자가 명시적으로 호출하여 활용도 가능하다

```js

```


이터레이터의 next메서드는 호출하면 이터러블을 순차적으로 한 단계씩 순회하며 순회 결과, 이터레이터 객체를 반환한다.

- for of 문

이터레이터를 명시적 호출도 가능하다

- 유사배열객체
유사 배열 객체는 배열과 비슷하게 프로퍼티가 있어 for 반복문등 가능하지만 이터러블 하지않아 for of문을 사용할 수 없다.

- Array from
이터러블이나 유사 배열을 받아 진짜 Array를 만들어준다. 따라서 배열의 메서드를 사용할 수 있게된다.
```js
const arrayLike = {
	0: 'value1',
	1: 'value2',
	length: 2
}

const arr = Array.from(arrayLike);
arr.pop(); // 'value2', 제대로 동작한다.
```

- 이터레이션의 필요성
	- 데이터 소비, 공급

- 사용자 정의 이터러블
	- 이터러블이면서 이터레이터인 객체
	- 무한 이터러블
	  지연 평가, 메모리, 데이터를 미리 생성 하지 않는다.