
> [!NOTE] Computational Problem
> is a binary relation $R \subseteq IxO$ between a set I of input instances and a set O of output instances, where there exists at least one (i,o) $\in R$ for each i $\in$ I


# Algorithm


> [!DANGER] Algorithm Formal Definition
> An Algorithm for a computational problem R is a procedure that, given i $\in$ I, computes with a finite (expected) number of elementary operations on o $\in$ O such that (i,o)$\in$ R

The computational complexity of an algorithm A is the amount $T_A(I)$ of a given resource that A needs to solve an instance I of the problem P

We focus on running time as resource, i.e. the number of elementary operations.

-> Find an approximate way to upper bound the running time of an algorithm, which describes its qualitative behavior for large inputs

# Asymptotic Running Time

In the running time analysis, neglect the constant additive terms and the constant multiplicative factors

>[!DANGER] Asymptotic Running Time formal Definition
>a function f(n), n$\geq$ 0 is (or belongs to) O(g(n)) if there exists constants c>0 and k$\geq$ 0 such that $$f(n)\leq c* g(n) \textit{for all } n\geq k$$ 

>[!DANGER] Lower bound the asymptotic running time
>a function f(n), n $\geq$ 0 is $\Omega(g(n))$ if there exists constants c>0 and k $\geq$ 0 such that $$f(n) \geq c*g(n) \textit{for all } n\geq k$$

>[!DANGER] Tight asymptotic bound
>a function f(n), n$\geq$ 0 is $\Theta (g(n))$ if there exists constants $c_2 \geq c_1 \gt 0$ and $k\geq 0$ such that $$c_1 * g(n) \leq f(n) \leq c_2 * g(n) \textit{for all } n\geq k$$
>-> This is equivalent to being $\Omega(g(n)) \textit{and} O(g(n))$


# Worst-Case Analysis

>[!DANGER] Average Case
>let $Pr_n(I)$ be the probability that I is the input conditioned on the fact that the input size is n. Then $$T(n) = \sum_{\textit{instance} :\vert I\vert =n} Pr_n(I) * T_A(I)$$

Worst case:
$$T(n) = max_{\textit{isinstance} I:\vert I\vert =n} T_A (I)$$



# Amortized Analysis

>[!DANGER] Amortized running time
>the amortized running time of a DS is $\sigma = sup_S{\frac{T(S)}{\vert S\vert}}$ where:
>- S is any sequence of $\vert S\vert$ operations applied to the data structure from a standard initialization state
>- T(S) is the total time to execute those operations

Intuitively this is the worst-case average running time: any q operations take at most $\sigma q$ time
- Some operations might be much more expensive than others, however such operations could be rare, so $\sigma$ can be way smaller than the worst-case running time

Sometimes we define an amortized time $\sigma_i$ per operation type i. So in case of $q_i$ operations of type i, the total time is at most $\sum_i \sigma_i q_i$

# Banker's method

- we exploit virtual coins in the analysis
- each such coin can be used to pay a fixed O(1) amount of work
- during cheap operations we save coins and during expensive operations we use previously saved coins to reduce the amount of work
- The maximum number of used coins in a given operation is a valid bound on the amortized cost up to constant factors

# Potential Function method
- In the potential function method we define a potential function $\phi$ (D) of the current state D of the data structure
- Let $D_0$ be the initialization state and $D_i$ be the state of D at the end of the i-th operation.
- The potential function has to satisfy:
	(1) $\phi (D_0) = 0$
	(2) $\phi(D_i) \geq 0$ for all i

- The amortized cost $a_i$ of the i-th operation is its actual cost $t_i$ plus the change $\phi (D_i) - \phi(D_{i-1})$ of the potential multiplied by a fixed constant $c>0$: 
  $$a_i=t_i+c*(\phi(D_i) -\phi(D_{i-1}))$$
- The sum of the amortized costs is a valid upper bound on the total cost. Indeed by a telescopic sum arguments, one gets
  $$\sum_{i\in S} a_i = \sum_{i\in S}t_i+c*(\phi(D_i)- \phi(D_{i-1})) = t(S) + c\phi(D_{\vert S\vert})- c\phi(D_0) \geq t(S)$$
- Hence $max_i{a_i}$ is a valid upper bound on the amortized cost 
  $$max_i{a_i} \geq \frac{1}{\vert S\vert} \sum_{i\in S} a_i \geq \frac{1}{\vert S\vert}\sum_{i\in S} t_i = \frac{1}{\vert S\vert} t(S)$$
- 