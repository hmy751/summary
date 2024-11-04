https://react.dev/blog/2022/03/29/react-v18
[deview session 자료](https://deview.kr/data/deview/session/attach/1_Inside%20React%20(%E1%84%83%E1%85%A9%E1%86%BC%E1%84%89%E1%85%B5%E1%84%89%E1%85%A5%E1%86%BC%E1%84%8B%E1%85%B3%E1%86%AF%20%E1%84%80%E1%85%AE%E1%84%92%E1%85%A7%E1%86%AB%E1%84%92%E1%85%A1%E1%84%82%E1%85%B3%E1%86%AB%20%E1%84%80%E1%85%B5%E1%84%89%E1%85%AE%E1%86%AF).pdf)
# Concurrent 
---
18버전에서 가장 크게 개선된 특징은 동시성(concurrent)지원이다. 동시성은 한 코어 안에서 여러 작업을 컨텍스트 스위칭을 하여 
리액트의 동시성은 기능이 아니라, 화면에 UI동작을 
병렬과는 다르게 

18버전부터 달라진 점은 우선 동시성 concurrent mode, 배칭automatic batcing 등이 있습니다.


동시성모드를 지원합니다.(병렬과 다름) 블로킹 렌더링(싱글 스레드)
concurrent mode, concurrent rendering

concurrency vs parallelism
concurrency => 컨텍스트 스위칭
streaming ssr
automatic batching

배칭은 핸들러 뿐만 아니라 fetch내에서도 setState를 한 번에 업데이트 할 수 있도록 배칭 기능이 지원됐습니다.

createRoot를 통해 동시성, 오토 배칭을 지원한

## useTransition, useDeferredValue, useSyncExternalStore

https://tecoble.techcourse.co.kr/post/2023-07-09-concurrent_rendering/

## Suspense
