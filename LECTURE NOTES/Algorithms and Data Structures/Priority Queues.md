>[!DANGER] Data Type Priority Queue
>**data type** priority queue
>**data** a set S of pairs (elem, key) with key from a numerical data type (with addition)
>**operations:** 
>	findMin() -> elem
>		returns e of pair(e,k) in S with minimum k or null if S= empty subset
>	insert(elem e, key k)
>		adds to S the pair (e,k)
>	deleteMin()-> elem
>		deletes from S the pair (e,k) with minimum k and returns e
>	increaseKey(pair p, key d)
>		increase key(p) by d
>	decreaseKey(pair p, key d)
>		decrease key(p) by d
>	merge(priority queue Q1, priority queue Q2) -> priority queue
>		returns union of Q1 and Q2


# Implementations

## Binary Heaps
>[!DANGER] Binary Heap
>A binary heap is a binary tree H with the following extra properties:
>1. each node v is labelled with a key k(v) from a totally ordered set
>2. ordering for each node v and child w of v, $k(v)\leq k(w)$
>3. structure the tree is complete up to the second last level and leaves in the last level occupy the left most available positions

- The minimum is contained in the root
- the height of the tree is O(log n)

### Operations
moveUp(v) restores the ordering property if the heap becomes correct by property increasing the key of v

>[!CODE] moveUp(node v)
>while v different from root(H) and key(v) < key(parent(v))
>	switch content of v and parent(v)
>	v = parent(v)

move up restores the ordering property in O(log n) time

MoveDown(node v) restores the ordering property if the heap becomes correct by properly decreasing the key of v 

>[!CODE] moveDown(node v)
>while true
>	u = child of v with minimum key(u) or null if no child 
>	if u not null and key(u) < key(v)
>		switch content of v and u
>		v = u
>	else return

moveDown restores the ordering property in O(log n) time

>[!CODE] FindMin()
>return elem(root[H])

findMin takes O(1) time

>[!CODE] Insert (elem e, key k)
>create node v with elem(v) = e and key(v) = k
>insert v on the last level of H as left as possible
>moveUp(v)

>[!CODE] DeleteMin()
>if root(H) = null return null
>switch content of root(h) with e = elem(root(H)) with content of rightmost leaf in the last level of H and delete v
>movedown(root(H))
>return e

>[!CODE] decreaseKey(pair p, key d)
>v = node containing p
>key(v) = key(v) -d
>moveUp(v)

>[!CODE] IncreaseKey(pair p, key d)
>v = node containing p
>key(v) = key(v) +d
>moveDown(v)
>



## Binomial Heaps

>[!DANGER] Binomial Tree
>the h-th binomial tree B_h is defined recursively as follows:
>1. $B_0$ is a single node
>2. $B_h$ for $h\geq 1$ is obtained by appending the root of one $B_{h-1}$ as child of the root of another $B_{h-1}$

![[Binomial Tree.png]]
>[!DANGER] Properties
>$B_h$ has the following properties:
>1. number of nodes $\vert B_h\vert = n = 2^h$
>2. deg(root($B_h$)) = h
>3. height($B_h$) = h
>4. subtree structure the subrtrees of root ($B_h$) are $B_0, B_1, .... B_{h-1}$


>[!DANGER] Binomial Heap
>A binomial heap is a forest of binomial trees with the following properties: 
>1. Stored data, each node v stores a pair (elem(v), key(v)) with key(v) belonging to a numerical domain
>2. unicity, there is at most one B_h for each $h\geq 0$
>3. ordering, for each node v and child u of v, key(v) $\leq$ key(u)

In a binomial heap H over n nodes ther are at most log_2 n many trees of degree and height at most log_2 n


### Operations on binomial heaps

>[!CODE] restructure()
>for increasing index i
>	while(exists at least two B_i)
>		take any two Bi's and B' and B'' with key(root(B')) $\geq$ key(root(B''))
>		make root(B'') a child of root(B')
>		delete root(B'') from the list of roots

>[!CODE] MoveUp(node v)
>while v different root(H) and key(v) < key(parent(v))
>	switch content of v and parent(v)
>	v = parent(v)

>[!CODE] MoveDown(node v)
>while true
>	u = child of v with minimum key(u) or null if no child
>	if(u not null and key(u) < key(v))
>		switch content of v and u
>		v = u
>	else return

>[!CODE] FindMin()
>if(H is empty) return null
>v = root of Bi with minimum key(root(Bi))
>return elem(v)

>[!CODE] insert(elem e, key k)
>create node v with key(v) = k and elem(v) = e
>add to the heap one B0 consisting of v
>restructure()

>[!CODE] DeleteMin()
>if H is empty: return null
>let Bi be th tree with minimum key(root(Bi)) with e = elem(root(Bi))
>delete root(Bi) and add its subtrees to the heap
>restructure()
>return e

>[!CODE] decreaseKey(pair p key d)
>v = node containing p
>key(v) = key(v) -d
>moveUp(v)

>[!CODE] increaseKey(pair p, key d)
>v = node containing p
>key(v) = key(v) + d
>moveDown(v)

>[!CODE] merge(binomial heap H1, binomial heap H2)
>create a binomial heap H containing all trees of H1 and H2
>apply restructure() to H
>return H

