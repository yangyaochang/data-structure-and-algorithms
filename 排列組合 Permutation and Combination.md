## 排列組合

排列組合的解法基本上靠窮舉，窮舉的過程是一個樹狀結構，所以可以透過 DFS 搭配 Backtracking 的概念走訪樹狀結構得到所有的可能。DFS 靠 recursion 實現。

用 Recursion 來窮舉的時候，需要解決的問題是如何建立 recursion。有三個元素可以幫助思考如何建立 recursion 來窮舉。

* 路徑 (path) - 已經做的選擇
* 選擇 (options) - 目前可以做的選擇
* 終止條件 (base cases) - 遞迴終止的條件

## 排列 Permutation

定義：從給定個數的元素中取出指定個數的元素進行排列。元素相同但順序不同為不同的排列。(順序不同算不同)

* 是不是排列？         
如果是，要考慮順序。先取 1 再取 2 與先取 2 再取 1 是不一樣的。每次都從頭 loop through 所有選擇。
	```
	for (let i = 0; i < options.length; i++) {
	// 做選擇
	}
	```
* 同一元素可不可以重複使用？      
若是不能重複同一元素，用一個 `used` array 紀錄這個 index 有沒有用過。做決定時 `used[i] = true`，在 backtrack 時  `used[i] = false`。`if (used[i]) {continue}` 跳過這個選擇。
* oprions 裡面有沒有重複的元素？        
若有重複元素，在同一個 level 遇到重複的元素要忽略。先將 options 排序後，當 `used[i - 1] && nums[i] === nums[i - 1]` 時 `continue`

	```
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

* 是不是組合？      
如果是，不考慮順序。先取 1 再取 2 與先取 2 再取 1 是一樣的。定義一個 `start` 參數，loop through 選擇的時候從 `start` 開始，避免重複的組合。
	```
	for (let i = start; i < options.length; i++) {
	// 做選擇
	}
	```
* 同一元素可不可以重複使用？      
如果可以重複使用同一元素，要做下一個選擇的 `start` 不變。若是不能重複同一元素，要做下一個選擇時 `start + 1`。
* oprions 裡面有沒有重複的元素？        
若有重複元素，在同一個 level 遇到重複的元素要忽略。先將 options 排序後，當 `i > start && options[i] === options[i - 1]` 時 `continue`

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