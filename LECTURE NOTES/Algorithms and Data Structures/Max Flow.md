given:
- n-node m-edge digraph G=(V,E)
- non negative edge capacities
- a source node s and a target node t

![[flowintro.png]]

>[!DANGER] Flow definition
>a flow is an assignment of values f:E->R such that:
>1. capacity constraint, the flow on any edge must be between 0 and the capacity of the edge
>2. flow conservation: for every node v (except starting and end node s, t) 
>   Total flow in = total flow out
>flow value: the total amount of flow leaving the source s

![[DS_notes/META/images/flowvalue.png]]

>[!DANGER] Max flow problem is to compute a flow of maximum value

>[!FAQ] There can be different optimal solutions

![[flowsolutions.png]]

# Flows and Cuts

>[!DANGER] S-T cut
>an s-t cut is any split of the graph into two groups where start(s) is one and end(t) is in the other
>Capacity: the sum of all edges crossing from the start group to the end group

![[st cut.png]]

>[!DANGER] Minimum S-T cut
>the min cut problem is to find an s-t cut of minimum capacity

![[minstcut.png]]
>[!DANGER] Flow Value
>let f be any s-t flow, and (AB) be any s-t cut. Then the net flow sent across the cut is equal to the flow value

![[flowvalue 1.png]]
![[flowvalue 2.png]]

>[!DANGER] Weak duality
>let f be any s-t flow and (A,B) be any s-t cut. then the value of the flow is at most the capacity of the cut: v(f) $\leq$ cap(A,B)

![[weakduality.png]]

>[!DANGER] certificate of optimality (COR)
>let f be any s-t flow, and (A,B) be any s-t cut. if v(f) = cap(A,B) the f is a max flow and (A,B) is a min cut

![[cor.png]]

# Algorithms

## Greedy
>[!CODE] Greedy max flow
>for all e in E: f(e) = 0
>while there exists an s-t path P whose edges are not at full capacity
>	augment flow along P 
>return F

-> this is not a max flow

# Residual graph

>[!DANGER] Residual graph
>given any s-t flow f, for any edge e=(a,b) we introduce a **reverse edge** $e^R = (b,a)$ 
>The residual capacities of these edges are defined as:
>$$c_f(e)= \begin{cases} c(e)-f(e) if e \in E \\ f(e^R) if e not \in E \end{cases}$$
>
>The residual graph $G_f = (V, E_f)$ is given by the original and reverse edges with positive residual capacity

![[residual graph.png]]


# Ford-Fulkerson (FF) Algorithm

>[!CODE] FF algo
>1. initialize flow f=0 for every edge
>2. build the residual graph G_f
>3. while there is a path p from s to t in G_f
>	a. find bottleneck (the smallest capacity on path P)
>	b. for each edge (u,v) in path P
>		if (u,v) is a forward edge
>			f(u,v) = f(u,v) + b || push flow forward
>		else( it is a backward edge):
>			f(v, u) = f(v,u) -b || cancel flow backward
>	c. update residual graph GF based on the new flows
>4. Return f


# Capacity Scaling Algorithm

choose augmenting paths carefully so that:
- the number of iterations is reduced
- each augmenting path can be found quickly enough

use an augmenting path with maximum increase of the flow

>[!DANGER] Scaling Parameter
>for a scaling parameter D, G_f(D) is the subgraph of the residual graph G_f consisting only of edges wiht capacity at least D

>[!CODE] scalingMaxFlow
>1. Initialize a threshold D (large power of 2)
>2. construct a subgraph G_f(D) containing only edges with capacity $\geq$ D
>3. Find paths and push flow in this restricted graph
>4. When stuck cut D in half (D/2) and repeat
>5. Stop when D < 1



# Applications

# Edge Disjoint Paths

given a digraph G and two nodes s and t, the (s-t edge) disjoint path problem (EDP) is to find the maximum number of s-t paths which are pairwise edge disjoint (they do not share any edge)

![[EDP.png]]
Reduce it to max flow: 
- compute an integral max-flow with capacities set to 1
- extract s-t paths one by one from edges with flow 1
![[edpmaxflow.png]]

the maximum number of edge disjoint paths equals the max flow value

>[!DANGER] S-t connectivity
>given a digraph and two nodes s and t, the **s-t connectivity** is the minimum number of edges F whose removal disconnects s from t


# Circulations

>[!DANGER] circulation
>given a directed capacitated graph (G, c) and values d on nodes. A node v is a demand supply and transshipment if d(v) is positive negative and 0, resp. A circulation is a function f on edges

reduction to max flow: 
- add a source node s and connect it to each supply node v with c(s, v) = -d(v)
- add a target node t and connect each demand node v to t with c(v, t) = d(v)
- a circulation exists iff max flow value = cap({s}, V\\{s})
