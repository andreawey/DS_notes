
AVL trees allow one to implement dictionaries in O(log n) worst case time per operation, where n is the current number of elements in the dictionary. 


### IDEA
- map the universe U of keys into the position of an array T (hash table) of size m=O(n) via a hash function h:U->{0, .. , m-1}
- This way we can perform the dictionary operations in O(1) time and linear space
1. Insert (e,k) into T[h(K)] if it contains null
2. search k by looking into T[h(k)] and checking if there is (e,k)
3. delete k by setting to null T[h(k)] if it contains (e,k)


## Collisions

using the doubling technique guarantee that $n\leq m\leq 2n$ and use a hash function that maps U into {0, ... m-1} e.g. h(x) = x mod m

Problem:
there might be collisions i.e. two keys $k \neq k'$ in the dictionary with h(k) = h(k' ) This could happen already for n=2

how to handle

1. Open addressing
2. cuckoo hashing
3. collision lists



# Open Addressing
open addressing the hash function h(k,1) has 2 arguments where i is an index that is used to explroe all positions in T in some order
1. search(k) check h(k,0), h(k,1), .... until you find k or null...
2. insert(e,k) check h(k,0) h(k,1), .... until you find null or canc ...
3. delete(k) check h(k,0) h(k,1), ... until you find k or null deleted entries have to be marked wiht canc

it is critical that 
1. m>n at any time so that there is always a free entry for insert
2. h(k,0) ... h(k,m-1) is a permutation of {0, ..., m-1} so that we explore all table entries exactly once



# Cuckoo Hashing
uses 2 hash functions $h_1(k)$ and $h_2(k)$
1. search(k) check T$[h_1(k)]$ and $T[h_2(k)]$ 
2. delete(k) check $T[h_1(k)]$ and $T[h_2(k)]$
3. insert(e,k) if $T[h_1(k)]$ and $T[h_2(k)]$ are both occupied temporarily remove (e', k') in $T[h_1(k)]$ to free one slot for (e,k) and then insert (e', k') with the same approach 

# Collision lists
each entry of T is a list of entries whose keys are hashed to the same index
1. insert(k) scan $T[h(k)]$ until you find k in which case you return the corresponding entry, or you reach the end of the list
2. delete(k) scan $T[h(k)]$ until you find k, in which case you remove the corresponding entry, or you reach the end of the list
3. insert(e,k) scan $T[h(k)]$ until you find k or you reach the end o fthe list, in the latter case add (e,k) to $T[h(k)]$

>[!CODE] Hash Tables with collision lists
>**data**
>m-size array T of lists, initialized with empty lists
>**search(k)**
>	v = first$T[h(k)]$
>	while v not null
>		if key(v) = k: return v
>		v = next(v)
>	return null
>**insert(v)**
>	if search(key(v)) = null add v to $T[h(key(v))]$
>**delete(k)**
>	v = search(key(v))
>	if v not null: remove v from $T[h(key(v))]$

![[hash tables with collision lists.png]]

Each operation takes O(L) time in the worst case where L is the lenght of the longest list


# Universal Hashing

>[!DANGER] Universal hashing
>a randomized algorithm that generates a hash function h is universal if for any 2 distinct x, y in U the probability of a collision is $$P(h(x) = h(y)) \leq 1/m$$
>The corresponding set of hash functions is a universal hash family

Consider a hash table where the has function is sampled from a universal hash family. Assume that $m=\Theta(n)$ at any time. Then the expected time per operations is O(1)


# Perfect Hashing

use the same approach as before but with sufficiently large hash table

>[!CODE] Perfect Hash
>perfectHash(list L of n pairs (el,k), with k in U = {0, ..., 2^w -1})
>1. create a hash table T of size m=n² 
>2. while true
>	initialize t with null entries
>	sample a universal hash function : U ->{0, ... m-1}
>	perfect = true
>	for elements L[0], ... L[n-1] in L 
>		j= h(key(L[i]))
>		if (T[j]=null) T[j] = L[i]
>		else perfect = false
>	if perfect return (T,h)

perfectHash constructs a perfect hash table in O(n²) expected time and using O(n²) space

for success probability p the expected number of independent attempts till a success it 1/p

# 2 level hashing

the idea is to use a 2-level hash table
first T of size m=2n with an associated first level universal hash function h

>[!CODE] 2levelPerfectHash
>2levelPerfectHash(list L of n pairs (el, k) with k in U = {0, ... , 2^w -1})
>T = first level hash table of size $m=\Theta(n)$
>M = array of empty lists of size m
>sample a universal hash function h:U->{0, ..., m-1}
>for (i=0, ... n-1)
>	add L[i] to list M[h(i)]
>for (j=0, ... m-1)
>	T[j] = perfetctHash(M(j))

![[2 level hashing.png]]
constructs a perfect 2-level hash table in O(n) expected time and using O(n) expected space

