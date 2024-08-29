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

## 2. 상태 변경을 트리거 하는 요인 파악하기

여기서는 사람의 입력이나 클릭 그리고 서버의 성공 및 에러 응답으로 생각할 수 있다.

모든 경우 state 변수를 설정해서 UI를 업데이트 할 수 있다.
텍스트 입력중, 제출 버튼클릭, 네트워크 응답 성공, 요청 실패 등으로 나눌 수 잇다.

그리고 이를 시각화 하여 흐름을 파악해볼 수 있다.

## 3. 메모리의 상태를 useState로 표현하기

메모리에서 시각적상태를 useState로 표현하는데 핵심은 가능한 적은수로 구현해야 한다.
적은게 좋지만 생각나지 않는다면 모든 경우의 수를 state로 나타낼수 있다.

```jsx
const [isEmpty, setIsEmpty] = useState(true);  

const [isTyping, setIsTyping] = useState(false);  

const [isSubmitting, setIsSubmitting] = useState(false);  

const [isSuccess, setIsSuccess] = useState(false);  

const [isError, setIsError] = useState(false);
```

## 4. 비필수적인 state 제거하기

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

## 5. 이벤트 핸들러를 연결하여 state를 설정하기

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

isSending, isSent와 같은 state가 있을경우 작동은하지만 둘 다 true로 설정되는 모순이 생길수 있으며 관리가 복잡할수 있다 이럴때는 하나의 status로 줄일 수 있다.

## 불필요한 state 피하기

firstName, lastName 이 있을때 굳이 fullName으로 불필요하게 추가할 필요가 없다.

### props를 state에 그대로 미러링 하면 안된다.
```jsx
function Message({ messageColor }) {  
	const [color, setColor] = useState(messageColor);
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

