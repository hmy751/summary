# 자료구조

## 자료구조 개념

보통 프로그렘에서는 정보를 출력하기 위해 데이터를 처리한다. 이때 자료구조는 데이터를 쉽게 이용할 수 있도록 하는 데이터 간의 논리적인 관계를 말한다.
이 자료구조에 따라 프로그램의 성능이 좋아지거나 나빠질 수 있다.

대표적으로는 아래와 같은 자료구조가 있다.

- Array
- Linked List
- Stack
- Queue
- Tree
- Graph

# Array, Linked List

선형 리스트(linear list)는 어떤 순서에 의해 나열된 데이터가 여러 개인 구조를 말한다. 그리고 선형 리스트를 구현하는 방법에는 배열(array)과 연결 리스트(linked list)가 있다.

## Array

---

배열(array)은 데이터를 순서대로 나열한 구조로, 첫 번째 데이터로부터 떨어진 상대적인 위치로 나머지 데이터를 참조한다.  
인덱스(index)로 해당 데이터에 접근할 수 있으며, 해당 인덱스를 알면 시간 복잡도는 O(1)가 된다.

### 데이터 삽입, 삭제

배열에서는 데이터를 삽입, 삭제 과정에서 해당 데이터의 공간을 위해 조정하는 작업이 필요하다.

데이터를 삽입할 경우 맨 뒤에 데이터를 삽입하는 경우는 빈 공간을 만들어 데이터를 삽입하면 되고 시간 복잡도는 O(1)이 된다. 이 경우를 제외하고는 해당 데이터가 들어갈 공간을 만들기 위해 해당 공간 뒤에 데이터를 뒤로 이동시키는 과정이 필요하며, 이 때 뒤로 이동한 데이터 만큼이며 시간 복잡도는 O(N)이다.

데이터를 삭제하는 경우 맨 뒤에 데이터를 삭제하는 경우는 시간 복잡도가 O(1)이 된다. 이 경우를 제외하고는 해당 데이터를 삭제하고 빈 공간을 채우기 위해 뒤에 데이터를 앞으로 이동하는 과정이 필요하며, 이때 데이터 이동 만큼이며 시간 복잡도는 O(N)이 된다.

배열에서 삽입과 삭제 동작에는 데이터를 옮기는 시간이 필요하다.

## Linked List

---

연결 리스트(linked list)는 각 데이터들을 노드형태로 저장하여 포인터로 연결하여 관리하는 구조다. 각 노드는 데이터 영역, 포인터 영역으로 구성되며 데이터 영역은 데이터 값을 포인터 영역은 다음에 연결될 데이터를 가리킨다.

연결 리스트에서 제일 첫 번째 노드를 Head라고 한다. 이 Head에서 시작하여 데이터들이 연결되며 가장 끝단에 노드를 Tail이라고 한다. 그리고 가장 끝, Tail의 포인터 영역은 Null을 가리키며 리스트가 구성된다.

### Singly Linked List

단순 연결 리스트(singly linked list)는 노드에 포인터 영역이 하나인 연결 리스트로 가장 많이 사용되는 기본적인 연결 리스트 형태이다.

#### 데이터의 탐색, 삽입, 삭제 및 시간 복잡도

배열과 달리 연결 리스트의 검색은 정해진 인덱스에 데이터가 있지 않고 포이터로 연결되어 있기 때문에 시간 복잡도는 O(N)이다.
데이터를 삽입하는 경우 끝에 생성한 노드의 연결만 바꾸면 되기 때문에 시간 복잡도는 O(1)이며,
데이터를 삭제하는 경우에는 끝에 있는 노드전의 노드의 위치가 필요하고, 찾은 다음에는 해당 노드의 연결만 바꾸면 되기 때문에 시간 복잡도는 O(N)이 된다.

#### [구현코드](https://github.com/hmy75d/data-structure-code/blob/main/src/dataStructure/singlyLinkedList.ts)

### Doubly Linked List

이중 연결 리스트는 단순 연결 리스트와 구조는 비슷하지만, 각 리스트의 노드가 다음 노드뿐만 아니라 이전 노드도 가리키는 연결 리스트다.

#### 데이터의 탐색, 삽입, 삭제 및 시간 복잡도

이중 연결 리스트는 탐색의 경우 포인터를 따라서 노드를 찾아가기 때문에 시간 복잡도는 O(N)이 된다.
데이터를 삽입하는 경우 맨 뒤에 있는 노드에 새로운 노드를 추가하기 때문에 시간 복잡도는 O(1)이 된다.
데이터를 삭제하는 경우 맨 뒤에 있는 노드가 이전 노드를 가리키고 있기 때문에 바로 연결만 바꿔주면 돼며 시간 복잡도는 O(1)이 된다.

#### [구현코드](https://github.com/hmy75d/data-structure-code/blob/main/src/dataStructure/doublyLinkedList.ts)

# Stack, Queue

## Stack

---

스택(stack)은 선형 자료구조의 일종으로 데이터의 삽입과 삭제가 한쪽 끝에서만 일어난다.
최신 데이터가 들어가 삽입되고, 먼저 나가는 후입선출(Last-In First-Out)의 구조다.

### [구현코드](https://github.com/hmy75d/data-structure-code/blob/main/src/dataStructure/stack.ts)

## Queue

---

큐(queue)는 선형 자료구조의 일종으로 데이터의 삽입과 삭제가 양 끝에서 일어난다.
먼저 들어간 데이터가 가장 먼저 나가는 선입선출(First-In First-Out)의 구조다

### [구현코드](https://github.com/hmy75d/data-structure-code/blob/main/src/dataStructure/queue.ts)

# Tree

트리(tree)는 비선형 자료구조로 계층적 관계를 나타내는 자료구조이다.

- 구성요소
  - Node: 노드(node)는 트리를 구성하는 요소를 의미한다.
  - Edge: 간선(edge)는 노드와 노드를 연결하는 선을 의미한다.
  - Root Node: 루트 노드(root node)는 트리 최상단에 위치하며 시작되는 노드다.
  - Terminal Node, Leaf Node: 단말 노드(terminal node) 또는 리프 노드(leaf node)라 하며, 트리 맨 아래에 놓여있는 노드를 의미한다.
  - Internal Node: 트리에서 단말 노드를 제외한 모든 노드를 내부 노드(internal node)라고 한다.
  - Subtree: 서브 트리(subtree)는 임의 노드와 그 노드 아래에 있는 노드로 이루어진 트리를 서브 트리라고 한다.
  - Level: 루트 노드에서 임의의 노드까지 방문한 노드의 수를 레벨(level)이라 한다.
  - Depth: 트리의 최대 레벨을 깊이(depth)라 한다.

## Binary Tree

이진 트리(binary tree)는 모든 노드들의 자식노드가 두개 이하인 트리를 말한다. 루트 노드를 중심으로 나누어진 서브트리도 모두 이진 트리이다.

이진 트리 중에서도 단말 노드를 제외한 나머지 노드가 모두 두개의 자식노드를 가지면 완전 이진 트리(complete binary tree)라고 한다.

완전 이진 트리 중에서도 모든 노드가 채워진 이진 트리를 포화 이진 트리(full binary tree)라고 한다.

### 순회

순회 방법에는 preorder(전위 순회), inorder(중위 순회), postorder(후위 순회) 총 3가지가 있다.

전위 순회는 루트 노드 부터 왼쪽 서브 트리, 오른쪽 서브 트리 순서로 값을 조회한다.

[구현 코드](https://github.com/hmy75d/data-structure-code/blob/main/src/dataStructure/binaryTree.ts#L22)

중위 순회는 왼쪽 서브 트리 부터 루트 노드, 오른쪽 서브 트리 순서로 값을 조회한다.

[구현 코드](https://github.com/hmy75d/data-structure-code/blob/main/src/dataStructure/binaryTree.ts#L41)

후위 순회는 왼쪽 서브 트리 부터 오른쪽 서브 트리, 루트 노드 순서로 값을 조회한다.

[구현 코드](https://github.com/hmy75d/data-structure-code/blob/main/src/dataStructure/binaryTree.ts#L60)

## Binary Search Tree

이진 탐색 트리(binary search tree)는 같은 데이터를 갖는 노드가 없으며, 왼쪽 서브 트리에 있는 데이터는 현재 노드의 데이터보다 작고, 오른쪽 서브 트리에 있는 데이터는 현재 노드의 데이터보다 큰 이진 트리이다.
데이터의 삽입, 삭제, 검색 등이 자주 발생하는 경우에 효울적이다.

### 데이터의 삽입, 삭제
