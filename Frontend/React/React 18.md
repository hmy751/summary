https://react.dev/blog/2022/03/29/react-v18
[deview session 자료](https://deview.kr/data/deview/session/attach/1_Inside%20React%20(%E1%84%83%E1%85%A9%E1%86%BC%E1%84%89%E1%85%B5%E1%84%89%E1%85%A5%E1%86%BC%E1%84%8B%E1%85%B3%E1%86%AF%20%E1%84%80%E1%85%AE%E1%84%92%E1%85%A7%E1%86%AB%E1%84%92%E1%85%A1%E1%84%82%E1%85%B3%E1%86%AB%20%E1%84%80%E1%85%B5%E1%84%89%E1%85%AE%E1%86%AF).pdf)
https://github.com/reactwg/react-18/discussions/46
# Concurrent 
---
리액트의 동시성은 특정 기능보다는 메커니즘으로 화면에 UI동작을 작은 단위, 즉 테스크로 나누어 각 테스크들을 동시에 다룰수 있게 한다.
병렬과는 다른점은 테스크들을 단순히 동시에 실행하는 것이 아니라 각 테스크들을 중지 및 재개하면서 컨텍스트 스위칭을 통해 한꺼번에 다루는 점이다. 
따라서 동시에 실행되는 것처럼 보이지만 실제로는 컨텍스트 스위칭을 통해 우선순위별로 자연스럽고 효율적으로 처리한다.

동시성을 지원하는 이유는 렌더링 블로킹 때문이다. 싱글 스레드 환경에서는 한 번에 하나의 작업만 할 수 있다 따라서 여러 테스크들이 한 번에 실행되면 중간에 무거운 연산이라도 있거나 여러 테스크들이 겹치게 되면 렌더링 블로킹으로 변경이 중지되어 부자연스러운 UI의 변화가 있을 수 있다.

## Automatic batching, createRoot

자동 배칭(automatic batching)은 모든 상태변경에 대해 리액트가 자동으로 한 번에 모아 일괄적으로 처리하는 방식이다. 
이전에는 이벤트 핸들러 내부에서 setState를 여러번 실행할 경우에만 지원됐다. 18버전 이후에는 이벤트 핸들러 외부 뿐만아니라 비동기 이벤트에서도 자동 배치가 이뤄진다.
다만 루트 컴포넌트의 createRoot메서드를 사용할 시에 자동 배칭 기능이 적용된다.

## Concurrent Features, useTransition, useDeferredValue

18에서 추가된 기능으로 동시성을 기반으로 구현된 기능이다.
동시성을 기반으로 배칭등 작업을 효율적이게 처리하지만 일부 업데이트를 개발자가 직접 명시하여 의도적으로 우선순위를 낮추어 렌더링 브로킹을 방지하며 UI의 변화를 자연스럽게 하기위한 기능이다.

useTransition은 상태 변화 메서드를 의도적으로 우선수위를 낮추는 기능이다.
```js
const App = () => {
  const [text, setText] = useState('');
  const [isPending, startTransition] = useTransition();
  const filteredItems = filterItems(text);

  const updateFilter = e => {
    startTransition(() => {
      setText(e.target.value);
    });
  };

  return (
    <div className="container">
      <input type="text" name="user-input" onChange={updateFilter} />
      <label htmlFor="user-input" className={isPending ? 'show' : 'hide'}>
        pending...
      </label>
      <List items={filteredItems} />
    </div>
  );
};

```

useDeferredValue는 값의 업데이트의 우선순위를 낮춘다.
주로 props로 받는 값등 제어할 수 없는 값에대해 사용을 권장한다.
```js
const List = ({ items }) => {
  const deferredItems = useDeferredValue(items);

  return (
    <ul>
      {deferredItems.map(item => (
        <li key={item}>{item}</li>
      ))}
    </ul>
  );
};

```

https://tecoble.techcourse.co.kr/post/2023-07-09-concurrent_rendering/

## Streaming SSR, Suspense, Selective Hydrating

기존에 워터펄 방식에서는
데이터 패칭(서버) => HTML 렌더링(서버) => 자바스크립트 코드 로드(클라이언트) => hydration(클라이언트)
순서로 실행되기 때문에 어느 부분이라도 지연된다면 다음 단계가 실행되지 않고 블로킹되며 비효율적이며 사용자 경험에 문제가 있었다.

그래서 18버전 부터는 스트리밍과 suspense를 활용하여 문제를 해결했다.

먼저 스트리밍을 활용하기 위해 renderToString대신 pipeToNodeWritable를 활용하여 청크 단위로 나누어 전달할 수 있게 하고 suspense를 활용하여 데이터 요청이 있는 UI를 감싸 선언적으로 fallback시켜 블로킹을 방지할 수 있게 한다.

그래서 suspense이외의 UI는 기다리지 않고 바로 hydration까지 가능하게 되었다.

만약 일부 유저 이벤트가 발생한다면 리액트는 동시성을 지원하므로 다른 작업을 잠시 중단하고 먼저 처리하여 사용자 경험을 개선했다.

또 suspense를 다른 컴포넌트에 배치하여 hydrate의 우선순위도 조정할 수 있는데 상위의 suspense부터 hydrating을 먼저 시작한다.
하지만 만약 다른 컴포넌트를 클릭하게 되면 유저 이벤트가 발생한 부분부터 즉시 처리한다.

https://blog.mathpresso.com/suspense-ssr-architecture-in-react-18-ec75e80eb68d
https://saengmotmi.netlify.app/react/streaming_ssr/
https://github.com/reactwg/react-18/discussions/37
## React Server Components(RSC)

