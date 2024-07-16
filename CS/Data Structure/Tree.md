

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
