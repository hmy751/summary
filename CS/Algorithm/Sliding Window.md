## 슬라이딩 윈도우 알고리즘

입력 데이터(**배열 또는 문자열**)에서 마치 범위를 창 처럼 정의하고 **해당 범위(창)을 이동**하여 작업을 수행하면서 답을 찾아내는 방법으로 일반적인 brute force보다 **효율적인 방법**이다.

투 포인터는 크기가 관여되어 있고 슬라이딩 윈도우는 크기보다는 연속된 조합을 구하는게 특징이다.

## 판단하는 방법

- 제약 조건이 보통 N ≤ 10^6이다.
- 배열내에서 **연속된 범위**를 계속 유지하며 해결해 나간다.

## use case

정해진 범위가 있다면 그 범위와 조회해 나가는 인덱스를 활용해서 해당 윈도우에서 계산하여 결과 값을 도출해 나간다.

k를 정해진 범위에서 현재 인덱스에서 만족하는 값이 있다면 조건에 만족하는 계산을 한다.

윈도우가 범위를 벗어 나가면 현재 인덱스에서 범위를 빼 윈도우에 시작점을 찾고 조건에 맞게 값을 빼나간다.

```jsx
for (let i = 0; i < s.length; i++) {
	if (i >= k) {
		if (vowels.includes(s[i - k])) {
			currentCount--;
		}
	}
	
	if (vowels.includes(s[i])) {
		currentCount++;
	}
...
```

[Sliding Window Technique - GeeksforGeeks](https://www.geeksforgeeks.org/window-sliding-technique/#how-to-identify-sliding-window-problems)