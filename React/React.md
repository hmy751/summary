# 리액트에서 동등 비교
[[Variable, Data#^5180cc]]
[[Basic#^ef3ef9]]
리액트에서 값을 비교할 때는 shallowEqual이라는 메서드를 통해서 비교한다.
shallowEqual은 기본적으로 Object.is()메서드를 기준으로 먼저 비교한다.
그리고 Object.is()의 객체 비교를 보완하고자 객체에 대한 비교를 한 번 더하게 된다. 
여기서 객체에 대한 비교를 할 때 1depth 프로퍼티의 데이터까지 조회하여 Object.is()를 통해 비교하며 중첩된 데이터에 대해서는 비교를 하지 않는다.
중첩된 프로퍼티 까지 비교를 하게 되면 프로퍼티 수가 많을 시 비교에 성능이 낭비될 수 있기 때문이다.
```ts
function shallowEqual(objA: mixed, objB: mixed) {
	// 여기서 Object.is메서드를 통해서 먼저 값을 비교한다.
	if (is(objA, objB)) {
		return true;
	}

	...

	const keysA = Object.keys(objA);
	const keysB = Object.keys(objB);

	if (keysA.length !== keysB.length) {
		return false;
	}

	...

	for (let i = 0; i < keysA.length; i++) {
		const currentKey = keysA[i];

		// objA의 key에 대해 objB에 있는지 검사하고
		// 1depth의 값에 대해 Object.is메서드를 기준으로 비교한다.
		if (
			!hasOwnProperty.call(objB, currentKey) ||
			!is(objA[currentKey], objB[currentKey])
		) {
			return false;
		}
	
	}


	return true;
}
```

 이러한 특성을 고려하면 useMemo, React.memo등을 통해 재 렌더링을 방지해 성능을 개선하려고 해도, 중첩된 props는 같은 값 이어도 정확히 비교를 못하고 다르다고 판단하기 때문에 의도한대로 동작하지 않을 수 있다.