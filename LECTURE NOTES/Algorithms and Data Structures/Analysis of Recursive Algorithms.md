# Iteration Method

Idea is to unroll the recurrence to get an explicit formula by induction

>[!FAQ] Example of Iteration method
>$$T(n) \leq \begin{cases} c & \text{if } n = 0,1 \\ c + T(n/2) & \text{otherwise} \end{cases}$$
>One has
>$T(n)\leq c+ T(n/2) \text{ and } T(n/2)\leq c+T(n/4) \text{ hence } T(n) \leq2c+T(n/4)$
>Repeating the process one obtains 
>$T(n)\leq i*c+T(n/2^i) \text{ hence }$
>$T(n)\leq q*c+T(n/2^q) = q*c+T(1) = (q+1)c = (1+log_2 n)c = O(log n)$


# Substitution Method
guess the format of the answer and then prove that the guess is correct by induction 

>[!FAQ] Example of Substitution Method
>$$T(n) \leq \begin{cases} c & \text{if } n = 0,1 \\ c + T(n/2) & \text{otherwise} \end{cases}$$
>We guess that $T(n)\leq c*n$ for some constant $c$
>The base case $n=1$ satisfied by $c\geq1$
>In the inductive case $n\geq 1$ we have $$T(n) \leq n + T(n/2) \leq n+c*n/2 = (c/2+1)n$$
>We need to impose $(c/2+1)n \leq cn$ which is satisfied by $c\geq2$
>We can conclude that $T(n) \leq 2n$
# Master Theorem

>[!DANGER] The Master Theorem
>$$T(n) \leq \begin{cases} c & \text{if } n \leq b \\ a*T(\frac{n}{b}) + f(n) & \text{otherwise} \end{cases}$$
>Has solution
>1. $\Theta(n^{log_ba})$ if $f(n) = O(n^{log_b a-\epsilon})$ for some constant $\epsilon > 0$
>2. $\Theta(n^{log_ba}log n$ if $f(n) = \Theta(n^{log_ba})$
>3. $\Theta(f(n))$ if $f(n) = \Omega(n^{log_ba+\epsilon})$ for some constant $\epsilon > 0$ and $a*f(n/b)\leq c*f(n)$ for some constant $c<1$ and $n$ large enough


# Randomized Algorithms

In a randomized algorithm we assume that we have access to a source of (unbiased) random bits that the algorithm can use to make certain decisions

## Las Vegas

>[!DANGER] Las Vegas algorithm
>A Las Vegas algorithm is a randomized algorithm that always gives a correct answer to the considered problem and whose running time is a random variable


## Monte Carlo

>[!DANGER] Monte Carlo
>A Monte Carlo algorithm is a randomized algorithm whose running time is deterministic and that returns the correct answer with probability $\geq 1/2 + \epsilon$


## Probability theory

>[!FAQ] Formulas recap
>Expected value
>$E[X]=\sum_{d\in D(X)}*d*P(X=d)$
>Variance
>$Var(X) = E[(X-E[X]^2] =E[X^2]-(E[X])²$
>Independence
>$P(X_1=d_1, X_2=d_2) = P(X_1=d_1)*P(X_2=d_2)$ 
>Union bound
>Given a set of random variables $X_1, ... X_n$ 
>$P(X_1=d_1, ..., X_n=d_n)\leq \sum_i P(X_i=d_i)$ for all $d_i\in D(X_i)$

## Expected Running Time
Consider a LV randomized algorithm that uses a sequenc eB of random bits and let $T(I,b)$ be the time taken by the algo on instance I when B=b. Then the expected running time on instance I is 
$E(I)=Ex_b[T(I,B)]=\sum_b T(I,b)*P(B=b)$
The worst expected running time is 
$E(n)=max_{I:\vert I\vert =n}E(I)$



