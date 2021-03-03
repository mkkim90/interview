## 이진 트리 순회

	이진 트리는 (binary tree)는 각 노드가 최대 두 개의 자식을 갖는 트리를 말한다. 

	중위 (in-order), 후위(post-order), 전위(pre-order) 순회

### 중위 순회(in-order traversal)
- 왼쪽 가지, 현재 노드, 오른쪽 가지 순서로 노트를 방문
```
void inOrderTraversal(TreeNode node) {
	if (node != null) {
		inOrderTraversal(node.left);
		visit(node);
		inOrderTraversal(node.right);
	}
}
```

### 전위 순회(pre-order traversal) : 자식 노드보다 현재 노드를 먼저 방문
```
void preOrderTraversal(TreeNode node) {
	if (node != null) {
		visit(node);
		preOrderTraversal(node.left);
		preOrderTraversal(node.right);
	}	
}
```
### 후위 순회(post-order traversal) : 모든 자식 노드들을 먼저 방문한 뒤 마지막에 현재 노드를 방문
```
void postOrderTraversal(TreeNode node) {
	if (node != null) {
		postOrderTraversal(node.left);
		postOrderTraversal(node.right);
		visit(node);
	}
}
```
## 그래프

- 트리는 그래피의 한 종류이다. 
- 그래프는 단순히 노드와 그 노드를 연결하는 간선을 하나로 모아 놓은 것과 같음
- 방향성이 있을 수도 있고 없을 수도 있다. 
- 여러 개의 고립된 부분 그래프로 구성
- 사이클이 없는 그래프는 비순환 그래프라고 부름

### 인접 리스트 
모든 정점을 인접 리스트에 저장한다. 
예를 들어, (a, b) 간선은 두번 저장된다. 한번은 a 정점에 인접한 간선을 저장하고 다른 한번은 b에 인접한 간선을 저장
```
class Graph {
	public Node[] nodes;
}
class Node {
	public String name;
	public Node[] children;
}
```

### 인접 행렬

인접 행렬은 N x N 불린 행렬로써 matrix[I][j]가 true라면 i에서 j로의 간선이 있다는 뜻
0과 1을 이용한 정수 행렬을 사용할수도 있다. 

인접 행렬에서는 어떤 노드에 인접한 노드를 찾기 위해서는 모든 노드를 전부 순회해야 알 수 있다.

### 그래프 탐색 

그래프를 탐색하는 일반적인 방법 두 가지로는 깊이 우선 탐색 (DFS - depth-first search)과 너비 우선 탐색(BFS - breadth-first search) 이 있다.

#### DFS : 해당 분기를 완벽하게 탐색, 깊게 탐색
```
Node 0
	Node 1
		Node 3
			Node 2
			Node 4
	Node 5
```

- 재귀 활용
```void search(Node root) {
	if (root == null) return;
	visit(root);
	root.visited = true
	for (Node n : root.adjacent) {
		if (n.vistied == false) {
			search(n);
		}
	}
}
```

#### BFS : 인접한 노드를 먼저 탐색 , 넓게 탐색
```
Node 0
Node 1
Node 4
Node 5
Node 3
Node 2
```

- 큐 활용
```
void search(Node root) {
	Queue queue = new Queue();
	root.marked = true;
	queue.enqueue(root);

	while (!queue.isEmpty()) {
		Node r = queue.dequeue();
		visit(r);
		for (Node node : r.adjacent) {
			if (n.marked == false) {
				n.marked == true;
				queue.enqueue(n);
			}
		}
	}
}
```

#### 양방향 탐색(bidirectional search)

-출발지와 도착지 사이에 최단 경로를 찾을 때 사용

- 동시에 너비 우선 탐색 
```
시간복잡도상 너비 우선 탐색보다 두배 빠르다
너비 우선 탐색의 경우 k개의 이웃노드를 d단계 접근한다면 O(k^d)가 되며
양방향 탐색의 경우 k개의 이웃노드를 d/2단계 접근하기에 O(k^(d/2)) 인 셈이다. 그러므로 두배 더 많은 탐색 가능
```
