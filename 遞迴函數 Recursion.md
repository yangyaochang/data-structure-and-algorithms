## Recursion

### 定義

### 為什麼會有 Recursion

### Recursion 函數的組成

### When should we use recursion?

Common signs  
    
* Branching cases
* Deeply nested data structures - traversing trees and graphs
* Unknown amount of loops needed
* Iterative solution is complex

Divide and Conquer 與 Recursion 的差別 - 我的理解是這樣，Divide and Conquer 是分析問題、規劃問題解決步驟的一個方法，規劃完的邏輯思維要能夠用程式表達。Recursion 就是程式運算的一種方式，Divide and Conquer 可以運用 Recursion 來實現解決問題的邏輯思維，但不僅限於只能使用 Recursion。

看到樹狀結構可以思考是否能使用 recursion。樹狀結構不僅限於 tree data structure，也包含解題邏輯為樹狀結構。因為在樹上的 DFS 可以透過 recursion 實現。

**以我目前的經驗回想，遇到 DFS on tree, graph、解題邏輯為樹狀結構 (Divide and Conquer、窮舉各種可能) 的時候會用到 recursion**。 

###Types of recursion

* Single Recursion - 遞迴函數在函數內部只調用自身函數一次 (e.g. 利用 bottom-up 求費氏數列)
* Multiple Recursion - 遞迴函數在函數內部調用自身函數多次 (e.g. 利用 top-down 求費氏數列)

> Bottom-up - Starts at base case and ends at input case，通常是 single recursion。**透過 return value 將解傳上去**。      
> Top-down - Starts at input case and ends at base case，Single 或 Multiple recursion 都有可能。**透過參數將解傳下去**。

###How to implement recursion - Helper function

####概念

* 在演算法的函數內定義一個 private function to handle recursive calls，這個 private function 就稱為 helper function (因為是 private function，所以可以自由定義需要的參數 (flexible)，此為利用 helper function to implement recursion 的優點)
* 代表演算法的函數本身則稱為 wrapper function，用來調用演算法本身
* 在 wrapper function 內根據需求 instantiate scope variables (scope variable 指的是 wrapper function 內定義的變數，在函數內任何地方都可以被存取)，用來 hold result 與 track state (state 為 values used to track progress of the recursive function，recursion 是否有逐漸往終止條件收斂。scope variable 或 helper function 的 parameter 都可以用來 track state)
* Wrapper function 還負責調用 helper function 與 return result。

####Steps of implementing recursion with helper function

用 helper function 建立 recursion 具有步驟明確的優點 (structured step-by-step process)

1. Instantiate scope variables
2. Return result
3. Define helper function (helper function 需要的參數可以看 diagram)
4. Invoke helper function
5. Define base case
6. Define recursive case

Bottom-up 方法求費氏數列        

	function nthFibonacci(n) {
		let fibonacci = [0, 1]
		
		const searchFib = (i) => {
			if (i > n) {return}
			fibonacci[i] = fibonacci[i - 2] + fibonacci[i - 1]
			searchFib(i + 1, n)
		}
		
		searchFib(2)
		return fibonacci[n]
	}
	
###How to implement recursion - Recursion w/ Side Effects

####概念

* Side effects are extra input parameters that can be passed into the function。也就是為演算法函數定義額外的參數來取代 helper function 方法中 scope variables 的用途 (hold results, track state)
* side effects 是額外定義的參數，只會在演算法內部調用 recursive calls 的時候才會為 side effects 賦值。演算法本身被調用的時候 side effects 並未被賦值。在 JavaScript 中，未被賦值的參數的值是 undefined，可以利用 short-circuit evaluation 來決定 side effects 的初始值
* 調用函數本身來 handle recursive calls
* 因為沒有 scope variables to hold results，通常 base case 需要 return results

####Steps of implementing recursion with side effects

1. 先利用 helper function 方法建立 recursion，再轉為 side effects 方法
2. 定義 side effects 參數 to hold results and track state，因為沒有 scope variables，所以運算後的結果當作參數傳給下一個 recursive call
3. Instantiate side effects at the begining of the function 來決定當函數第一次被調用時，未被賦值的 side effects 的初始值
4. Define base case，沒有 scopr variable，所以 typically requires a return statement
5. Define recursive case，傳入 side effects，typically requires a return statement as well。
	* 如果是 multiple resursion：
		* If there is a loop, the return statement is typically found outside the loop after it completes
		* If there isn't a loop, there's typically a set number (a set number = a fixed number) of recursive calls made. Then the results are combined and returned up one level
	* 如果是 single recursion：通常在最後一行會有一個 return statement
 		
以費氏數列為例

	function nthFibonacci(n, i, fibonacci) {
		fibonacci =. fibonacci || [0, 1]
		i = i || 2
		
		if (n < 2) {return fibonacci[n]}
		
		fibonacci[i] = fibonacci[i - 1] + fibonacci[i - 2]
		
		if (i === n) {return fibonacci[i]}
	
		return nthFibonacci(n, i + 1, fibonacci)
	}

###How to implement recursion - Pure Recursion

####概念

* No helper function, No side effects - 所以我的理解是，需要靠 return statement 返回結果，且無法 track state。

####Steps of implementing recursion with pure recursion

1. 先利用 helper function 方法建立 recursion，再轉為 side effects 方法，再化簡為 pure recursion
2. Define base case
	* initialize and return the result of the base case
3. Define recursive case
	* Define operations to build up the result from the base case
	* return result

以費氏數列為例

	function nthFibonacci(ni) {
		if (n < 2) {
		return n
		}
	
		return nthFibonacci(n - 1) + nthFibonacci(n - 2)
	}

