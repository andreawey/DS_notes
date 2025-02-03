The AHC (Agglomerative [[Hierarchical clustering]]) Algorithm

The AHC algorithm: a generic method to create a hierarchical clustering, works for all possible linkage functions.

- Initialize for each data element $x_i \in X$ its cluster singleton $G_i = x_i$.
- Initialize the list of “current” clusters as $L = G_i$.
- While $L$ has at least two elements, do:
  - Choose $G$ and $H$ such that $\Delta(G, H)$ is minimal among all $G, H \in L$.
  - Merge $I = G \cup H$ (and remember that $I$ has been created from $G$ and $H$).
  - Add $I$ to the list, remove $G$ and $H$ from the list.
- Stop when the length of $L$ is 1, that’s the root node.


# Time Complexity

Exactly $N - 1$ merges + $O(N^2)$ comparisons $\Rightarrow O(N^3)$.

- The clustering tree may not be unique since there can be several “closest” pairs of subsets. This is particularly common if the elementary distance only takes a fixed set of values (e.g., integers when the Hamming distance is used).
- **Single linkage** tends to create chaining, where a series of singletons are merged into the tree.
- **Complete linkage** (which can be implemented in a similar way) is very sensitive to outliers.
- With dynamic programming and single linkage, an $O(N^2)$ implementation can be achieved, keeping the updated nearest neighbors in an array, **SLINK algorithm**.
