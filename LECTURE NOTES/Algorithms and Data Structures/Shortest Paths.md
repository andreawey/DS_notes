we focus on directed graphs

>[!DANGER] Path
>a $v_1 - v_k$ path in a graph G=(V, E) is a sequence of nodes $P=(v_1, ... v_k)$ such that $v_iv_{i+1} \in E$ for $i=1, ... k-1$. The path is simple if no node is repeated. If $v_1 = v_k$ the path is a cycle

>[!DANGER] shortest path
>given a path $P=(v_1, ... , v_k)$ in a weighted graph G(V, E, w) its length is $w(P) = \sum_iw(v_iv_{i+1})$. P is shortest if there is no path from $v_1$ to $t_k$ with strictly smaller length.

>[!DANGER] Distance
>The distance $d_{ab}$ from node a to node b is the length of a shortest a-b path (or +$\infty$ if there is no such path)

>[!DANGER] Simplicity of shortest paths
>for any pair of nodes a,b, there exists **always a simple** shortest a-b path ( in case no negative cycles) If weights are strictly positive all shortest a-b paths are simple

>[!DANGER] Optimality of subpaths
>if $P=(v_1, ..., v_k)$ is a shortest path, then any subpath $Q=(v_i, ..., v_j)$ for j>i is also a shortest path



# Single Source Shortest Paths (SSSP)

>[!DANGER] SSSP definition
>The single source shortest path problem is to compute all the distances from a given source node s

>[!DANGER] shortest path tree
>there exists a set of shortest paths from s to all other nodes whose support is a tree ( the shortest path tree )



# Algorithms

## Bellman and Ford's algorithm 

>[!CODE] BellmanFord(weighted graph G=(V, E, w), node s)
>	$D_SS$ = 0 
>	$D_SV$ = $\infty$ for all v in V\\{s}
>	e(v) = null for all v in V
>	for (n-1) times
>		for all uv in E, v not in s
>			if D_sv > D_su + w(uv)
>				D_sv = D_su + w(uv) 
>				e(v) = uv
>	return (D, e())


![[Bellman Ford ex.png]]
![[Bellman Ford table.png]]

>[!DANGER] Bellman and Ford's algorithm solves SSSP in O(mn) time



## Dijkstra's algorithm

used to solve SSSP faster in case of non-negative weights

similar to prim's algorithm
1. at the beginning of each iteration we are given a tree T
2. The goal of the iteration is to add a new node v and edge e(v) to T
3. nodes are stored in a priority queue used to select v

difference from Prim's 
1. T is a partial shortest path tree rooted at s
2. the selection of v is based on the current estimated distance from s which is key in the priority queue

>[!CODE] Dijkstra(weighted graph G=(V, E, w), node s)
>	set d(s) = 0 and d(v) = nW for all v in V\\{s}
>	set e(v) = null for all v in V
>	PQ = Priority Queue
>	for all v in V:
>		PQ.insert(v, d(v))
>	T = ({s}, [])
>	while PQ.findMin() not null:
>		u = PQ.deleteMin()
>		T =V(T).append(u), E(T).append(e(u))
>		for all e=uv in E, v not in V(T)
>			if d(v) > d(u) + w(e)
>				PQ.decreaseKey(v, d(v)-d(u)-w(e))
>				d(v) = d(u) + w(e)
>				e(v) = e
>	return T

![[dxtra.png]]![[dxtra 2.png]]


# SSP and DAGs

>[!DANGER] DAG
>A directed Acyclic Graph is a directed graph without (directed) cycles

>[!DANGER] Topological order
>of a graph is an ordering of its nodes such that, for each edge ab, a appears before b in the ordering

>[!CODE] Topological order(DAG)
>	dag = dag.copy()
>	order = []
>	zero_in_degree = []
>	for vertices in DAG:
>		if in-degree(vertex) = 0
>		zero_in_degree.append(vertex)
>	while Zero_in_degree not empty
>		extract v from Zero_in_degree and append to order
>		for all vertex and edges of dag
>			if in degree = 1: append to zero_in_degree
>		delete vertex from dag
>	return order


>[!CODE] SSSP on DAG (DAG, node)
>	for all vertex in DAG:
>		distance(node) = 0
>		distance(vertex) = infinity
>		...
>		



# All pairs shortest paths

>[!DANGER] problem def
>the all pairs shortest path problem APSP is to compute the distance s between all pairs of nodes


## Floyd and Warshall's algorithm O(n³)

>[!CODE] FloydWarshall (weighted graph G=(V,E, w))
>	distance(source, target) = weight(source-target) if it exists, otherwise infinity
>	edge(source, target) = source-target for all existing edges, otherwise null
>	
>	for k=1, ... n
>		for all source, target pairs in vertex
>			if distance(s,t) > distance(s, v_k) + distance (v_k, t) 
>				distance (s,t) = distance(s, v_k) + distance (v_k, t) 
>				edge(s,t) = edge(v_k, t)
>	return (distance, edges)

![[fw1.png]]
![[fw2.png]]
![[fw3.png]]
![[fw4.png]]
![[fw5.png]]
![[fw6.png]]
>[!DANGER] FW algo solves APSP in O(n³) time




## Johnson's algorithm O(mn+n² log n)

in case of non negative weights 

- add a dummy node q with edges qv of weight 0 for all v
- compute distances d(v) from source q using Bellman and Ford's algorithm
- Modify edge weights to **w'(uv) = w(uv) +d(u)-d(v)**
- then run dijkstra from every source s wrt weights w'

![[johnson's.png]]
>[!CODE] Johnson(weighted graph G)
>	G' = graph obtained from G by adding a dummy node q plus edges qv for all v in V of weight 0
>	d(), e() = BellmanFord(G', q)
>	set w'(vu) = w(vu) + d(v) - d(u) for all vu in E
>	
>	for all s in V
>		T(s) = dikstra((V, E w'), s)
>	return T()



