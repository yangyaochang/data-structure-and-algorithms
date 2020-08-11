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

#### Binary Heap

在理解 Heap Sort 之前先了解什麼是 Binary Heap。Binary Heap 是一個利用 Array 來表示 Complete Binary Tree 的資料結構，有兩項基本特徵：

1. Binary Heap 之結構可以視作 Complete Binary Tree     
所以每當要 Add 或 Remove 一個 Element 時必須發生在 Breadth-first-most Position (也就是 Array 的尾端) 以維持 Complete Binsry Tree 的結構。

2. Binary Heap 一共有兩類：
	1. Max Heap - Parent 的值比 Child 大，Root 會有最大值
	2. Min Heap - parent 的值比 Child 小，Root 會有最小值

Binary Heap 的概念雖然以二元樹來解釋，但實際上是以 Array 在儲存資料。利用二元樹陣列表示法的概念以 Array 來代表一個樹狀結構。為什麼使用 Array 來儲存資料，卻是用 Array 另外表示一個二元樹，而不是直接使用 Array 的方法呢？因為在 Array 上操作 Insertion, Deletion 是屬於 O(n) 的操作，透過二元樹可以降為 O(log n)，因為最多就是操作 log n 次。**常用來解決的問題是，想要取資料中的最大或最小值，但想節省 Time Complexity**。

當有 Add 或 Remove 發生時，有可能要重新調整二元樹的結構以符合 Max 或 Min Heap 定義。可以此概念推導出 Binary Heap 所需要的方法。

	class Minheap {
	    constructor() {
	        this.storage = []
	    }
	
	    swap(index1, index2) {
	        [this.storage[index1], this.storage[index2]] = [this.storage[index2], this.storage[index1]]
	    }
	
	    peak() {
	        return this.storage[0]
	    }
	
	    size() {
	        return this.storage.length
	    }
	
	    insert(value) {
	        this.storage.push(value)
	        this.bubbleUp(this.size() - 1)
	    }
	
	    getParent(childIndex) {
	        if (childIndex % 2 === 0) {
	            return (childIndex - 2) / 2
	        } else {
	            return (childIndex - 1) / 2
	        }
	    }
	
	    bubbleUp(childIndex) {
	        let parentIndex = this.getParent(childIndex)

	        while (childIndex > 0 && this.storage[childIndex] < this.storage[parentIndex]) {
	            this.swap(childIndex, parentIndex)
	            childIndex = parentIndex
	            parentIndex = this.getParent(childIndex)
	        }
	    }
	
	    removePeak() {
	        this.swap(0, this.size() - 1)
	        let result = this.storage.pop()
	        this.bubbleDown(0)
	        return result
	    }
	
	    getChild(parentIndex) {
	        let leftChild = 2 * parentIndex + 1
	        let rightChild = 2 * parentIndex + 2
	
	        if (leftChild >= this.size()) {
	            return leftChild
	        } else if (rightChild >= this.size()) {
	            return leftChild
	        } else if (this.storage[leftChild] < this.storage[rightChild]) {
	            return leftChild
	        } else {
	            return rightChild
	        }
	    }
	
	    bubbleDown(parentIndex) {
	        let childIndex = this.getChild(parentIndex)
	
	        while (childIndex < this.size() && this.storage[parentIndex] > this.storage[childIndex]) {
	            this.swap(childIndex, parentIndex)
	            parentIndex = childIndex
	            childIndex = this.getChild(parentIndex)
	        }
	    }
	
	    remove(value) {
	        let removeIndex
	        for (let i = 0; i < this.storage.length; i++) {
	            if (this.storage[i] === value) {
	                removeIndex = i
	            }
	        }
	
	        if (removeIndex === undefined) {
	            return
	        }
	
	        this.swap(removeIndex, this.size() - 1)
	        let result = this.storage.pop()
	        this.bubbleDown(removeIndex)
	        this.bubbleUp(removeIndex)
	        return result
	    }
	}

#### Heap Sort

##### 邏輯

* 選擇一個 Pivot Value
* 將資料分為比 Pivot Value 大與比 Pivot Value 小兩組，就可以確定 Pivot Value 排序後的位置
* 對兩組資料重複上述步驟直到資料大小只剩下一筆資料
