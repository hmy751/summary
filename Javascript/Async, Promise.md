# 비동기
---
## 동기 처리와 비동기 처리

동기 처리는 실행 중인 테스크가 종료할 때까지 다음에 실행될 테스크가 대기하는 방식을 말한다. 비동기 처리 방식은 실행 중인 테스크가 있어도 다음에 실행될 테스크가 바로 실행 되는 방식을 말한다.

동기 방식은 실행 순서가 보장되지만 블로킹이 발생할 수 있고, 비동기 처리 방식은 블로킹은 발생하지 않지만 실행 순서가 보장되지 않는다. ^c6e5a8

자바스크립트 엔진은 단 하나의 실행 컨텍스트 스택을 갖고 있어 한 번에 하나의 테스크만 실행할 수 있는 싱글 스레드 환경으로 동작한다.

싱글 스레드 환경은 한 번에 하나의 테스크만 처리하기 때문에 블로킹이 발생한다.

### 비동기를 사용해야 하는 이유?

동기 처리 방식은 블로킹이 발생한다. 따라서 브라우저에서 이벤트 핸들러, HTTP 요청,setTimeout과 같은 동작들이 자연스럽게 실행되려면 비동기 처리방식을 통해서 병렬적으로 처리한는 과정이 필요하다.

## 이벤트 루프와 테스크 큐

싱글 스레드 환경은 하나의 테스크만 처리가 가능하여 블로킹이 발생한다.
하지만 실제 브라우저에는 비동기 처리 방식을 통해 여러 테스크가 동시에 처리된다.

이러한 자바스크립트의 동시성을 지원하는 것이 이벤트 루프다. 이벤트 루프는 콜 스택과 테스크 큐 사이에 존재하며, 두 곳을 계속해서 감시한다.
감시하는 중에 큐에 대기 중인 함수가 있다면 콜 스택이 비어있을 때 콜 스택으로 이동 시켜 비동기 처리를 도와준다.

테스크 큐는 비동기 함수의 콜백 함수 또는 이벤트 핸들러를 일시적으로 보관하는 장소다.

### 이벤트 루프와 비동기 처리과정

```js
function foo() {
  console.log("foo");
}

function bar() {
  console.log("bar");
}

setTimeout(foo, 0);
bar();
```

브라우저는 멀티 스레드로 동작하며, 브라우저내 자바스크립트 엔진이 싱글 스레드로 동작한다.

# Promise
---
프로미스란? 비동기 처리 상태와 처리 결과를 관리하는 객체다.
프로미스는 Promise 생성자 함수가 인수로 전달받은 콜백 함수 내부에서 비동기 처리를 수행한다.
이때 성공하면 resolve함수를 호출하고, 실패하면 reject함수를 호출한다.
```js
const promise = new Promise((rseolve, reject) => {
	if (response.status === 200) {
		resolve(response);
	} else {
		reject(reponse.error);
	}
});
```
프로미스는 비동기 처리에 대한 상태 와 결과 값을 저장한다.
`[PromiseState]`에 상태 값 pending, fulfilled, rejected 3가지와 `[PromiseResult]`에 처리 결과 값을 저장한다.
그리고 fulfilled, rejected 둘 중하나의 상태로 변하면 settled 상태로 표현한다.

## 프로미스가 필요한 이유
- 비동기 함수 동작
  비동기 함수는 비동기 함수의 처리 결과를 외부에 반환할 수 없고, 상위 스코프의 변수에 할당할 수도 없다.
  따라서 처리 결과에대한 후속처리는 비동기 함수 내부에서 수행해야 하는데 이렇게 하면 코드가 중첩되어 복잡도가 증가하는 콜백 지옥 현상이 발생할 수 있다.

- 에러 처리의 한계
```js
try {
  setTimeout(() => {
    throw new Error("error");
  }, 1000);
} catch (err) {
  console.error("error", err);
}
```

에러 처리를 위해 try/catch구문을 사용할 때, setTimeout함수와 같이 일반적인 비동기 함수 형태로 처리한다면 catch구문에 에러가 잡히지 않는다.
에러는 콜 스택에서 caller(호출자)방향으로 전파된다. 즉 이전 실행 컨텍스트 방향으로 전파되는데 setTimeout함수의 콜백 함수는 setTimeout함수의 실행 컨텍스트가 종료된 후 실행 되므로 이전 실행 컨텍스트로 에러가 전파되지 않으면서 catch에서 에러를 감지하지 못한다.

## 프로미스 후속 처리 메서드
 then, catch, finally는 비동기 처리 상태가 변하면 후속 처리 메서드에 인수로 전달한 콜백함수가 호출된다.
 ```js
const promise1 = new Promise((resolve, reject) => {
	resolve('success');
});

console.log(promise1); 
// 동기적으로 실행하기 때문에 promise의 반환을 확인하지는 못한다.

promise1
	.then(() => {
		(result) => console.log(result); // 'success'
	})

```
### Promise.prototype.then
두 개의 콜백 함수를 인수로 전달 받는다. 첫 번째는 프로미스가 fullfillled 되면 호출되고 비동기 처리 결과를 인수로 받는다.
그리고 then메서드는 언제나 프로미스를 반환한다.
> [!NOTE]
> thenable
> thenable은 then이라는 함수를 가진 객체를 말한다.  
> 프로미스도 then메서드 가지며 이를 만족하기 때문에 thenable에 개념에 포함되며, thenable하다고 표현한다.
> 이 then 메서드를 가지면 프로미스와 호환되며 체이닝까지 가능하게 된다.
> 이 thenable은 then메서드를 통해 다른 프로미스와 호환 되어지기 위해 사용되는 인터페이스 개념으로 생각하면 된다.
> https://promisesaplus.com/#terminology

### Promise.prototype.catch
한 개의 콜백함수를 받으며 프로미스가 rejected상태인 경우에만 호출된다.
then메서드와 마찬가치로 언제나 프로미스를 반환한다.
then을 통해서도 에러처리가 가능하지만 then내부의 콜백 함수의 에러 발생 시 catch를 통해서 감지할 수 있다.
해당 문제 뿐만 아니라 가독성을 위해서 catch를 사용하는게 낫다.

### Promise.prototype.finally
프로미스의 성공, 실패 여부와 상관없이 무조건 한 번 호출된다. 상태와 관계없이 공통적으로 수행해야 할 처리가 있을 때 유용하다.

### 프로미스 체이닝
프로미스와 후속 처리 메서드를 이용해 콜백 헬을 해결할 수 있다.
후속 처리 메서드들은 모두 프로미스를 반환하여 연속적으로 연결하여 처리를 할 수 있는데 이를 프로미스 체이닝이라고 한다.
```js
const promise1 = new Promise((resolve, reject) => {
	if (res.success) {
		resolve('success');
	}

	reject(new Error('error'));
});

promise1
	.then(() => {
		(result) => console.log(result); // 'success'
	})
	.catch((err) => console.log(err)) // 'error'
	.finally(() => console.log('finish'));

```

## 프로미스의 정적 메서드
### Promise.all
배열을 통해 여러개의 프로미스를 전달 받는다. 전달 받은 프로미스를 동시에 병렬로 실행 시킬 수 있다.
만약 하나라도 rejected되면 즉시 거부되어 에러를 발생하고 나머지 모든 프로미스의 결과도 무시된다. 단 나머지 프로미스들은 중단 되지 않고 실행된다.
다수의 프로미스를 병렬로 실행시킬 때 유용하다.

### Promise.race
배열을 통해 여러개의 프로미스를 전달 받는다. 전달 받은 프로미스를 동시에 병렬로 실행 시킬 수 있다.
Promise.all과 차이점은 단 하나의 가장 빨리 처리된 프로미스의 결과를 반환 한다. 이후에 프로미스들의 결과는 fulfilled, rejected상태와 상관없이 무시된다.

### Promise.allSettled
전달 받은 모든 프로미스가 처리될 때 까지 기다린다. 하나라도 rejected 되더라도 나머지 결과들과 같이 받을 수 있다.
```js
[
	{ status: 'fulfilled', value: '응답' },
	{ status: 'fulfilled', value: '응답' },
	{ status: 'rejected', value: 에러 },
]
```
병렬로 실행한 후 하나의 실행 결과가 실패해도 나머지 값이 필요할 때 유용하다.

## 마이크로 테스크 큐
마이크로 테스크 큐는 테스크 큐와는 별도로 프로미스 후속 처리 메서드의 콜백 함수가 저장되는 공간이다.
테스크 큐처럼 비동기 함수를 저장하다 이벤트 루프에 의해 처리 되며 차이점은 마이크로 테스크 큐가 테스크 큐보다 우선순위가 높다.
[[Browser#^c84bba]]

## fetch
fetch는 HTTP 요청을 처리하는 Web API이다.
fetch함수는 HTTP 응답인 Response를 가지는 **Promise객체를 반환**한다.
그래서 프로미스가 가지는 특징을 가지며 then메서드를 통해서도 처리가 가능하다.

# Async/Await
---


비동기 처리를 동기 처리처럼 구현할 수 있다. 가독성도 좋아진다.

async/await 구문은 프로미스를 기반으로 동작한다. 
async는 암묵적으로 반환 값을 resolve하는 프로미스를 반환한다. 프로미스가 아닌 값을 반환 해도 해당 값을 reolve로 감싸 이행된 프로미스로 반환한다.
```js
async function foo() {
	return 1;
}

async function foo() {
	return Promise.resolve(1);
} 
```


then/catch/finally 후속처리 메서드를 사용하지 않아 콜백 함수를 활용할 필요가 없어 동기 처리처럼 프로미스를 사용할 수 있다.

await 키워드는 반드시 async 구문안에 사용해야 한다. 

await 키워드는 프로미스가 settled 상태가 될때까지 대기하다가 프로미스가 처리한 reolve한 결과를 반환한다. 그리고 settled상태가 될때까지 코드를 일시중지 시킨다.


try catch 구문과 같이 사용할 수 있고 일반 비동기함수에서 try/catch구문에서의 에러 전파문제 없이 에러처리가 가능해진다.