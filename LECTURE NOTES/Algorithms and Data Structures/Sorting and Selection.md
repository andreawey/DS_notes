# Sorting

>[!DANGER] Sorting formal definition
>given a sequence $u_1, ... u_n$ of n elements from a totally ordered universe U, compute an ordered **permutation** $v_1, ..., v_n$ of the input sequence

>[!WARNING] Stability
>A sorting algorithm is stable if in case of ties, preserves in the output the original ordering of the elements in the output.
## Insertion Sort $O(n²)$

>[!CODE] Insertion Sort
>```
>insertionSort(array L, size n)
>	for(j=2, ..., n)
>		l = L[j]
>		i = j-1
>		while(i>0 and L[i]>l)
>			L[i+1] = L[i]
>			i = i-1
>		L[i+1] = l
>```

![[Insertion Sort.png]]

## Selection Sort $O(n²)$

>[!CODE] Selection Sort
>```
>selectionSort(array L, size n)
>	for (j=1, ... n-1)
>		m=j
>		for (i = j+1, ...,n)
>			if(L[i]<L[m])
>				m = i
>		swap L[j] and L[m]```

![[selection sort.png]]


## Bubble sort $O(n²)$

>[!CODE] Bubble sort
>```
>bubbleSort(array L, size n)
>	sw = True
>	while(sw)
>		sw = False
>		for (i=1, ... n-1)
>			if(L[i] > L[i+1])
>				swap L[i] and L[i+1]
>				sw = true
>``` 

![[bubble sort.png]]


## Merge Sort $O(\textit{n log n})$

Works with devide an impera approach

1. In the divide step the input list L is partitioned arbitrarily into two halves A and B
2. A and B are sorted recursively
3. In the merge step A and B are merged together to get the output L by iteratively extracting the minimum between A and B which is obtained by comparing the first (remaining) elements in A and B

>[!CODE] Merge Sort
>```
>mergeSort(list L)
>	if(L containes at least 2 elements)
>		A = first half of L
>		B = second half of L
>		L = empty list
>		mergeSort(A)
>		mergeSort(B)
>		while(A and B are both not empty)
>			let c be the minimum of the first elements of A and B and let $C\in${A, B} be the corresponding list extract c from C
>			add c at the end of L
>		Let C $\in${A, B} be the non empty list among A and B
>		Append C at the end of L
>```


![[MergeSort.png]]

## QuickSort $O(\textit{n log n})$

Works with randomization in the divide step of the divide and impera approach

1. One pivot $p\in L$ is selected and the input L is subdivided into the elements A smaller than p
2. A and B are sorted recursively
3. The output is the concatenation A p B

if p is chosen deterministically quickSort takes $O(n²)$ worst case time

>[!CODE] QuickSort
>```
>quickSort(list L)
>	if(L contains at least 2 elements)
>		p = elements of L chosen uniformly at random
>		A = elements \geq p in L_p
>		B = elements > p in L_p
>		quicksort(A)
>		quicksort(B)
>		L = A p B
>```

![[Quicksort.png]]

### Quicksort In place

![[quicksort in place.png]]

## HeapSort $O(\textit{n log n})$

Works with efficient data structure, based on iteratively extracting the minimum or maximum from the remaining elements
Uses a heap unlike selectionSort

place the elements in a heap, and then iteratively extract the maximum

>[!CODE] heapSort(list L)
>```
>heapSort(list L)
>	place the elements of L in a disordered heap H
>	L = empty list
>	heapify(H)
>	while(H is not empty)
>		place root(H) at the beginning of L 
>		deleteMax(H)
>```

![[heapsort 1.png]]![[heapsort 2.png]]

# Lower Bounds on Sorting

>[!DANGER] Lower Bound
>a lower bound (LB) on a computational problem is a lower limit on the amount of a given resource that every algorithm needs to solve a given problem

# Comparison based sorting
>[!DANGER] Comparison based sorting
>a comparison-based sorting algorithm is an algorithm that can only compare values in a black-box manner

>[!DANGER] All comparison based sorting algorithms require $\Omega$ (n log n) time


# Decision Tree

>[!DANGER] Decision Tree
>a decision tree is a binary tree that represents the choices made by a deterministic comparison based sorting algorithm on an imput of a given length n
>- Each node represents a comparison between two input values 
>- its children correspond to the outcomes of this comparison
>- the leaves indicate the output permutation of the input


![[Decision Tree.png]]


A binary tree with k leaves has height at least $log_2 k$



# Linear Sorting Algorithms

## Bucket Sort $O(n+k)$ 
When k is linear $k = O(n)$

>[!CODE] BucketSort
>```
>bucketSort(list L (with pointer to the last element), length n)
>	create an array B of k empty lists (with pointer to last)
>	for(i=1, ... n)
>		add i-th element L[i] of L at the end of B[L[i]]
>	L = empty List
>	for j=1, ..., k
>		add B[j] at the end of L
>```

![[BucketSort.png]]


## Radix Sort $O(n\frac{log_k}{log_n})$
when k is polynomial in n i.e. $k=n^{O(1)}$

>[!CODE] RadixSort
>```
>radixSort(list L, length n, base b)
>	represent each number in base b
>	for(i = 1, ... [log_bk])
>		L = L sorted with (stable) bucketSort using as key the i-th digit
>```

![[RadixSort.png]]
>[!INFO] RadixSort sorts in $O((n+b)log_bk)$ time


# Search in a Sorted Sequence
problem Given a sorted sequence S of n integers and an integer key k, determine whether k belongs to S or not

>[!CODE] binary search
>binarySearch(array S of length n, $k\in \mathbb{N}$) -> Boolean
>1. if (n=0) return False
>2. if ($S[n/2] = k$) return true
>3. if ($S[n/2] > k$) 
>	return binarySearch(S$[1, [n/2]-1], k$)
>else
>	return binarySearch(S$[[N/2]+1,n], k$)

>[!INFO] The binarySearch takes $O(log n)$ time

# Selection
Extract from a large amount of data a small set of values that represent the main statistical features of the data

EG: 
- Minimum and Maximum
- average
- mode = most frequent element
- median = middle element after sorting


>[!DANGER] Selection problem definition
>given a set of n elements from a totally ordered universe, and an integer $k\in[1,n]$ find the k-th smallest element

the median is obtained by solving selection with $k=[n/2]$


## QuickSelect

>[!CODE] QuickSelect
>quickselect (list L , length n, position k)
>1. choose a random pivot $p\in L$
>2. compute $L_1 = {x\in L: x<p}, L_2={x\in L: x=p}, L_3={x\in L: x>p}$
>3. if $(k\leq \vert L_1\vert)$ then return quickSelect($L_1, \vert L_1 \vert, k$)
>4. if $(k>\vert L_1\vert + \vert L_2\vert)$ then return quickSelect$(L_3, \vert L_3\vert , k-\vert L_1\vert - \vert L_2\vert)$
>5. return p

>[!INFO] Quick Select solves selection in $O(n)$ expected and $O(n²)$ worst-case time


## Deterministic QuickSelect

solves selection in O(n) worst case time
similar to quickselect only pivot selection changes


