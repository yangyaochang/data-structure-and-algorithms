## 回溯法 Backtracking

### 回溯法的邏輯

逐一嘗試各種可能 (enumeration of candidates 列舉各種可能性, enumeration 列舉)，在嘗試過程中若發現路徑不為解或已窮盡所有可能，退回前一步驟，重新嘗試其他可能。

### 解題重點

Backtracking 是一種演算法，也就是一種邏輯。以我目前的解題經驗來看，通常被應用在窮舉。窮舉的邏輯是一個樹狀結構，窮舉的過程是 DFS on Tree，所以窮舉可以透過 Recursion 與 Backtracking 來實現。

#### 如何分析一個窮舉過程

建立窮舉邏輯的樹狀結構時，思考窮舉過程的三個元素：

* 路徑 (path) - 已經做的選擇
* 選擇 (options) - 目前可以做的選擇
* 終止條件 (base cases) - 遞迴終止的條件

窮舉的邏輯主要分為兩個層面

* 第一個層面 (做選擇) - 調用 Recursion，調用 Recursion 代表做了一個選擇，往下一個選擇移動。
* 第二個層面 (窮舉各種選擇) - 是在 Recursion 中利用 Iteration 窮舉當下的所有選擇。Backtracking 會與 Iteration 在同一個 level 發生，也就是在 recursion 中。Backtracking 的邏輯就出現在

	1. 做選擇
	2. 調用遞迴
	3. 取消選擇 

簡而言之，做選擇就是在樹狀結構上執行 DFS，做完選擇，狀態改變，掉用遞迴，前往下一個狀態。
	
```
let result = []
const backtrack = (path, options) => {
	if (base case) {
		result.push(path)
		return
	}
	
	for (option in options) {
		path.push(option) // 做選擇
		backtracking(path, options) // 調用遞迴
		path.pop() // 取消選擇
	}
}
```

	            
	            
全排列範例
用 array 來存路徑 (已經做的決定)
用 includes 來減持這個決定是否做過
不改變本來的決定 set