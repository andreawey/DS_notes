## Maximum Matching
>[!DANGER] Matching
>a matching of an undirected n- node m -edge graph G=(V, E) is a subset M of edges with pairwise disjoint endpoints. 
>A matching is perfect if all nodes are matched. 
>The maximum matching problem (MM) is to compute a matching of G of maximum cardinality
>endpoints of matching edges are matched and the other nodes are unmatched

![[matching.png]]

>[!DANGER] Paths
>given a graph G=(V, E) and a matching M of G, an alternating path is a path that alternates between edges of M and E\\M. An augmenting path is an alternating path whose endpoints are free

(alternating with dashed lines, augmenting unfinished with dashed lines)

>[!DANGER] Symmetric difference
>for two sets A and B their difference are the elements in either A or B (not both)

## Maximum Matching

>[!DANGER] augmenting path lemma
>given a graph G=(V,E) a matching M of G is maximum iff (G,M) does not admit an augmenting path

if there exists an augmenting path, M is not maximum

>[!CODE] genericMaximumMatching(G=(V,E))
>	m = []
>	while true
>		P = augmentingPath(G, M)  // returns an augmenting path or null if there is none
>		if P not null:
>			m = m.append(P)
>		else: return null


# Bipartite Graphs

>[!DANGER] Bipartite
>an undirected graph is bipartite if it contains no odd cycles or equivalently there exists a bipartitionn (L, R) of its node set such that all its edges have one endpoint in L and the other endpoint in R.

![[bipartite.png]]
>[!DANGER] Maximum Bipartite Matching
>the MBM problem is the special case of MM where the input graph is bipartite

## Reduction to max flow
- Orient edges from L to R
- Add a source node s and edges from s to L
- add a target node t and edges from R to t
- compute an integral max flow with edge capacities equal to 1 (values 0 and 1)
- return edges in LxR with flow equal to 1

>[!FAQ] MBM can be solved in O(mn) time

there is a simpler and more direct way to achieve the same goal via augmenting paths
- it is sufficient to find an augmenting path if any in O(m) time
- the generic maximum matching algorithm gives time O(mn)

(G, M) admits an augmenting path P iff there exists a simple direct path P' in G' from a free node in L to a free node in R

Given a bipartite graph G and a matching M of G, in O(m) time one can find an augmenting path for (G, M) if any


## Hopcroft - Karp Algorithm

augment along multiple augmenting paths in parallel
augment along shortest augmenting paths first

can be solved in time $O(m\sqrt n)$
### Marriage theorem
>[!DANGER] a matching is perfect when all nodes are matched

>[!DANGER] Theorem marriage
>$G=(LuR, E)$ with $\vert L \vert = \vert R \vert$ has a perfect matching if for any set S in L, the number of unique neighbors must be equal to or bigger than the set size
>

![[marriage thm.png]]
# Matching in Non-bipartite graphs

it is again sufficient to find an augmenting path if any
more complex due to the presence of blossoms

>[!DANGER] blossom
>given a graph G = (V, E) and a matching M of F, a **blossom** (of G w.r.t. M) is an odd-length cycle where edges alternate between M and E\\M excluding 2 consecutive edges in E\\M

![[blossom.png]]

>[!DANGER] Blossom contraction
>given a blossom B, contract it into one node v(B) (this might create parallel edges of which we keep one copy only) Let G'=G | B be the resulting graph

![[blossom contraction 1.png]]

![[blossom contraction 2.png]]

>[!DANGER] Blossom expansion
>Let M' be a matching of G' = G | B for a blossom B with **|B|=2k + 1**. Then a matching M= expand(G, B, M') of G of size **|M'|+k** can be obtained by decontracting the blossom and adding to the edges corresponding to M' in G k extra edges between decontracted nodes. This is always possible since at most 1 edge of M' is incident to v(B)

>[!DANGER] Blossom Contraction lemma
>consider a blossom B of size 2k + 1. Let G' = G and M' be a maximum matching of G'. Then M=expand(G, B, M') is a maximum matching of G


# Finding Augmenting Paths or Blossoms

>[!CODE] AugPathOrBlossom(G=(V, E), M)
>	set l(v) = even if v free, unmarked otherwise
>	for each even node u:
>		create tree T_u containing u
>	U = list initialized with even nodes
>	while U not empty
>		extract v in T_u from U
>		for each {v, w} in E
>			if I(w) = unmarked
>				let w' be such that {w, w'} in M
>				T_u= 
>	TBD

![[augpathorblossom.png]]

# Edmond's Algorithm

>[!CODE] Edmonds(G= (V, E), M)
>	M' = M
>	while true
>		PB = augPathOrBlossom(G, M') // Returns an augmenting path or blossom if any
>		if PB = null: 
>			return M'
>		if PB is an augmenting path:
>			M' = M' with edges of PB // flips edges 
>		else: 
>			M'' = Edmonds(M' contracted)
>			M' = M'' expanded
>			return M


>[!FAQ] Edmonds algo solves MM in O(mn) time


>[!DANGER] Maximum Weight Matching MWM
>given an edge-weighted undirected graph G, the maximum weight matching problem is to compute a matching M of maximum weight w(M) which is the sum of all weights of edges

