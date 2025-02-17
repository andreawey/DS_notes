
A random variable $X$ is a function that assigns a numerical value to each possible outcome of a random experiment.

> $E(X)$  or $\mu$ denotes the expected value
> $V(X)$ or $Var(x)$ denotes the variance
> $S(X)$ or $\sigma$ denotes the standard deviation

There are two main types of random variables: 

1. **Discrete Random Variable**: Takes on a countable number of distinct values (ex. number of heads in coin toss)

	The variable $X$ can assume the values $x_1, x_2, x_3, ...$ with respective probability $p_1, p_2, p_3, ...$ such that
	$\sum\limits_{i} p_i = 1$ then:
	
	$E(X) = \sum\limits_{i} p_i x_i$
	$V(X) = \sum\limits_{i} p_i (x_i - E(X))^2 = \sum\limits_{i} p_i x^2_i - E^2(X)$
	$S(X) = \sqrt{V(X)}$

2. **Continuous Random Variable**: Takes on a countable number of values typically within an interval (ex. height of a randomly selected person)

	let $f$ be a function such that $f(x) \geq 0$ for all real numbers x and 
	$\int_{-\infty}^{\infty} f(x) \,dx = 1$
	
	$f$ is the *probability density function (PDF)* of a random variable $X$ if 
	$P(a \leq X \leq b) = \int_{a}^{b}f(x) \, dx$ 
	
	$F$ is the *cumulative distribution function (CDF)* of a random variable $X$ if
	$F(x) = P(X \leq x) = \int_{-\infty}^{x} f(t) \,dt$
	
	$E(X) = \int_{-\infty}^{\infty} x f(x) \,dx$
	 $V(X) = E(X^2) - (E(X))^2 = \int_{-\infty}^{\infty} x^2 f(x) \,dx - E^2(X)$
	 $S(X) = \sqrt{V(X)}$

## Independent random variables

Two discreet random variables $X$ and $Y$ are *independent* if for each 
$P(X=x,Y=y)=P(X=x)P(Y=y)$

if $X$ and $Y$ are independent:
$E(XY)=E(X)E(Y)$ 
$Var(X+Y)=Var(X)+Var(Y)$
