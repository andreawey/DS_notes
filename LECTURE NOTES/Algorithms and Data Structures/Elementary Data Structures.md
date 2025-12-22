>[!DANGER] Data Type
>A data type consists of two main ingredients:
>- A collection of variables (or data) defined over proper domains, whose value can change over time
>- A collection of operations that applied to the variables return values and/or change the values of the variables

>[!DANGER] Data Structure
>A data structure is an implementation of a data type on a given machine



# Stack

>[!DANGER] Stack
>```
>data type Stack:
>data: 
>	sequence S of elements
>operations:
>	isEmpty()-> boolean
>		returns true if S is empty, false otherwise
>	push(elem e)
>		adds e as last element of S
>	pop() -> elem
>		removes from S the last element, and returns it
>	top() -> elem
>		returns the last element in S (without removing it from S)
>```


# Queue
 >[!DANGER] Queue
 >```
 >data type queue:
 >data:
 >	sequence S of elements
 >operations:
 >	isEmpty() -> boolean
 >		returns true if S is empty, false otherwise
 >	enqueue(elem e)
 >		adds e as last element of S
 >	dequeue() -> elem
 >		removes from S the first element, and returns it
 >	first() -> elem
 >		returns the first element in S (without removing it from S)
 >```
 >	
 
# Data Structures for Lists

- Indexed representations: data stored into the entries of **arrays**
- Linked representations: data stored into **records**, which are "connected" using **pointers**


## Indexed representations and Linked representations

![[Indexed and linked representations.png]]

Indexed representations access the i-th element in O(1) time, but take O(n) time to add a new element
Linked representations take 0(1) time to add a new element and access to the i-th element in O(i) time.

To mitigate indexed representation cons use **doubling technique**
>[!INFO] Doubling Technique
>- When k=n move the list to a new array of size 2n
>- When k=n/4 move the list to a new array of size n/2


# Stacks
## Indexed representations 
- Each operation can be performed in O(1) by operating on the last element of the array
- This unless we need to change the size of the array, which costs O(k) with the doubling technique

## Linked representations
- Each operation can be trivially performed in O(1) using the pointer to the first record


# Implementation of queues
## Indexed representations
- The current list is contained into an interval of entries in the array, interpreted in a circular sense
- This way all the operations can be performed in O(1) time
- Also this case changing the size of the array costs O(k)

 ![[indexed representation of queues.png]]

## Linked representations
- we need also a pointer to the last element of the list, this way we can perform enqueue() in O(1) time
- first() and dequeue() can be implemented in O(1) time using the pointer to the first record
![[Linked representation of queues.png]]

# Trees

>[!DANGER] Trees formal definition
>A tree is defined recursively as follows:
>- a single node v (root) is a tree
>- a node v (root) plus a collection of subtrees is a tree, where each subtree is a tree


![[tree structure.png]]


## Operations on trees
- Add a child to a node
- delete a node (and all its subtree)
- search for a node with a given label
- merge two trees $T_1$ and $T_2$ by making the root of $T_2$ a child of $T_1$ 
- Switch the labels of two nodes
- Move an entire subtree to a different position in the tree
- ....


## Linked representation of trees

![[Linked representations of trees.png]]


## Indexed representations of trees
Positional vector:
- This works only for k-ary trees (bounded by number of children)
- nodes are stored into an array. The j-th child of a node in position i is in position ki+j-1. the root is in position 1

![[indexed representation of trees.png]]

>[!HINT] Unbalanced trees
>If the tree is unbalanced, there is a huge space waste.


# Graphs

## Directed Graphs

>[!DANGER] Directed graphs formal definition
>a directed graph G is a pair (V,E) where:
>- V is a set of n (nodes or vertices)
>- E is a collection of m ordered pairs (u,v) of node (edges or arcs)

>[!HINT] Rooted trees are directed graphs


## Undirected graphs

>[!DANGER] Undirected graphs formal definition
>an undirected graph G is a pair (V, E) where:
>- V is a set of n (nodes)
>- E is a collection of m unordered pairs {u,v} of node (edges)

>[!HINT] Unrooted graphs are undirected connected graphs without cycles
>

>[!HINT] from undirected to directed
>undirected graphs can be represented via directed ones by replacing each undirected edge {u, v} with two oppositely directed edges (u,v) and (v,u)

Often weights are associated with edges, and one can define lengths (hence distances) as sum of edge weights along paths.

![[Graphs properties.png]]

## Operations on graphs
- determine if there is an edge from u to v
- return the list of neighbors of a node
- add a node/edge
- delete a node/edge
- ...

## Indexed representations of graphs
Adjacency matrix

![[adjacency matrix.png]]

| Operation          | Time    |
| ------------------ | ------- |
| Adjacent (u,v)     | O(1)    |
| Neightbors(u)      | O(n)    |
| add/delete a node  | $O(n²)$ |
| add/delete an edge | O(1)    |

>[!HINT] The adjacency matrix of an undirected graph is symmetric

>[!HINT] The space usage is $O(n²)$ which is often not acceptable


# Heaps

>[!DANGER] Heap formal definition
>a binary heap is a binary tree with the following extra properties
>1. Each node v is labelled with a key **k(v)** from a totally ordered set
>2. (ordering) for each node v and child w of v, **k(v) $\geq$ k(w)**
>3. (structure) the tree is complete up tot the second last level and leaves in the last level occupy the left-most available positions

>[!HELP] Properties
>1. a maximum is contained in the root
>2. the height of the tree is O(log n)

![[HEAP.png]]

## Procedures
### FixHeap
Suppose that H is a heap except for the root that violates the ordering property, then fixHeap(r) enforces the correct ordering

>[!CODE] FixHeap()
>```
>fixHeap(node v, heap H)
>	if(v is not a leaf)
>		let u be the child of v with largest key
>		if(k(v) < k(u))
>			switch the key (and content) of nodes u and v
>			fixHeap(u, H)
>```

>[!NOTE] fixHeap takes O(log n) time

### DeleteMax
We can use fixHeap() to delete the maximum from a heap 

>[!CODE] deleteMax
>```
>deleteMax(heap H)
>	v = rightmost leaf in the last level of H
>	switch the key (and content) of v and root(H)
>	delete v
>	fixHeap(root(H), H)
>```

>[!NOTE] deleteMax also takes O(log n) time


### Heapify
it is a linear time procedure that given a disordered heap enforces the ordering property

>[!CODE] Heapify
>```
>heapify(heap H)
>	if(H contains at least 2 elements)
>		heapify(left subtree of H)
>		heapify(right subtree of H)
>		fixHeap(root(H), H)
>```

>[!NOTE] heapify takes O(n) time




