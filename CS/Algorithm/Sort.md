# Bubble Sort
---
버블 정렬은 서로 인접한 두 원소를 모두 비교하여 정렬하는 방식이다. 가장 기본적이고 간단한 정렬 알고리즘으로 시간 복잡도가 커서 거의 사용하지는 않는다.
in-plce 알고리즘 방법중 하나로 추가 메모리를 사용하지는 않는다.

시간 복잡도 - O(N2), O(N) 모두 정렬되어 있을 경우
공간 복잡도 - O(1)

https://github.com/hmy75d/data-structure-code/blob/main/src/Algorithm/bubbleSort.ts

# Selection Sort
---
선택 정렬은 첫 번째 위치부터 최소값이 있을 자리를 선택하고, 그 이후의 원소들을 차례대로 조회하며 선택한 자리에 최소값이 올수 있도록 지정하며 정렬해 나가는 방식이다.
in-place 알고리즘 방법중 하나로 추가 메모리를 사용하지 않는다.

시간 복잡도 - O(N2)
공간 복잡도 - O(1)

https://github.com/hmy75d/data-structure-code/blob/main/src/Algorithm/selectionSort.ts

# Insertion Sort
---
삽입 정렬은 왼쪽에 정렬한 배열을 두고, 그 이후의 원소들을 차례대로 조회해 나가며 정렬한 배열의 삽입될 위치를 찾아 삽입해 나가며 정렬을 하는 방식이다.
대부분의 요소들이 이미 정렬되어 있는 경우에 효율적일 수 있다. 하지만 비교적 많은 요소들의 이동을 포함할 수 있다.

시간 복잡도 - O(N) 최선일 경우, 이외는 O(N2)
공간 복잡도 - O(1)

https://github.com/hmy75d/data-structure-code/blob/main/src/Algorithm/insertionSort.ts

# Merge Sort
---
병합 정렬은 분할 정복(Divide and Conquer)알고리즘 중 하나이다.
병합 정렬은 하나의 리스트를 두 개의 균등한 크기로 분할하고 분할된 리스트를 정렬한 다음, 두 개의 정렬된 리스트를 다시 합하여 전체가 정렬되게 하는 방법이다.

in-place알고리즘이 아니며 병합할 새로운 배열이 필요하다. 또 요소들이 많으면 이동 횟수가 많아 시간적 낭비가 크다.

시간 복잡도는 병합하는 단계의 수 즉 호출의 깊이는 O(logN)이며, 여기에 각 호출에서 n/2인 배열을 
데이터의 분포의 영향없이 시간 복잡도가 동일하다.

시간 복잡도 - O(NlogN)
공간 복잡도 - O(N)

https://github.com/hmy75d/data-structure-code/blob/main/src/Algorithm/mergeSort.ts