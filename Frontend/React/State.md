컴포넌트는 반응에 따라서 데이터가 변경되고 이를 기억해야 하는데, 컴포넌트 안에서 기억되고 유지하는 메모리 역할을 하는것을 state라고 한다.

렌더링 사이에 유지되는 데이터를 state, 변수를 업데이트 하고 리액트에 리렌더링을 알리는 setState로 구성된다. 

# state의 private
---
state는 컴포넌트에 지역적으로 존재한다. state는 같은 컴포넌트에 있더라도 각 컴포넌트가 여러 군데에서 랜더링되어도 state는 서로 분리되어 영향을 미치지 않는다.
state는 일반 함수나 코드의 특정위치보다는 화면상의 특정위치에 지역적으로 있는다고 볼 수 있다.
props와 달리 이를 선언하는 컴포넌트 이외에는 완전히 비공개되며 부모가 이를 변경할 수 없다.

# 스냅샷으로서 state
---
state는 코드에서 변수지만 스냅샷 처럼 동작한다.

## state를 설정해야 렌더링이 트리거 된다.

사용자 이벤트에 의해 인터페이스가 직접 변경되지 않고 state를 설정해야 리렌더링이 트리거 된다.

## 렌더링은 그 시점의 스냅샷을 찍는다.

리액트에서 렌더링은 컴포넌트 함수의 호출을 의미하기도 한다. 해당 함수에서 반환하는 JSX는 그 순간의 UI의 스냅샷처럼 동작한다.
prop, 이벤트 핸들러, 로컬 변수등은 모두 렌더링 당시의 state를 사용해서 계산된다.

UI 스냅샷은 interactive하고 변화하며 리액트는 이 스냅샷과 일치 하도록 화면을 업데이트하고 이벤트 핸들러를 연결한다.

state의 설정은 현재가 아닌 다음 렌더링에 대해서만 변경한다. 즉 연속적으로 state를 변경하여도 다음 렌더링에 영향을 준다.
```jsx
setNumber(number + 1);
setNumber(number + 1);
setNumber(number + 1);

...

setNumber(0 + 1);
setNumber(0 + 1);
setNumber(0 + 1);
```

## 시간경과에 따른 state

state 변수의 값은 이벤트 핸들러의 코드가 비동기적이어도 현재 렌더링 내에서 절대 변경되지 않는다. 
```jsx
setNumber(0 + 5);
setTimeout(() => {
	alert(0);
}, 3000);
```

state는 현재 렌더링, 현재 함수 내부에서는 고정으로 유지하여 호출되므로 실행되는 동안 이벤트 핸들러 내에서  비동기로 호출하더라도, state의 변경등을 고려할 필요는 없다.

# 여러 state 업데이트를 큐에 담기
---
state변수를 설정하면 렌더링이 큐에 들어가게 된다. 그러나 경우에 따라서 다음 렌더링을 큐에 넣기전에 여러 작업을 수행하고 싶을때는 state업데이트를 배치하는 방법들이 있다.

## state 업데이트 일괄처리

각각의 렌더링에서 state값은 고정되어 있어 연속적으로 실행해도 고정된 state값이며, 다음 렌더링에 영향을 준다.

리액트는 state 업데이트를 하기전에 이벤트 핸들러의 모든 코드가 실행될 때까지 기다린다. 그래서 리 렌더링은 모든 setState 호출이 완료된 이후에 일어난다.
이렇게 해야 너무 많은 리 렌더링을 트리거하지 않으면서 다수의 state 변수를 업데이트 할 수 있다. 이를 일종의 일괄처리(batching)라고한다.

그리고 state 업데이트와 달리 클릭과 같은 이벤트에 대해서는 일괄처리하지 않고 개별적으로 처리한다.

## 다음 렌더링 전에 동일한 state 변수를 여러번 업데이트 하기

현재 렌더링에서 다음 렌더링전에 동일한 state 변수를 여러번 업데이트 하고 싶다면 setState에 다음 state를 전달하지 않고, 이전 state를 기반으로 다음 state를 계산하는 함수를 전달해야 한다.
이는 단순히 state의 대체가 아니라 state값으로 무언가를 하라고 지시하는 방법과 같다.
```jsx
setNumber((number) => number + 1);
```

여기서 (number) => number + 1은 업데이터 함수라고 부른다.
이 업데이트 함수를 state setter에 전달하면 리액트는 이벤트 핸들러의 다른 코드가 모두 실행된 후에 업데이트 함수가 처리되도록 큐에 넣는다.
다음 렌더링 중에 리액트는 큐를 순회하며 최종 업데이트된 state를 제공한다.

## state를 교체한 후 업데이트 한다면?

업데이터 함수에 state 변수를 교체해서 진행해도 결과는 똑같다.

## 업데이트 후 state를 바꾸면?

```jsx
setNumber(n => n + 1);
setNumber(number + 5);
```
둘 다 큐에 추가된다. 첫번째는 1을 증가시키라고 하여 1을 반환하고 그 다음 큐에서는 0에서 5로 교체하라고 하기때문에 최종 값은 5가된다.

이벤트 핸들러가 완료되면 리렌더링을 실행한다. 리렌더링 동안 큐를 처리한다.

# 객체 state 업데이트
---
state는 객체를 포함해서 어떤 데이터도 저장할수 있다. 하지만 직접 객체를 변이하면 되지 않고 대신 객체 업데이트를 하려면 새 객체를 생성하고 해당 복사본을 사용해서 state를 다시 설정해야 한다.

## mutation?

자바스크립트의 원시형 데이터로 state에서 0에서 5로 변경했다고 가정한다면 0자체는 변경되지 않고 새로운 데이터5로 교체되면서 다른 데이터로 인지해 리렌더링을 발생시킨다.
왜냐하면 자바스크립트에서는 원시형 값은 불변하기 때문이다.

하지만 객체의 경우 객체 내부의 프로퍼티를 통해서 변경하게 되면 객체 값 자체는 유지되면서 프로퍼티가 변경되게 된다.
이를 mutation이라고 한다.
객체는 기술적으로 이렇게 변이할 수 있지만 불변하는 것처럼 취급해야 한다. 그래서 변이대신 교체해야 된다.

## state를 읽기 전용으로 취급하기

state에 넣는 모든 자바스크립트 객체는 읽기 전용으로 취급해야 한다.
만약 프로퍼티를 통해 변경하면 객체 자체는 변경되지 않았기 때문에 리액트가 변경을 인식하기 못한다.

그래서 읽기 전용으로 취급하여 새로운 객체를 만들고 전달해야 한다.

지역변이(local muation)는 가능하다 예를들어 
```jsx
const nextPosition = {};  

nextPosition.x = e.clientX;  

nextPosition.y = e.clientY;  

setPosition(nextPosition);
```
이렇게 지역적으로 새로운 객체를 만들어서 변경하는 것은 가능하다. 왜냐면 이전 객체가 아니라 지역적으로 생성한 새로운 객체이기 때문이다.

## 스프레드 구문으로 복사하기

변경할 프로퍼티 이외에 프로퍼티는 스프레드 연산자로 간결하게 복사하여 작성할 수 있다.

## 중첩된 객체 업데이트 하기

중첩된 객체도 스프레드 연산자를 통해서 변경이 가능하다.

## Immer로 간결한 업데이트 로직 작성하기

중첩된 객체를 변경할때 immer를 통해서 플랫하게 할수 있다.
immer에서는 내부적으로 drafet라는 프록시의 특수 유형의 객체로 사용자가 수행하는 작업을 기록한다. 그래서 이를 통해 자유롭게 수정이 가능하다.

## React에서 state변이를 권장하지 않는 이유

state를 변이하지 않으면 과거의 기록이 최근에 변의에 의해 지워지지 않고 디버깅할 수 있다.

최적화 전략은 state의 변경 여부에 따라서 작업을 건너뛰는데 이때 state변이를 하지 않아야 이 여부를 정확하게 확신할 수 있다.

그외 새로운 기능은 state가 스냅샷처럼 취급되있는 것을 기준으로 개발 되기때문에 변이를 하게 되면 문제가 생길수 있다.

요구 사항 변경에 대해서도 사용자가 양식을 이전 값으로 재설정하는 등 state의 변이가 없어야 과거의 state복사본을 통해 쉽고 빠르게 처리할 수 있다.

변이가 없다고 가정되면 프록시로 감싸거나 별 다른 작업을 필요로 하지 않고 구현할 수 있다.

따라서 변이하지 않아야 추후에 새로운 기능이 개발되어도 영향이 가지 않는다.

# 배열 state 업데이트
---
배열도 객체처럼 변이 없이 업데이트 해야 한다.

## 변이 없이 배열 업데이트

배열 내부 항목에 접근해서 변경하거나 push, pop과 같은 배열을 변경하는 메서드를 사용하면 안된다.
대신 새 배열을 전달해야 하며 filter, map과 같은 비변이 메서드는 가능하다.

스프레드 연산자나 immer를 사용할 수 있다.

배열내 객체가 있는 경우 map을 이용해서 해당 객체를 찾고 스프레드 연산자를 통해 새로운 객체르 복사하여 구할수 도 잇으며 immer를 통해서 중첩된 객체에 대한 변이를 처리할 수 잇다.

```jsx
const myNextList = [...myList];
const artwork = myNextList.find(a => a.id === artworkId);
artwork.seen = nextSeen;
setMyList(myNextList);

///

setMyList(myList.map(artwork => {
	if (artwork.id === artworkId) {
		return {...artwork, seen: nextSeen };
	}
	...
}));

```

```jsx
	const [myList, updateMyList] = useImmer(initialList);

	updateMyList(draft => {
		const artwork = draft.find(a => a.id === id);
		artwork.seen = nextSeen;
	});


```




# state로 입력에 반응하기
---
리액트는 UI를 조작하는 선언적인 방법을 제공한다. 개별적으로 직접 조작하는 대신 다양한 상태를 기술하고, 사용자 입력에 반응하여 각 상태들 사이를 전환한다.

## 선언형과 명령형의 차이점

명령형은 인터랫션을 구현할때 직접적으로 지침을 작성하여 UI를 조작한다.
단점은 복잡한 시스템에서는 관리가 기하급수적으로 늘어나고 모든 인터렉션 코드를 주의깊게 확인해야 한다.

리액트에서는 명령형이 아닌 선언형으로 작성된다.
표시할 내용을 선언만 하면 리액트가 UI를 업데이트할 방법을 알아낸다.

## UI를 선언적 방식으로 생각하기

선언적 방식으로 생각하고 구현한다면 아래의 과정을 거친다.

1. 컴포넌트의 다양한 시각적 상태를 식별한다.
2. 상태 변화를 촉발하는 요소를 파악한다.
3. useState를 사용하여 메모리의 상태를 표현한다.
4. 비필수적인 state변수를 제거한다.
5. 이벤트 핸들러를 연결하여 state를 설정한다.

### 1. 컴포넌트의 다양한 시각적 상태를 식별한다.
리액트는 디자인과 개발의 교차점에 있으며 비슷한 방식을 공유한다.
form요소를 구현한다고 가정할 때 다양한 시각적 상태에 따라 시각화 한다.
- 비어있는 경우
- 입력중일 경우
- 제출중
- 성공시
- 실패시
이런식으로 먼저 로직을 추가히전에 목업을 만든다.
첫번재로 비어있는 상태에 대한 목업의 UI를 만든다.

시각적 상태가 많은경우 미리 한페이지에 모두 표시할 수 있는데 그렇게 할수도 있지만 스토리북을 활용할 수 도 잇다.

### 2. 상태 변경을 트리거 하는 요인 파악하기
여기서는 사람의 입력이나 클릭 그리고 서버의 성공 및 에러 응답으로 생각할 수 있다.

모든 경우 state 변수를 설정해서 UI를 업데이트 할 수 있다.
텍스트 입력중, 제출 버튼클릭, 네트워크 응답 성공, 요청 실패 등으로 나눌 수 잇다.

그리고 이를 시각화 하여 흐름을 파악해볼 수 있다.

### 3. 메모리의 상태를 useState로 표현하기
메모리에서 시각적상태를 useState로 표현하는데 핵심은 가능한 적은수로 구현해야 한다.
적은게 좋지만 생각나지 않는다면 모든 경우의 수를 state로 나타낼수 있다.

```jsx
const [isEmpty, setIsEmpty] = useState(true);  

const [isTyping, setIsTyping] = useState(false);  

const [isSubmitting, setIsSubmitting] = useState(false);  

const [isSuccess, setIsSuccess] = useState(false);  

const [isError, setIsError] = useState(false);
```

### 4. 비필수적인 state 제거하기
최대한 간결하게 state를 줄이기 위해서 몇가지 기준으로 제거할 수 있다.

- state 모순
예를 들어 isTyping과 isSubmitting은 동시에 true가 되는 상황은 논리적으로 불가능하다. 이럴경우 두 상태를 각각 분리하지 않고 합칠수 있다.
- 동일한 정보를 포함하는 다른 state변수
isEmpty와 isTyping은 서로 연관이 있는 상태다. 두 상태는 서로 모순되는 상태이므로 이 모순을 피하기 위해 isEmpty는 answer.length === 0 이라는 표현으로 불필요한 상태를 줄일 수 있다.
- 다른 state변수로부터 파생되는 상태
isError와 error는 서로 관계가 있는데 error를 통해서 isError의 상태를 가지고 표현하지 않아도 된다.

```jsx
const [answer, setAnswer] = useState('');  

const [error, setError] = useState(null);  

const [status, setStatus] = useState('typing'); // 'typing', 'submitting', or 'success'
```

### 5. 이벤트 핸들러를 연결하여 state를 설정하기
모든 상호작용을 이벤트 핸들러로 연결하여 마무리한다.

이렇게 선언적으로 작성하면 나중에 기존 상태를 깨지 않고도 새로운 시각정 상태를 도입할수 있고 인터렉션 자체의 로직을 변경하지 않아도 된다.

# State 구조 선택
---
state를 잘 구조화 해야 수정과 디버깅이 편한 컴포넌트로 설계할 수 있다.

## state 구조화 원칙

1. 관련 state를 그룹화한다. 두개이상의 state를 동시에 업데이트 하는경우 단일 state 변수로 병합하는 것이 좋다.
2. state의 모순을 피한다. 여러 state조각이 서로 모순되거나 불일치 하면 오류가 생길수있다.
3. 불필요한 state를 피한다. 기존 state 변수나 props로 계산할 수 있는것은 생성하지 않아도 된다.
4. state 중복을 피한다. 동일한 데이터가 여러 state 변수간에 또는 중첩된 객체 내에 중복되면 동기화 state를 유지하기 어렵다.
5. 깊게 중첩된 state는 피한다. 깊게 계층화된 state는 업데이트하기 쉽지 않다.

## 관련 state 그룹화하기

x,y 포지션과 같이 항상 함께 변경되는 경우는 따로 선언하기 보다 그룹화 해서 객체로 선언하는게 더 낫다. 그래야 동기화 상태를 같이 잊지않고 유지할 수 있다.

## state의 모순을 피하기

isSending, isSent와 같은 state가 있을경우 작동은 하지만 둘 다 true로 설정되는 모순이 생길수 있으며 관리가 복잡할수 있다 이럴때는 하나의 status로 줄일 수 있다.

## 불필요한 state 피하기

firstName, lastName 이 있을때 굳이 fullName으로 불필요하게 추가할 필요가 없다.

### props를 state에 그대로 미러링 하면 안된다.
```jsx
function Message({ messageColor }) {  
	const [color, setColor] = useState(messageColor);
	...
```
이렇게 useState에 초기값으로 전달하는 경우 props가 변경되도 color의 변경을 발생하지 않는다. 이유는 useState의 초기값은 초기렌더링에만 영향을 주고 그 이후에는 영향을 주지 않는다.
props를 state로 미러링 하는 경우는 모든 업데이트를 무시하고 초기값만 전달할 때 유효하다.

## state 중복을 피하기

```jsx
const initialItems = [
  { title: 'pretzels', id: 0 },
  { title: 'crispy seaweed', id: 1 },
  { title: 'granola bar', id: 2 },
];

export default function Menu() {
  const [items, setItems] = useState(initialItems);
  const [selectedItem, setSelectedItem] = useState(
    items[0]
  );
  
  <button onClick={() => {
    setSelectedItem(item);
	}}>Choose</button>
  }
```
이렇게 아이템 state를 복제하여 selectedItem으로 지정할 수 도 있지만 동기화가 되지 않을 수 있다.

중복을 피하는 방법으로는 
```jsx
const initialItems = [
  { title: 'pretzels', id: 0 },
  { title: 'crispy seaweed', id: 1 },
  { title: 'granola bar', id: 2 },
];

export default function Menu() {
  const [items, setItems] = useState(initialItems);
  const [selectedId, setSelectedId] = useState(0);

  const selectedItem = items.find(item =>
    item.id === selectedId
  );
  
           <button onClick={() => {
              setSelectedId(item.id);
            }}>Choose</button>


```
아이디만 따로 state로 분리하고 items를 그대로 활용하여 selectedItem을 지정한다. 이렇게 되면 items에 있는 item과 selectedItem이 동기화 된다.

## 깊게 중첩된 state는 피하기

너무 깊게 형성된 객체 데이터는 업데이트하기 어렵기 때문에 정규화 과정으로 수정하면 업데이트가 더 수월해진다.

# 컴포넌트 간의 state 공유
---
때로는 두 컴포넌트의 state가 항상 함께 변경되기를 원할 때가 있다. 이럴때는 state를 가장 가까운 부모 컴포넌트로 이동한다음 props를 통해 전달하면 된다. 이를 state 끌어올리기라고 한다.

## state 끌어올리기

하나의 스테이트가 두 컴포넌트에 공통적으로 영향을 주려면 상위에 가까운 부모 컴포넌트로 끌어올려 공유할 수 있다.

여기서 state가 끌어올려지고 그 state를 관리하는 부모컴포넌트느 제어 컴포넌트, 그 state에 의해 제어되는 컴포넌트를 비제어 ㅋ컴포넌트라고 한다.

## 각state의 single source of truth

state의 위치에 따라 소유하는 컴포넌트와 제어되는 컴포넌트등으로 나눠지게 된다.
여기서 single source of truth, 단일 진실 공급원이라는 개념이 나오는데 모든 state가 한곳에 있다는 의미가 아니라 각 state마다 해당 정보를 소유하는 특정 컴포넌트가 있다는 의미다.
즉 컴포넌트간에 공유하는 state를 복제하는 대신 공통으로 공유하는 부모로 끌어올려서 필요한 자식에게 전달하여 공유 된다는 의미다.

# state 보존 및 재설정
---
state는 컴포넌트간에 격리된다. 리액트는 UI트리에서 어떤 컴포넌트가 어떤 state에 속하는지를 추적하며 state를 언제 보존하고 언제 초기화할지를 제어할 수 있다.

## state는 트리의 한 위치에 묶인다.

컴포넌트에 내부에 따라 state가 스냅샷으로서 역할을 하기 때문에 내부에 존재한다고 생각할수 있다. 하지만 state는 실제로 React 내부에서 유지된다. 리액트는 렌더링 트리에서 해당 컴포넌트의 위치에 따라 보유하고 있는 각state를 올바르게 연결한다.

state는 렌더링 되는동안에만 유지된다. 예를 들어 해당 컴포넌트에서 카운트를 증가 시킨뒤 해당 컴포넌트를 제거하고 다시 렌더링하면 해당 state는 유지되지않고 초기화 된다. 또한 같은 위치에 다른 컴포넌트가 렌더링되어도 해당 컴포넌트의 state를 삭제한다.

## 동일한 위치의 동일한 컴포넌트는 state를 유지한다.

https://codesandbox.io/p/sandbox/react-dev-6f2zp8?file=%2Fsrc%2FApp.js%3A12%2C14&utm_medium=sandpack
동일한 컴포넌트가 조건에 의해 서로 교환되며 삭제되어도 동일한 위치에 같은 컴포넌트로서 렌더링되면 리액트 관점에서는 같은 것으로 보아 state가 유지된다.
React에서 중요한 것은 JSX 마크업이 아니라 UI트리에서의 위치가 중요하다.

## 동일한 위치의 다른 컴포넌트는 state를 초기화 한다.

같은 위치에 다른 컴포넌트로 전환하면 state가 재설정된다.
같은 위치를 판별하는 기준은 해당 컴포넌트 위치에 바로 위의 부모컴포넌트 입장에서 바로 아래 위치에 컴포넌트가 바뀌면 트리를 기준으로 변경되었다고 판단하여 state를 초기화한다.

즉 리렌더링 사이에 state를 유지하려면 트리의 구조가 일치해야 한다.

간혹 같은 위치여도 state가 유지되지 않을 수 있는데 이는 컴포넌트 내부에 또다른 컴포넌트를 정의하고 렌더링하는 경우다. 해당 컴포넌트가 리렌더링 되면 정의한 컴포넌트가 새로 생성되기 때문에 이 경우에는 state가 유지되지 않는다.
따라서 항상 컴포넌트 함수는 최상위 수준에서 선언해야 한다.

## 동일한 위치에서 state 재설정하기

같은 컴포넌트 위치에 잇을때 state를 유지하는데 때로는 변경되어야 하는 경우도 있다. 예를 들어 같은 컴포넌트지만 이를 이름과 같은 구분되는 상태에 따라 구분되어야 하는 경우는 state가 유지되면 안된다.

이럴 경우 두 가지 방법이 있다.

1. 컴포넌트를 다른 위치에 렌더링 시키기
2. 각 컴포넌트에 특정 key 부여하기

다른 위치라 함은 삼항 연산자가 아닌 각각의 독립적인 위치에 렌더링을 시킨다.
```jsx
      {isPlayerA &&
        <Counter person="Taylor" />
      }
      {!isPlayerA &&
        <Counter person="Sarah" />
      }
```

두번째 방법은 key를 부여하는 것이다.
```jsx
     {isPlayerA ? (
        <Counter key="Taylor" person="Taylor" />
      ) : (
        <Counter key="Sarah" person="Sarah" />
      )}
```
key를 부여하게 되면 리액트가 부모 내 순서가 아닌 key자체로 특정 컴포넌트 임을 리액트에 알릴수 있다.

## 키로 form 재설정하기

채팅앱에서 텍스트 폼이 있고 이때 key로 구분하지 않게되면 텍스트 필드는 같은 위치에 있기 때문에 주체가 달라져도 state가 달라지지 않게 된다. 
이때 key를 부여함으로서 해결할 수 있다.

만약 무조건 재설정이 아니라 주체자에 따라 구분하고 저장되게 하려면 필요한 state를 상위로끌어올려서 해결할 수 있다.

# state로직을 reducer로 추출하기
---
여러 개의 state업데이트가 여러 이벤트 핸들러에 분산되어 있는 컴포넌트는 과부하가 걸릴 수 있다. 이러한 경우 reducer를 통해 단일 함수로 컴포넌트 외부의 모든 state업데이트 로직을 통합할 수 잇다.

## reducer로 state로직 통합하기

컴포넌트가 복잡해지고 이벤트 핸들러가 많아지면 컴포넌트의 state의 업데이트가 파악하기 어려워진다. 
이때 state 로직을 컴포넌트 외분의 reducer라고 하는 단일 함수로 옮길수 있다.

1. state를 설정하는 것에서 action들을 전달하는 것으로 변경하기
2. reducer 함수 작성
3. 컴포넌트에서 reducer 사용
각 단계를 거쳐서 reducer로 통합할 수 있다.

각 이벤트 핸들러에서 action으로 변경하여 dispatch 하는 형태로 변경한다.
그 다음 reducer 함수를 작성한다. reducer 함수 내부에서 action type에 따라서 상태를 처리하며 이 reducer 함수는 외부로 내보낼수도 있다. 

컴포넌트에서 useReducer를 통해서 해당 reducer를 사용할 수 있다. 인자는 reducer 함수와 초기 상태가 전달된다.

## useState와 useRedcuer 비교하기

코드 크기는 이벤트 핸들러가 많아지게 되면 useReducer가 더 유리하다

가독성은 간단할 때는 useState가 더 낫지만 state 변경 로직등이 복잡해지면 useReducer가 한눈에 파악하기 더 좋다.

디버깅 측면에서 reducer가 어떤 action으로 인해 발생했는지 더 파악하기 쉽다.

테스팅 측면에서 reducer는 컴포넌트에 의존하지 않는 순수함수 이기때문에 별도로 내보내 테스트하기 좋다.

## reducer 잘 작성하기

reducer는 반드시 순수해야 한다. 요청을 보내거나 사이드 이펙트를 수행해서는 안된다.

## immer를 같이 활용하기

immer를 같이 활용해서 useImmerReducer를 활용하면 reducer 내부에 상태 변경도 immer를 통해서 더 간결해진다.

# context로 데이터 깊숙히 전달하기
---
Context를 사용하면 컴포넌트의 깊이가 깊어져도 state를 전달할 수 있다.

## props전달의 문제

전달하려믄 컴포넌트의 깊이가 깊어지면 props전달이 장황하고 불편해지며 이렇게 많이 깊어지는 경우르 props drilling이라고 한다.

## props 전달의 대안

Context를 사용하면 상위 컴포넌트가 그 아래 전테트리에 데이터를 제공한다.

1. context 만들기
2. context 사용하기
3. context 제공하기

## Context는 중간 컴포넌트를 통과한다.

## context를 사용하기 전에

context를 너무 쉽게 남용하기전에 
props 전달을 시도하기 props를 사용하면 데이터의 흐름이 명확해진다.
그리고 props 데이터 전달이나 context 사용전에 JSX를 children으로 전달하여 해결할 수 있다. 상위에 Layout 역할을 하는 컴포넌트가 데이터만 전달한다면 children으로 전달하는 형태로 수정하여 해결할 수 있다.

## context 사용사례

테마, 로그인 유저 정보, 라우팅 등에 사용할 수 있다.

# Reducer와 Context
---
reducer를 통해 복잡한 state변경 로직들을 통합하고 이를 하위 컴포넌트에 context를 활용해서 이벤트 함수를 전달할수 잇다.
그럼 state로직의 복잡성을 줄여 통합하고 props driliing 문제를 해결할 수 있다.

provider 컴포넌트를 생성하여 context와 reducer 함수를 하나의 파일로 합쳐서 통합할수 도 있다.
