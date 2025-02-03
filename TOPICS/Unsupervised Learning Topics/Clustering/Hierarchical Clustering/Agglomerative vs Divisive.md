[[Hierarchical clustering]]

Clustering Tree Construction

In most applications, the clustering tree is built agglomeratively:

- Start with subsets which consist of one sample each.
- In each step, merge the two “closest” subsets.
- Finish when only one set (containing all samples) is left.

One can also build the tree divisively, starting from a set of all samples and refining, but this is the rarer case. A reason for preferring agglomerative clustering: the complexity is lower, comparing:

- For $N$ samples, always $N - 1$ merge or split steps for both.
- For merging (agglomerating), in each step consider $O(N^2)$ possible merges.
- For splitting (dividing), in each step consider $O(2^N)$ possible splits.
