The Fundamental Algorithm for [[Centroid based clustering]]

Ordinarily, the **Euclidean distance** is used as the distance metric.

- Assume there are $N$ data points $x_n \in \mathbb{R}^D$, $n = 1, \dots, N$, which are to be assigned to $K$ clusters with means $\mu_k$, $k = 1, \dots, K$.
- Assignments of data points are hard: each point should belong to exactly one cluster.
- We describe the assignment of data points to clusters by the variables $\gamma_{n,k}$: If data point $x_n$ belongs to cluster $k$, $\gamma_{n,k} = 1$, otherwise $\gamma_{n,k} = 0$.
- Clearly, $\gamma_{n,k} \in \{0, 1\}$ since the assignments are hard, and $\sum_n \gamma_{n,k} = 1$ for all $n$.

## Objective Criterion
Minimize the sum of distances of each data point to the centroid of its assigned cluster, which can be written as a formula: $$ J = \sum_{n=1}^{N} \gamma_{n,k} \| x_n - \mu_k \|^2 \quad \text{with} \quad \gamma_{n,k} \in \{0, 1\} $$ Another way of writing the same formula, where $k(x)$ is the cluster assigned to a point $x$: $$ J = \sum_{n=1}^{N} \| x_n - \mu_{k(x_n)} \|^2 $$ Unfortunately, there is no closed formula for minimizing $J$, since the cluster mean depends on the cluster assignment and vice versa. Checking all combinations is $O(K^N)$, hence not practical.

# Standard Algorithm

Optimize $J$ iteratively by alternatingly improving the cluster assignments and the cluster means!

- **Initialize** the cluster means randomly.
  
- **Assignment step**: Keep the $\mu_k$ fixed and assign each data point to the cluster whose mean is nearest, i.e.

$$
\gamma_{n,k} =
\begin{cases}
1 & \text{if } k = \arg \min_j \| x_n - \mu_j \|^2 \\
0 & \text{otherwise}
\end{cases}
$$

- **Update step**: Keep $\gamma_{n,k}$ fixed and recompute

$$
\mu_k = \frac{1}{S_k} \sum_{n=1}^{N} \gamma_{n,k} x_n
$$

- $S_k = \sum_{n=1}^{N} \gamma_{n,k}$ is the number of datapoints assigned to the cluster.


# Standard Algorithm Notes

- Each step reduces the value of $J$ (convergence proof) unless a local minimum is found.
- However, a local minimum instead of a global one may be reached.
- Convergence may also be very slow with large datasets. Possible stopping conditions:
  - Fixed number of iterations
  - Improvement below threshold
  - Cluster assignment does not change

# Proof of Convergence

- **Assignment step**: $J_{\text{new}} = \| x_n - \mu_k^{\text{new}} \|^2$ where $\mu_k^{\text{new}}$ is selected to minimize the expression. Since the number of cluster means is finite and fixed, it has to be larger or equal to the old value $J_{\text{old}} = \| x_n - \mu_k^{\text{old}} \|^2$ by the assignment rule.

- **Update step**, consider each individual cluster $J_k$, where $J = \sum_{k=1}^{K} J_k$. So, if $J_k$ is minimized, then so is $J$. Show that computing the new mean: 
  $$
  \mu_k^{\text{new}} = \frac{1}{N_k} \sum_{j=1}^{N_k} x_j
  $$ 
  minimizes $J_k$.

- Deriving: 
  $$
  \frac{\partial J_k}{\partial \mu} = -2 \left( \sum_{j=1}^{N_k} x_j - N_k \mu \right)
  $$ 
  Setting it equal to zero yields what we want to show.


# Initialization

The initialization of the cluster means has a great influence on the convergence of the algorithm.

- **Standard**: Randomly choose $K$ data points as initial means.
   $+$ Selects points in high-density areas.
   $+$ Important to pick randomly and not the first $k$.
   $-$ No guarantee of fast convergence or global optimum.
  
- **Standard practice**: Run K-means several times with different initializations.

## Drawbacks

• Clusters are assumed to have similar shapes, in particular similar covariance matrices (mickey
mouse problem).
• There is no way to determine the optimal number of clusters