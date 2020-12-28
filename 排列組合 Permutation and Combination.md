## 排列組合 Permutation and Combination

**排列組合的解題邏輯基本上靠窮舉，窮舉的邏輯是一個樹狀結構而窮舉的過程就是 DFS on Tree，所以可以透過 DFS 搭配 Backtracking 的概念走訪窮舉邏輯的樹狀結構得到所有的可能。DFS 靠 Recursion 實現**。

用 Recursion 來實現窮舉的時候，需要解決的問題是如何建立 Recursion。有三個元素可以幫助思考如何建立 Recursion 來窮舉。

* 路徑 (path) - 已經做的選擇
* 選擇 (options) - 目前可以做的選擇
* 終止條件 (base cases) - 遞迴終止的條件

### 解題步驟

1. 以上述三點建立窮舉的樹狀邏輯
2. **透過樹狀邏輯得到 Recursion 函數需要的參數與 Base Case**
3. 再利用模板確認細部邏輯

## 排列 Permutation

### 定義

從給定個數的元素中取出指定個數的元素進行排列。元素相同但順序不同為不同的排列。(順序不同算不同)

### 邏輯

因為是排列，先取 1 再取 2 與先取 2 再取 1 是不一樣的，所以除了路徑中已經被選過的元素不能再選以外 (通常排列不考慮同一元素可以重複使用)，所有選擇都在被考慮範圍。若選項中有重複的元素，在同一個 Level 相同元素的選項是被視為相同的，不需重複選取。

### 模板

使用模板前先考慮三個問題：

* 是不是排列？         
如果是，要考慮順序。先取 1 再取 2 與先取 2 再取 1 是不一樣的。每次都從頭 loop through 所有選擇。
	```
	for (let i = 0; i < options.length; i++) {
	// 做選擇
	}
	```
* 同一元素可不可以重複使用？      
若是不能重複同一元素，用一個 `used` array 紀錄這個 index 有沒有用過。做決定時 `used[i] = true`，在 backtrack 時  `used[i] = false`。`if (used[i]) {continue}` 跳過這個選擇。
* options 裡面有沒有重複的元素？        
若有重複元素，在同一個 level 遇到重複的元素應視為相同的選擇，要忽略。**先將 options 排序後**，當 `i > 0 && !used[i - 1] && nums[i] === nums[i - 1]` 時 `continue`(**`used[i - 1] === false` 因為有 Backtracking**，`used[i - 1] === true` 會發生在下一個 Level 的選擇但元素與上一個 Level 被選擇的元素一樣，此時 `options[i]` 是可以選擇的)
* 檢查 Backtracking

[LeetCode 47 花花答案參考](https://zxi.mytechroad.com/blog/searching/leetcode-47-permutations-ii/)

```
options = options.sort((a, b) => a - b)

dfs(state, path, used) {
	if (state === 終止條件) {遞迴終止}
	
	for (let i = 0; i < options.length; i++) {
		if (used[i]) {continue}
		used[i] = true
		path.push(options[i])
		dfs(state + options[i], path, used)
		path.pop()
		used[i] = false
	}
}
```

## 組合 Combination

定義：從給定個數的元素中取出指定個數的元素，不考慮排序。元素相同但順序不同依然為相同的組合。(順序不同算相同)

### 邏輯

因為是組合，先取 1 再取 2 與先取 2 再取 1 是一樣的，所以前面選過的元素後面不要再考慮。同一個元素是否可以重複選取會影響接下來可做的選擇。若選項中有重複的元素，在同一個 Level 相同元素的選項是被視為相同的，不需重複選取。

### 模板

* 是不是組合？      
如果是，不考慮順序。先取 1 再取 2 與先取 2 再取 1 是一樣的。定義一個 `start` 參數，loop through 選擇的時候從 `start` 開始，避免重複的組合。
	```
	for (let i = start; i < options.length; i++) {
	// 做選擇
	}
	```
* 同一元素可不可以重複使用？      
如果可以重複使用同一元素，要做下一個選擇的 `start` 不變。若是不能重複同一元素，要做下一個選擇時 `start + 1`。
* options 裡面有沒有重複的元素？        
若有重複元素，在同一個 level 遇到重複的元素應視為相同的選擇，要忽略。**先將 options 排序後**，當 `i > start && options[i] === options[i - 1]` 時 `continue`
* 檢查 Backtracking

```
options = options.sort((a, b) => a - b)
	
dfs(state, path, start) {
	if (state === 終止條件) {遞迴終止}
	
	for (let i = start; i < options.length; i++) {
		path.push(options[i])
		dfs(state + options[i], path, i)
		path.pop()
	}
}
```