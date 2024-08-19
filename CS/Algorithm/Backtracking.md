백트래킹(Backtracking)은 재귀적으로 조회하며 해답을 찾아가는 기법이다. 다만 완전 탐색처럼 모든 경우의 수를 저장하지 않고 해답이 아니라고 판단되면 조회 도중 중단하고 되돌아가서 다시 해답을 찾아나가는 기법이다. 이 되돌아가는 과정을 백트래킹이라고 부른다.

백트래킹을 사용하면 불필요한 탐색을 줄일 수 있다.
주로 조합이나 가지치기처럼 경우의 수를 구하는 문제에 사용된다.

과정은 
보통 선택 => 탐색(백트래킹) => 해제 순서로 코드를 실행한다.
```js
answer = [];

const backtrack = (comb, index) => {
	// 종료 및 해제 조건, 여기서 해답을 만족하면 결과를 저장한다.
	if (...) {
		answer.push(comb);
	}

	for (let i = 0; i < length; i++) {
		// 선택
		comb += current;
		// 다음 가짓수로 탐색을 실행하며
		backtrack(comb, index + 1);
		// 위에서 현재 선택한 경우를 해제한다.
		comb -= current;
	}
}

backtrack([], 0);

```