>[!DANGER] Data Type
>**data type** : union find
>**data**: a collection C of disjoint subsets S of a given universe U of n elements, say U = {1, ... n} where each set S has a representative r(s) in S
>**operations**: 
>union(A,B) merges sets with representative A and B into one set with representative A
>makeSet(x) creates a new set S= {x} with r(S) = x
>find(x) returns r(S) for S containing e

![[unionfind.png]]


# Implementations
## QuickFind
each subset S is stored as a tree of height 1 where
1. the leaves contain the elements of S
2. the root contains r(S) 

>[!CODE] makeSet(elem x)
>create a tree with one root v and one child w and set elem(v) = elem(w) = x

>[!CODE] find(elem x) 
>v = parent of the node w containing x
>return elem(v)

>[!CODE] union(elem A, elem B)
>a = node containing A 
>b = node containing B
>make all children of b point to a and delete b

makeset and find -> O(1)
union -> O(n)

![[Quickfind.png]]
### Balance Heuristic in QF
union operations are expensive, change the pointers to the smallest set
![[QF balance heuristic.png]]

>[!CODE] makeSet(elem x)
>create a tree with one root v and once child w, and set size(v) = 1

>[!CODE] Union(element A and B)
>a = root node containing A 
>b = root node containing B
>if size(a) $\geq$ size(b)
>	size(a) = size(a) + size(b)
>	make all children of b point to a and delete b
>else
>	size(b) = size(a) + size(b)
>	make all children of a point to b and delete a
>	elem(b) = A (update the rot value)

in QF with union by size any sequence of m find() n makeSet() and n-1 union() takes **O(m + log n** time in total

## QuickUnion


Each subset S is stored as a tree where:
1. the nodes contain the elements of S
2. the root contains r(S)

>[!CODE] makeSet(elem x)
>	create a node v with elem(v) = x

>[!CODE] Find(elem x)
>	v = node containing x
>	while(parent(v) not null)
>		v = parent(v)
>	return elem(v)

>[!CODE] Union(elem A, elem B)
>	a = node containing A 
>	b = node containing B
>	make b child of a


makeset and union take O(1) and find O(n)

![[Quickunion.png]]
It is possible to reach height n-1

![[QU height.png]]
### Balance Heuristics in Quick Union


find ops are very expensive, during union append the shorter tree to the taller one (union by rank)

-> store with each root node the height (rank(v)) of the tree 
during union make the root with the tree with smaller rank point to the root with of the other tree

>[!CODE] makeSet(elem x)
>	node v with elem(v) = x and rank(v) = 0

>[!CODE] Union(elem A , elem B)
>	a = node containing A
>	b = node containing B
>	if rank(a) > rank(b)
>		make b child of a
>	else
>		if rank(a)<rank(b)
>			make a child of b
>			switch the elements of a and b
>		else (rank a = rank b)
>			make b child of a
>			rank(a) = rank(a) + 1

![[QU balance heuristic.png]]![[union find summary.png]]


### Compression Heuristic

while executing a find() change the pointers of some visited nodes to reduce their distance to the root

let $u_0, u_1, ... u_t$ be the nodes visited during find(x) where $u_0$ is the node containing x

1. Path compression: make $u_i$ a child of $u_t$ for all $i\leq t-2$
2. Path splitting: make $u_i$ a child of $u_i+2$ for all $i\leq t-2$
3. Path halving: make $u_2i$ a child of $u_{2i+2}$ for all $i\leq [t/2] -1$


![[compression Heuristics.png]]
