## 深度優先搜索與廣度優先搜索 DFS and BFS on Tree and Graph

### 重點整理

* 以 `while` loop 與 Queue 實現 BFS
* 以 Recursion 實現 DFS
* 在 Graph 上多考慮 Redundancy 問題
* Tree 的解題重點通常在於在每一個 node 上要做什麼事情，還有要使用 Bottom-up 還是 Top-down

### Implementing BFS on Binary Tree

#### 邏輯

1. Add the starting node (root node) to the queue
2. while queue is not empty
	1. 利用變數 currentNode 指向正在操作的 node (to mark which node you are operating on) - `dequeue()`
	2. Visit the currentNode
	3. Check currentNode 是否有左子節點與右子節點，有的話 Add them to the queue
	
	```
	if (currentNode.left) {queue.push(currentNode.left)} 
	```
	```         
	if (currentNode.right) {queue.push(currentNode.right)}
	```
	
```
const queue = []
	
queue.push(root)
while (queue.length > 0) {
	const current = queue.shift()
	// Visit the current node
	if (current.left) {queue.push(current.left)}
	if (current.right) {queue.push(current.right)}
}
```

### Implementing DFS on Binary Tree

使用 Recursion 比較適合，利用 Iteration 來建立 DFS 時，Preorder, Inorder, Postorder 三種方法會很不一樣，並不只是換指令的順序而已。

#### 邏輯

1. 定義一個 helper function traverse()，接受一個參數 currentNode
	1. Base case: `currentNode === null`
	2. Recursive case:
		* Visit the currentNode 
		* 以左子節點與右子節點作為參數調用 recursive function     
		     
		> 在 Recursive Calls 之前、之中、之後 visit currentNode 決定是 Preorder, Inorder, 還是 Postorder 
2. Invoke the helper function with the starting node (root node) as the input parameter

```
const traverse = (current) => {
	if (current === null) {return}
	// Proorder
	traverse(current.left)
	// Inorder
	traverse(current.right)
	// Postorder
}
```

### Implementing BFS on Graph

概念基本上與 BFS on Tree 類似，但是 Graph 需要多考慮 Redundancy 的問題，也就是同一個 Vertex 可能被重複走訪，需要透過一個 set to keep track of the vertex that has been visited

#### 邏輯

1. Add the starting node to the queue
2. Add the starting node to the set
3. While queue is not empty
	1. 利用變數 currentNode 指向正在操作的 node (to mark which node you are operating on) - `dequeue()`
	2. Visit the currentNode
	3. Loop through all neighbors
	4. Check neighbors 是否有被走訪過
	5. 沒有的話 add them to the queue, add them to the set

```
const queue = []
const visited = new Set()
	
queue.push(start)
visited.add(start)

while (queue.length > 0) {
	const current = queue.shift()
	// Visit the current node
	const neighbors = graph[current]
	for (let i = 0; i < neighbors.length; i++) {
		if (!visited.has(nieghbors[i])) {
			queue.push(neighbors[i])
			visited.add(neighbors[i])
		}
	}	
}
```

### Implementing DFS on Graph

概念基本上與 DFS on Tree 類似，但是 Graph 需要多考慮 Redundancy 的問題，也就是同一個 Vertex 可能被重複走訪，需要透過一個 set to keep track of the vertex that has been visited。

#### 邏輯

1. 定義一個 helper function `traverse()`，接受一個參數 currentNode
	1. Base case: currentNode has been visited
	2. Recursive case:
		* Visit the currentNode
		* Add the currentNode to the set
		* 以 neighbors 作為參數調用 Recursive Function 	 
2. Invoke the helper function with the starting node as the input parameter

```
const visited = new Set()

const traverse = (current) => {
	if (visited.has(current)) {return}
	
	// Visit the current node
	visited.add(current)
	const neighbors = graph[current]
	for (let i = 0; i < neighbors.length; i++) {
		traverse(neighbors[i])
	}
}
```

DFS on Graphs 也可以利用 `while` loop 與 Stack 來實現，將 BFS on Graph 的步驟中的 Queue 改成 Stack 即可。
