# 백트래킹

백트래킹은 **모든 경우의 수를 조회하며 조건에 맞는 경우의 수를 수집**하여 답을 구해나가는 알고리즘이다.

조회 과정은 DFS를 활용해 탐색해 나간다. 단순 DFS와 차이점은 찾아나가는 과정에서 모든 경우의 수를 조회하기 위해 **dfs내에서 0부터 for 반복문을 조회**하고, 조회하는 과정에서 경우의 수중 **선택된 경우의 중복을 차단하기 위해 선택과 해제의 과정을 거쳐** 조회해 나간다.

그리고 comb데이터를 새로운 데이터로 생성하여 전달하는 것 보다, **선택 해제 과정이 메모리 관점에서** 더 효율적이다

```jsx
combinationSum3Helper([...combs, nums[i]], sum + nums[i], i + 1);

for (let i = idx; i < 9; i++) {
        combs.push(nums[i]); // 선택
        combinationSum3Helper(combs, sum + nums[i], i + 1);
        combs.pop(); // 선택 해제
      }
```

# 탬플릿

순열은 **순서가 중요**하므로 **항상 0부터 탐색해도 문제가 없으며**, 조합은 **순서가 중요하지 않아 중복된 요소의 조합도 같은 경우의 수**로 **0부터가 아니라 다음 인덱스부터 시작하여 탐색하여 제한할** 수 있다.

```jsx
    for (let i = idx; i <= 9; i++) {
      dfs([...comb, i], sum + i, i + 1); // i+1로 시작점 업데이트
    }
    // 또는 index + 1 로 넘어간다 
    for (let i = 0; i < current.length; i++) {
      dfs(index + 1, comb + current[i]);
    }
    
    
    
    for (let i = 0; i < nums.length; i++) {
      if (used[i]) continue; // 이미 사용한 숫자는 건너뜀
      used[i] = true;
      dfs([...perm, nums[i]]);
      used[i] = false;
    }
```

# 그래프 DFS와 차이점

백트래킹도 DFS를 활용한다는 점에서 비슷하지만 차이점은 **백트래킹은 모든 경우의 수를 조회**해 나간다는 점이고, **그래프는 연속적으로 연결된 노드를 바탕**으로 조회해 나간다.

그래프 DFS는 dfs과정에서 처음부터 조회하는 **반복문을 사용하는건 똑같지만,** **visited의 선택, 해제보다는 visited여부와 연결성 여부를 확인**하고 DFS를 실행하며 조회해 나간다.

그리고 백트래킹과 달리 연결성이 중요하기 때문에 DFS외부에서 **처음 실행시 각 지점에서 반복문을 통해 각 지점에서 모두 시작 될 수 있도록 조회**해 나간다.

```jsx
list visited

const dfs = (index) => {
	visited[index] = true;

	for (let i = 0; i < list.length; i++) {
		if (visited[i]) continue;
		dfs(i);
	}

}

for (let i = 0; i < list.length; i++) {
	if (visited[i]) continue;
	dfs(i);
}

```

# 오답 케이스

- **Leetcode - 17. Letter Combinations of a Phone Number**
    
    [Letter Combinations of a Phone Number - LeetCode](https://leetcode.com/problems/letter-combinations-of-a-phone-number/)
    
    - 정답 코드
        
        ```jsx
        var letterCombinations = function(digits) {
          if (!digits) return [];
            
          const dic = {
            '2': 'abc',
            '3': 'def',
            '4': 'ghi',
            '5': 'jkl',
            '6': 'mno',
            '7': 'pqrs',
            '8': 'tuv',
            '9': 'wxyz'
          };
          
          const result = [];
          // digits 23
          // current abc, def
          
          const dfs = (index, comb) => {
            if (index === (digits.length)) {
              result.push(comb);
              return;
            }
            const current = dic[digits[index]];
            
            for (let i = 0; i < current.length; i++) {
              dfs(index + 1, comb + current[i]);
            }
          };
          
          dfs(0, '');
               
          return result;
        };
        ```
        
        여기선 comb는 문자열이며 별도의 해제가 필요하지 않아 새로운 문자열을 전달한다.
        
- **Leetcode - 216. Combination Sum III**
    
    [Combination Sum III - LeetCode](https://leetcode.com/problems/combination-sum-iii)
    
    - 해당 문제는 조건을 만족하는 숫자의 조합을 구하는 경우로 백트래킹으로 접근한다.
        
    - 선택 해제 과정에서 […comb] 참조 복사를 하던가 아니면 이렇게 push pop 처리를해야 한다. 그리고 push pop 처리를 해도 answer에 참조 복사를 해야 변경되지 않은 정답이 제대로 저장된다.
        
    - 조합 문제로 중복을 제거해야 한다. **반복문의 시작을 current로 시작하고, i + 1**로 넘어가야한다.
        
    - 정답 코드
        
        ```jsx
        /**
         * @param {number} k
         * @param {number} n
         * @return {number[][]}
         */
        var combinationSum3 = function (k, n) {
          const answer = [];
        
          const backtrack = (current, comb) => {
            const sum = comb.reduce((acc, cur) => acc + cur, 0);
        
            if (comb.length >= k) {
              if (sum === n) {
                answer.push([...comb]); // 결과는 배열 복사본 저장
              }
              return;
            }
        
            for (let i = current; i <= 9; i++) {
              if (comb.includes(i)) continue; // 중복 방지
              comb.push(i); // 값 추가
              backtrack(i + 1, comb); // 현재 배열 그대로 전달
              comb.pop(); // 값 제거 (선택 해제)
            }
          };
        
          backtrack(1, []);
          return answer;
        };
        
        ```