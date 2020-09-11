## 股票買賣問題 Best Time to Buy and Sell Stock

### TL;DR

股票買賣在 LeetCode 上有一系列問題，這裡整理一個統一的方法解決這類型的問題

### 模板

股票買賣問題可以透過 Dynamic Programming 的 Tabulation 來解，所以這個問題本質上其實是一個窮舉問題，可以將以下的思考流程與**利用 Tabulation 來優化窮舉的思考流程**互相對照。

* 對於每一個股票買賣問題都有 **3 個 Options, 3 個 States**
	*  3 個 Options - 每一天都可以決定 Buy, Sell, Do Nothing
	*  3 個 States - 狀態由三個變數決定，天數、最多交易次數、持有狀態。分別以 `i`, `k`, `0/1` 表示
* `dp[i][k][0/1]` 的意義是：總共有 k 天，最多可以交易 k 次，最後持有狀態為 0/1 的最大獲利。則最終答案為 `dp[n - 1][k][0]`
* 狀態轉移方程式 - 因為狀態轉移方程式本來是由遞迴函數中的狀態轉移過程推導來的，但解決股票買賣問題採取直接使用 Dynamic Programming，所以狀態轉移方程式以直接理解方式推導。若利用遞迴中的狀態轉移關係會比較好敘述。遞迴中的狀態轉移是由第 `i - 1` 天轉移到第 `i` 天，在 Dynamic Programming 中 `i` 代表的是總共有 `i` 天的子問題。
	* `dp[i][k][0] = Math.max(dp[i - 1][k][0], dp[i - 1][k][1] + prices[i])`
	* `dp[i][k][1] = Math.max(dp[i - 1][k][1], dp[i - 1][k - 1][0] - prices[i])`
	* 以買股票時計算交易次數，換成賣股票時計算交易次數也可以
* Base Cases - 分為兩種類別，開始前與未進行交易
	* `dp[-1][k][0] = 0` - Because `i` starts at 0
	* `dp[-1][k][1] = -Infinity` - Because it's impossible
	* `dp[i][0][0] = 0` - Because `k` starts at 1
	* `dp[i][0][1] = -Infinity` - Because it's impossible

#### 重點整理

	Base Case：
	dp[-1][k][0] = dp[i][0][0] = 0
	dp[-1][k][1] = dp[i][0][1] = -Infinity
	
	狀態轉移方程式：
	dp[i][k][0] = max(dp[i-1][k][0], dp[i-1][k][1] + prices[i])
	dp[i][k][1] = max(dp[i-1][k][1], dp[i-1][k-1][0] - prices[i])