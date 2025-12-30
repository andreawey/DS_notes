# Data Type

>[!DANGER] Dictionary
>**data type** Dictionary:
>**data**: a set S of pairs (elem, key) with key from a totally ordered domain
>**operations**:
>- insert(elem e, key k)
> 	 adds to S the pair(e,k)
> - delete(key k)
> 	deletes from S a pair (e,k) if any
>- search(key k) -> elem
>	returns element e for some (e,k) in S if any


# Search Trees

>[!DANGER] Binary Search Tree (BST)
>is a binary tree with the following extra properties:
>1. Each node v is associated with a key k(v) from a totally ordered set
>2. searching if u is a node in the leaf (resp., right) subtree of v, then k(u)<k(v) (respectivelly, k(u)> k(v))


## Operations on BST

>[!CODE] Search
>```
>search(BST tree T, key k)
>v = root(T)
>while(v not null)
>	if(k= key(v)) return v
>	if(k<key(v)) v is left child of v
>	else v is right child of v
>return null
>```

>[!CODE] Insert
>```
>insert(BST T, node u)
>	v = root(T), w = null, k= key(u)
>	while(v not null)
>		if(k=key(v)) exit while
>		if(k < key(v)) w = , v left child of v
>		else w=v, v right child of v
>	if(v not null) return // means that we found k
>	if(w = null), T={u} // the tree was empty
>	else if (key(w)>k) add u as left child of w
>		else add us as right child of w
>```


>[!CODE] Max
>```
>max(BST T, node u in T)
>	v = u
>	while(right child of v not null)
>		v= right child of v
>	return v
>```


>[!DANGER] Predecessor
>The predecessor of a node $u\in T$ is the node v with the largest key such that key(v)<key(u)

The predecessor of u is the maximum in the left subtree of u or the ancestor v for which u is minimum in its right subtree

![[predecessor in bst.png]]
>[!CODE] predecessor
>```
>pred(BST T, node u in T)
>	if(left child u not null) return max(T, left child of u)
>	v = u
>	while parent(v) not null and v is left child of parent(v)
>		v= parent(v)
>	return parent(v)
>```

### Delete a node

To delete a node with key k, we first search for such a node u. if we find it we distinguish 3 cases:
1. u is a leaf, simply delete it
2. if u has exactly 1 child w, connect w to parent(u) (or make w the new root if u was the root)
3. if u has 2 children, switch u with its predecessor v, and then delete v as in previous 2 cases
![[delete node 1.png]]![[delete node 2.png]]

>[!CODE] Delete
>```
>delete(BST T, node u in T)
>	if u has no child delete u
>	else
>		if u has 2 child delete1(T, u)
>		else
>			w= pred(T, u) // w has at most 1 child
>			switch the content of w and u
>			if w has no child delete w
>			else delete1(T, w)
>			
>delete1(BST T, node u in T wich one child)
>	v = unique child of u
>	parent(v) = parent(u)
>	if u=root(T): root(T) = v
>	if u = left child of parent(u): left child of parent(u) = v
>	else: right child of parent(u) = v
>```


# AVL Trees

>[!FAQ] All operations take O(h) time, where h is the current height of T

Balance factor:
bf(v) = balance factor of node v = height of left subtree - height of right subtree

A tree is balanced in height if bf(v) is {-1, 0, 1} for every node v
>[!DANGER] An AVL tree is a search tree balanced in height

an AVL tree with n elements has height O(log n)

## balancing via rotations

![[avl balancing via rotations.png]]

>[!CODE] Insert
>insert (AVL tree T, node u)
>1. insert us as in a BST
>2. v = lowest node on path u-root(T) with absolute balance factor 2
>3. execute the corresponding rotation on v

insert takes O(log n) time and the AVl tree remains balanced

>[!CODE] Delete
>delete(AVL tree T, key k)
>u = search(T,k)
>if u= null return
>w = parent(u)
>delete u as in a BST
>for each node v on the path P form w to root(T)
>	if v is unbalanced execute the corresponding rotation on v

delete takes O(log n) time and the AVL tree remains balanced


