## 動態規劃 Dynamic Programming

Dynamic Programming (DP) 是一種優化演算法的方式。可以利用 DP 優化的演算法多半解決的問題是求極值 (例外，費氏數列)，而求極值的方法就是窮舉，窮舉出所有可能找到最大或最小值。

求極值靠窮舉，**窮舉的邏輯是樹狀結構，窮舉的過程是 DFS on Tree**，可以透過 Recursion 與 Backtracking 來實現。若窮舉的樹狀邏輯具有

* Overlapping subproblems
* Solutions to smaller problems can be used to solve larger problems incrementally

這兩點特性時，可以透過 DP 來優化。使用 DP 優化窮舉過程的思考流程：

1. 找到狀態和選擇
2. 定義 dp 陣列的意義
3. 列出狀態轉移方程式 (描述狀態之間的關係)

#### 如何列出狀態轉移方程式

1. 找到 Base Case
2. 找到狀態
3. 找到選擇
4. 定義 dp 陣列的意義
