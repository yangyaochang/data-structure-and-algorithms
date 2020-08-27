## 動態規劃 Dynamic Programming

Dynamic Programming (DP) 是一種優化演算法的方式。可以利用 DP 優化的演算法多半解決的問題是求極值 (例外，費氏數列)，而求極值的方法就是窮舉，窮舉出所有可能找到最大或最小值。

求極值靠窮舉，**窮舉的邏輯是樹狀結構，窮舉的過程是 DFS on Tree**，可以透過 Recursion 與 Backtracking 來實現。若窮舉的樹狀邏輯具有

* Overlapping subproblems
* Solutions to smaller problems can be used to solve larger problems incrementally

這兩點特性時，可以透過 DP 來優化。若是發現有 **Overlapping subproblems 可以透過 Memoization 優化**，若是發現有 **Solutions to smaller problems can be used to solve larger problems incrementally 可以透過 Tabulation 優化**。基本上兩種方式殊途同歸，可以用來解同一個問題。會發現哪一個特性取決於分析問題的方法，可以優化的窮舉以 Top-down Approach 分析時會發現 Overlapping subproblems；以 Bottom-up Approach 分析時會發現 Solutions to smaller problems can be used to solve larger problems incrementally。

依據我的經驗，通常在解題時建立窮舉邏輯的時候，都是以 Top-dowm Approach 推演的，不容易發現 Solutions to smaller problems can be used to solve larger problems incrementally 這個特性，所以下面討論如何利用推演完的窮舉邏輯轉換成利用 Tabulation 方式優化的 Dynamic Programming。

### Tabulation

#### 使用 Tabulation 優化窮舉過程的思考流程

要注意到，本來推演的窮舉邏輯是 Top-down Approach，但是 Tabulation 是以 Bottom-up Approach 分析問題。所以要以 Tabulation 優化窮舉要從底部思考起。

1. 找到狀態和選擇
2. 定義 dp 陣列的意義，以狀態為陣列變數 (index)
3. 列出狀態轉移方程式 (描述狀態之間的關係)
4. 以狀態作為 dp 陣列的 index，為 Base Case 賦值，以狀態轉移方程式求得剩餘狀態的值

#### 怎麼找到窮舉邏輯中的狀態與 dp[i] 的定義
 
**簡單來說 Recursion 的參數就是用來描述狀態的**。

因為 Recursion 函數的參數會用來 Track State, Hold Result，所以從 Recursion 函數的參數找到窮舉邏輯中的狀態。而 dp[i] 的定義就是遞迴函數的意義。

#### 怎列出狀態轉移方程式 - 窮舉邏輯中的狀態轉移

Recursion 邏輯的特色就是 **Different State, Same Policy**，在不同狀態下執行一樣的運算。遞迴函數不只被應用在窮舉，例如 Divide and Conquer。

窮舉邏輯在做的事情其實就是在不同選擇的狀態 (Different State) 中窮舉各種選擇 (Same Policy)，符合遞迴函數的特性可以用遞迴實現，實現方法是在一個選擇後的狀態中列舉當下所有選擇，做了選擇之後，以選擇後的狀態調用下一個 Recursion，這個過程稱為**狀態轉移**。

所以 Tabulation 所需要的狀態轉移方程式，也是透過遞迴函數之間如何狀態轉移推導而來。

#### 如何列出狀態轉移方程式

我的經驗可以直接從 Recursion 怎麼調用來推導，不一定需要下面的步驟。

1. 找到 Base Case
2. 找到狀態
3. 找到選擇
4. 定義 dp 陣列的意義

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
