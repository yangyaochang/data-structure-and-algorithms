## 動態規劃 Dynamic Programming

Dynamic Programming (DP) 是一種優化演算法的方式。可以利用 DP 優化的演算法多半解決的問題是求極值 (例外，費氏數列)，而求極值的方法就是窮舉，窮舉出所有可能找到最大或最小值。**所以動態規劃就是用來優化窮舉**。

求極值靠窮舉，**窮舉的邏輯是樹狀結構，窮舉的過程是 DFS on Tree**，可以透過 Recursion 來實現。若窮舉的樹狀邏輯具有

* Overlapping subproblems
* Solutions to smaller problems can be used to solve larger problems incrementally

> 排列組合也是利用窮舉的邏輯，但排列組合的窮舉因為每個 case 的 path 都是不同的，所以沒有 Overlapping subproblems，無法透過  Dynamic Programming 優化。排列組合的題目比較常遇到搭配 Backtracking 的技巧

這兩點特性時，可以透過 DP 來優化。若是發現有 **Overlapping subproblems 可以透過 Memoization 優化**，若是發現有 **Solutions to smaller problems can be used to solve larger problems incrementally 可以透過 Tabulation 優化**。基本上兩種方式殊途同歸，可以用來解同一個問題。會發現哪一個特性取決於分析問題的方法，可以優化的窮舉以 Top-down Approach 分析時會發現 Overlapping subproblems；以 Bottom-up Approach 分析時會發現 Solutions to smaller problems can be used to solve larger problems incrementally。

依據我的經驗，通常在解題時建立窮舉邏輯的時候，都是以 Top-dowm Approach 推演的，不容易發現 Solutions to smaller problems can be used to solve larger problems incrementally 這個特性，所以下面討論如何利用推演完的窮舉邏輯轉換成利用 Tabulation 方式優化的 Dynamic Programming。

### Tabulation

#### 使用 Tabulation 優化窮舉過程的思考流程

要注意到，本來推演的窮舉邏輯是 Top-down Approach，但是 Tabulation 是以 Bottom-up Approach 分析問題。所以要以 Tabulation 優化窮舉要從底部思考起。

1. 找到狀態和選擇
2. 定義 dp 陣列的意義，以狀態為陣列變數 (index)
3. 列出狀態轉移方程式 (描述狀態之間的關係)
4. 以狀態作為 dp 陣列的 index，帶入 dp[i] 的定義，為 Base Case 賦值，以狀態轉移方程式求得 (Iteration) 剩餘 case 的值

#### 怎麼找到窮舉邏輯中的狀態與 dp[i] 的定義
 
**簡單來說 Recursion 的參數就是用來描述狀態的**。

因為 Recursion 函數的參數會用來 Track State, Hold Result，所以從 Recursion 函數的參數找到窮舉邏輯中的狀態。而 dp[i] 的定義就是遞迴函數的意義。

> e.g. 以 LeetCode Coin Change 為例       
以 Top-down Approach 推導完窮舉的邏輯後，遞迴函數的參數是當下硬幣的總和，而遞迴函數的意義是當目標金額為 `amount` 時，湊齊目標金額所需的最少錢幣數。所以若要以 Tabulation 優化此窮舉，狀態是硬幣總和，dp[i] 的意義為當目標金額為 i 時，湊齊目標金額所需的最少錢幣數。將兩個概念合併在一起，以狀態的值套入 dp[i] 的定義 - 當目標金額為 i (0 - `amount`) 時，湊齊目標金額所需的最少錢幣數。
 
#### 怎列出狀態轉移方程式 - 窮舉邏輯中的狀態轉移

Recursion 是程式運算的一種方式，邏輯的特色就是 **Different State, Same Policy**，在不同狀態下執行一樣的運算。若是演算法的邏輯具有這種特色，可以透過遞迴實現，不只被應用在窮舉，例如 Divide and Conquer。

窮舉邏輯在做的事情其實就是在不同選擇的狀態 (Different State) 中窮舉各種選擇 (Same Policy)，符合遞迴函數的特性可以用遞迴實現，實現方法是在一個選擇後的狀態中列舉當下所有選擇，做了選擇之後，以選擇後的狀態調用下一個 Recursion，這個過程稱為**狀態轉移**。

所以 Tabulation 所需要的狀態轉移方程式，也是透過**遞迴函數之間如何狀態轉移**推導而來。

#### 如何列出狀態轉移方程式

我的經驗可以直接從 Recursion 怎麼調用來推導，不一定需要下面的步驟。

1. 找到 Base Case
2. 找到狀態
3. 找到選擇
4. 定義 dp 陣列的意義

#### 找到 Base Cases

我自己的經驗是，有時候會不確定 Base Cases 的值為多少。這時候可以回到 dp[i] 的定義去思考。Base Cases 代入 dp[i] 的定義後值會是多少。

> 以 LeetCode Unique Paths 為例       
> dp[i][j] 代表的是在有 i + 1 rows, j + 1 columns 的方格中從 [0, 0] 走到 [i, j] 的路徑數，那麼 Base Case dp[0][0] 就是從 [0, 0] 走到 [0, 0] 的路徑數，也就是 1。所以 dp[0][0] = 1

#### Tabulation 模板

```
// 初始化 base case
dp[0][0][...] = base
// 進行狀態轉移
for 狀態1 in 狀態1的所有取值：
    for 狀態2 in 狀態2的所有取值：
        for ...
            dp[狀態1][狀態2][...] = 求極值(選擇1，選擇2...)
```

e.g. 以 LeetCode #322 Coin Change 為例

當 amout = 11，coins = [1, 2, 5] 

以 Top-down Approach 推導，11 分別減去 1, 2, 5 後得到的狀態 10, 9, 6 的結果都是使用了一枚 coin，如果改以 Bottom-up Approach 推導，一枚 coin 的結果就是未知的必須由 Base Case 推到 10, 9, 6 的狀態再得到 11 的最終答案。

	var coinChange = function(coins, amount) {
		let cache = {}
		    
		const add = (curSum) => {
			if (curSum > amount) {return Infinity}
			if (curSum === amount) {return 1}
			if (curSum in cache) {return cache[curSum]}
				
			let returnVal = []
			    
			for (let i = 0; i < coins.length; i++) {
				returnVal.push(add(curSum + coins[i])) 
			}
				
			cache[curSum] = Math.min(...returnVal) + 1
			return cache[curSum]
		}
		    
		return add(0) - 1
	};
	
* 狀態為目標金額，也就是 Tabulation 的狀態
* 遞迴函數 `add()` 代表的是當目標金額為 amount 時，所需要最少的硬幣數以湊齊 amount 的金額，所以 dp[i] 的定義就是當目標金額為 i 時所需要最少的硬幣數。也就是 dp[i] 的定義。

### Memoization
