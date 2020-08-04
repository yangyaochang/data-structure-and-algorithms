## 遞迴函數 Recursion

### 定義

一個在內部調用自身函數的函數稱為遞迴函數

### 為什麼會有 Recursion？

以 JavaScript 為例，當一個函數被調用的時候會建立一個 Function Execution Context 並放到 Call Stack 上。若在函數內部又調用了另一個函數，會有另一個 Function Execution Context 並放到 Call Stack 上。由於 Call Stack 遵守 Last In, First Out (LIFO)，所以內部被調用的函數會先被執行完，才會執行本來的函數。所以如果函數在內部調用的是自身函數，就可以形成遞迴。

### Recursion 函數的組成

一個遞迴函數的組成主要分兩個部分：

* Base Case - 遞迴函數的終止條件。當終止條件被滿足，則遞迴終止。
* Recursive Case - 當終止條件還未滿足時，執行 Recursive Case，並逐漸往終止條件收斂。

> 遞迴函數一定要有終止條件，否則會產生堆疊溢位 (Stack Overflow)


### When Should We Use Recursion?

**Different States, Same Policy** 是遞迴函數執行的特色。基本上遞迴函數就是在不同的狀態下執行一樣的邏輯。

所以當演算法中出現要在 Different States 下重複執行 Same Policy 的時候，就可以使用遞迴函數。

**以我目前的經驗回想，遇到 DFS on Tree and Graph、解題邏輯為樹狀結構 (Divide and Conquer、窮舉各種可能)、Traversal on Linked List 的時候會用到 Recursion**。 

> Recursion 不僅限於在 Tree and Graph 這兩種資料結構中 DFS，也可以用於解題邏輯為樹狀結構時，因為在樹上的 DFS 可以透過 Recursion 實現。    
> 
> Divide and Conquer 與 Recursion 的差別     
> 我的理解是這樣，Divide and Conquer 是分析問題、規劃問題解決步驟的一個方法，規劃完的邏輯思維要能夠用程式表達。Recursion 就是程式運算的一種方式，Divide and Conquer 可以運用 Recursion 來實現解決問題的邏輯思維，但不僅限於只能使用 Recursion。


### Types of Recursion

* Single Recursion - 遞迴函數在函數內部只調用自身函數一次 (e.g. 利用 bottom-up 求費氏數列)
* Multiple Recursion - 遞迴函數在函數內部調用自身函數多次 (e.g. 利用 top-down 求費氏數列)

> 什麼是 Bottom-up, Top-down    
> Bottom-up - Starts at base case and ends at input case。這樣的解題邏輯通常需要**透過 return value 將解傳上去**。      
> Top-down - Starts at input case and ends at base case。這樣的解題邏輯通常需要**透過參數將解傳下去**。

### How to Implement Recursion - Helper Function

#### 概念

* 在演算法的函數內定義一個 private function to handle recursive calls。這個 Private Function 就稱為 Helper Function。       
(因為是 Private Function，所以可以自由定義需要的參數 (flexible)，此為利用 helper function to implement recursion 的優點)
* 代表演算法的函數本身則稱為 Wrapper Function，用來調用演算法本身。
* 在 Wrapper Function 內根據需求 instantiate scope variables，用來 hold result 與 track state。      
(Scope Variable 指的是 Wrapper Function 內定義的變數，在函數內任何地方都可以被存取)        
(State 為 values used to track progress of the recursive function。Scope Variable 或 Helper Function 的 Parameter 都可以用來 track state)
* Wrapper Function 還負責調用 Helper Function 與 return result。

#### Steps of Implementing Recursion with Helper Function

用 Helper Function 建立 Recursion 具有步驟明確的優點 (Structured Step-by-step Process)

1. Instantiate scope variables
2. Return result
3. Define helper function 
4. Invoke helper function
5. Define base case
6. Define recursive case

```
// Bottom-up 方法求費氏數列      
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
```
	
### How to Implement Recursion - Recursion w/ Side Effects

#### 概念

* Side effects are extra input parameters that can be passed into the function。也就是為演算法函數定義額外的參數來取代 Helper Function 方法中 Scope Variables 的用途 (hold results, track state)
* Side Effects 是額外定義的參數，只會在演算法內部調用 Recursive Calls 的時候才會為 Side Effects 賦值。演算法本身被調用的時候 Side Effects 並未被賦值。在 JavaScript 中，未被賦值的參數的值是 undefined，可以利用 Short-circuit Evaluation 來決定 Side Effects 的初始值
* 調用函數本身來 handle recursive calls
* 因為沒有 scope variables to hold results，通常 Base Case 需要 return results

#### Steps of implementing recursion with side effects

1. 先利用 Helper Function 方法建立 Recursion，再轉為 Side Effects 方法
2. 定義 Side Effects 參數 to hold results and track state，因為沒有 Scope Variables，所以運算後的結果當作參數傳給下一個 Recursive Call
3. Instantiate side effects at the begining of the function 來決定當函數第一次被調用時，未被賦值的 Side Effects 的初始值
4. Define base case，沒有 Scopr Variable，所以 typically requires a return statement
5. Define recursive case，傳入 Side Effects，typically requires a return statement as well。
	* 如果是 Multiple Resursion：
		* If there is a loop, the return statement is typically found outside the loop after it completes
		* If there isn't a loop, there's typically a set number (a set number = a fixed number) of recursive calls made. Then the results are combined and returned up one level
	* 如果是 Single Recursion：通常在最後一行會有一個 return statement

``` 		
// 以費氏數列為例
function nthFibonacci(n, i, fibonacci) {
	fibonacci =. fibonacci || [0, 1]
	i = i || 2
	
	if (n < 2) {return fibonacci[n]}
	
	fibonacci[i] = fibonacci[i - 1] + fibonacci[i - 2]
	
	if (i === n) {return fibonacci[i]}
	
	return nthFibonacci(n, i + 1, fibonacci)
}
```

### How to Implement Recursion - Pure Recursion

#### 概念

* No Helper Function, No Side Effects - 所以我的理解是，需要靠 return statement 返回結果，且無法 track state。

#### Steps of Implementing Recursion with Pure Recursion

1. 先利用 Helper Function 方法建立 Recursion，再轉為 Side Effects 方法，再化簡為 Pure Recursion
2. Define base case
	* Initialize and return the result of the base case
3. Define recursive case
	* Define operations to build up the result from the base case
	* Return result

```
//以費氏數列為例
function nthFibonacci(ni) {
	if (n < 2) {
	return n
	}
	
	return nthFibonacci(n - 1) + nthFibonacci(n - 2)
}
```

