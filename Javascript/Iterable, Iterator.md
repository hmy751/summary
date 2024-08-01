# Iterable 객체
이터러블은 박본 가능한 객체로 배열을 일반화한 객체다. for of  반복문을 적용할 수 있다.
이터레이터를 통해서 for of를 사용할 수 있는 객체를 이터러블 객체라고 한다.

- Symbol.iterator
이터러블한 객체 즉 for of문이 동작하게 하려면 객체에 Symbol.iterator 메서드가 필요하다.

Symbol.iterator가 있어야 이터러블 메서드가 구현이 된다.
결과는 이터레이터라고 부른다.
이터레이터는 반복과정을 처리한다.

이터레이터에는 객체 { done: boolean, value:  any }를 반환하는 메서드 next가 구현되어 있어야 한다.

메서드 Symbol.iterator는 for of에 의해 자동으로 호출된다.
개발자가 명시적으로 호출하여 활용도 가능하다

```js

```


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