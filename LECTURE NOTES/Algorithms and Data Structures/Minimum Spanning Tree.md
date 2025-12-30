>[!DANGER] Spanning tree
>given an undirected garph G=(V, E) a spanning tree is a subtree T of G that contains all nodes V

>[!DANGER] minimum spanning tree
>given an undirected graph G=(V, E) with positive edge weights, the minimum spanning tree problem (MST) is to compute a spanning tree T of G of minimum weight $w(T)=\sum_{e\in E(T)}w(e)$

![[MST.png]]

a MST is the cheapest subgraph that connects all nodes

# Cut Rule
>[!DANGER] cut rule
>a cut of an undirected graph G=(V,E) is a non-empty proper subset of nodes
>the edges of a cut W are the ones with exactly 1 endpoint in W

>[!DANGER] Cut rule
>Let T be an MST containing all edges in B and no edge in R. Let also W be a cut containing no edge in B and let $e \in \beta (W)$ be an uncolored edge of minimum cost. Then there exists an MST T' that contains all edges in $B u {e}$ and no edge in R

# Cycle Rule

>[!DANGER] Cycle Rule
>let T be an MST containing all edges in B and no edge in R. Let also C be a cycle containing no edge in R and let $e\in$ E(C) be an uncolored edge of maximum cost. then there exists an MST T' that contains all edges in B and no edge in Ru{e}


# Algorithms

## Kruskal's algorithm
maintain a forest induced by blue edges
uncolored edges are considered in **non-decreasing order of weight**
There are 2 cases:
1. Both endpoints of e belong to the same tree of the forest. In this case color e red ( do not include in the MST)
2. the endpoints of e belong to different trees in the forest. In this case color e blue (include in the MST)

At the end of the process all edges are colored and blue ones define an MST that is given in output

![[kruskal 1.png]]
![[kruskal 2.png]]

use Union Find to store the nodes belonging to different trees in the forest. In particular, when you add an edge between 2 trees, use union to merge the into one tree

>[!CODE] Kruskal(weighted graph G=(V, E, w))
>	1. sort the edges in non decreasing order of weight e1, ... em
>	2. UF = union find data structure with one set per node
>	3. F = []  # will contain the blue edges
>	4. for e_i = {a_i, b_i} i=1, ... m
>		x=UF.find(a_i) 
>		y=UF.find(b_i)
>		if x different from y    # otherwise it's red
>			UF.union(x,y)
>			F = F union e_i
>			if  ( |F| = n-1) return T= (V,F)

>[!DANGER] Kruskal's algorithm can be implemented in O(m log n) time


## Prim's algorithm

in Prim's algorithm blue edges induce a single tree T ( initially containing an arbitrary node s)
at each step we color blue the cheapest edge with exactly one endpoint in V(T)

![[prim 1.png]]![[prim 2.png]]

using a priority queue to store the nodes not in V(T) The key for node v is the minimum weight of an edge between v and V(T)

>[!CODE] Prim(weighted graph G=(V,E, w), node s)
>	set d(s) = 0
>	set d(v) = W + 1 for all v in V\\{s}
>	set e(v) = null for all v in V
>	PQ = priority queue
>	T = ([],[])
>	while(PQ.findMin() not null)
>		u = PQ.deleteMin()
>		T.add(u, e(u))
>		for e={u,v} in E, v not in V(T):
>			if d(v) > w(e):
>				PQ.decreaseKey(v, d(v)-w(e))
>				d(v) = w(e)
>				e(v) = e
>	return T

>[!DANGER] Prim's algorithm can be implemented in O(m+n log n) time