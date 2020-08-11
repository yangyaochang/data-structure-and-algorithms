## 排序 Sorting

### 定義

排序是將一組資料根據鍵值大小作遞增或遞減排列。

解題時常見的 Sorting 演算法基本上分兩種

* Quadratic sorting - Time Complexity 為 O(n ^ 2)
	* Bubble Sort
	* Selection Sort
	* Insertion Sort
* Quasilinear sorting - Time Complexity 為 O(nlogn)
	* Quick Sort
	* Merge Sort
	* Heap Sort

### Quasilinear Sorting

Merge Sort 是 Stable Sort，Quick Sort 是 Unstable Sort

> Stable Sort & Unstable Sort       
> A sorting algorithm is said to be stable if two objects with equal values appear in the same order in sorted output as they appear in the input array to be sorted. 保持本來的順序

#### Quick Sort

##### 邏輯

* 選擇一個 Pivot Value
* 將資料分為比 Pivot Value 大與比 Pivot Value 小兩組，就可以確定 Pivot Value 排序後的位置
* 對兩組資料重複上述步驟直到資料大小只剩下一筆資料

```    
const quicksort = (arr) => {
	const divide = (start, end) => {
		if (start >= end) {return}
		// > handles the case where pivot value is the biggest or the smallest => mid = start or mid = end
	
		let mid = start
	
		for (let i = start; i < end; i++) {
			if (arr[i] < arr[end]) {
				[arr[i], arr[mid]] = [arr[mid], arr[i]]
				mid++
			}
		}
		[arr[mid], arr[end]] = [arr[end], arr[mid]]
		
		divide(start, mid -1)
		divide(mid + 1, end)
	}
		
	divide(0, arr.length - 1)
	return arr
}
```

#### Merge Sort

##### 邏輯

* 將資料不斷對分直到每一筆資料只剩下一個 Element，此時為 Sorted Array
* 再排序合併成一個 Sorted Array

```
const merge = (arr1, arr2) => {
	let p1 = 0
	let p2 = 0
	let result = []
		
	if (arr1 === undefined) {return arr2}
	if (arr2 === undefined) {return arr1}
		
	while (arr1[p1] && arr2[p2]) {
		if (arr1[p1] <= arr2[p2]) {
			// equal makes merge sort stable
			result.push(arr1[p1])
			p1++
		} else {
			result.push(arr2[p2])
			p2++
		}
	}
		
	if (p1 === arr1.length) {
		result = result.concat(arr2.slice(p2))
	} else if (p2 === arr2.length) {
		result = result.concat(arr1.slice(p1))
	}
		
	return result
}
	
	
function mergesort(input) {
	if (input.length <= 1) {return input} 
		
	let midIndex = Math.floor(input.length / 2)
	let left = input.slice(0, midIndex)
	let right = input.slice(midIndex, input.length)
		
	return merge(mergesort(left), mergesort(right))
}
```

#### Heap Sort

##### Binary Heap

##### 邏輯

* 選擇一個 Pivot Value
* 將資料分為比 Pivot Value 大與比 Pivot Value 小兩組，就可以確定 Pivot Value 排序後的位置
* 對兩組資料重複上述步驟直到資料大小只剩下一筆資料
