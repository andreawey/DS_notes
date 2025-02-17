### 1. **For Discrete Random Variables (Probability Mass Function)**

The probability mass function (PMF) for a discrete random variable $X$ is given by:

$$
P(X = x) \quad \text{where} \quad \sum P(X=x_i) = 1 \quad \text{for all} \quad x_i
$$

The sum of all probabilities for a discrete random variable must equal 1.

### 2. **For Continuous Random Variables (Probability Density Function)**

The probability density function (PDF) for a continuous random variable $Y$ is given by:

$$
f(y) \quad \text{where} \quad \int_{a}^{b} f(y) \, dy = 1
$$

The integral of the PDF over its entire range must equal 1.

### 3. **Expected Value (Mean)**

The expected value for a discrete random variable $X$ is:

$$
E[X] = \sum x_i P(X = x_i)
$$

For a continuous random variable $Y$:

$$
E[Y] = \int_{a}^{b} y f(y) \, dy
$$

### 4. **Variance**

The variance for a discrete random variable $X$ is:

$$
\text{Var}(X) = E[X^2] - (E[X])^2
$$

where:

$$
E[X^2] = \sum x_i^2 P(X = x_i)
$$

For a continuous random variable $Y$:

$$
\text{Var}(Y) = E[Y^2] - (E[Y])^2
$$

### 5. **Standard Deviation**

Standard deviation is the square root of the variance:

$$
\sigma_X = \sqrt{\text{Var}(X)} \quad \text{or} \quad \sigma_Y = \sqrt{\text{Var}(Y)}
$$

### 6. **Linear Transformation of Random Variables**

If $Y = aX + b$, then:

$$
E[Y] = aE[X] + b
$$

$$
\text{Var}(Y) = a^2 \text{Var}(X)
$$

### 7. **Central Limit Theorem**

The sum of $n$ independent, identically distributed (i.i.d.) random variables, as $n$ becomes large, approximates a normal distribution. If $X_i \sim N(\mu, \sigma^2)$, then the sum of $n$ random variables is approximately:

$$
\text{Sum}(X) \sim N(n\mu, n\sigma^2)
$$

### 8. **Hypothesis Testing**

**Null Hypothesis** $H_0$ and **Alternative Hypothesis** $H_A$:
- If $H_0: \mu = \mu_0$, you test whether the observed sample mean is significantly different from $\mu_0$.
- P-value: The probability of observing a test statistic at least as extreme as the one observed under $H_0$.

For a **two-sided t-test**:

$$
t = \frac{\bar{x} - \mu_0}{\frac{s}{\sqrt{n}}}
$$

where $\bar{x}$ is the sample mean, $s$ is the sample standard deviation, and $n$ is the sample size.

The p-value is obtained using the t-distribution for the computed $t$-statistic.

### 9. **Confidence Intervals**

For the population mean with unknown standard deviation, the 95% confidence interval is:

$$
\left[ \bar{x} - t_{\alpha/2} \frac{s}{\sqrt{n}}, \, \bar{x} + t_{\alpha/2} \frac{s}{\sqrt{n}} \right]
$$

where $t_{\alpha/2}$ is the critical value from the t-distribution, $\alpha = 0.05$ for a 95% CI.

### 10. **QQ Plot**

A QQ plot is used to assess if the data follow a normal distribution. If the points lie roughly along a straight line, the data can be assumed to follow a normal distribution.

### 11. **Variance of a Sum of Independent Variables**

For two independent random variables $X$ and $Y$, the variance of the sum is:

$$
\text{Var}(aX + bY) = a^2 \text{Var}(X) + b^2 \text{Var}(Y)
$$

(where $a$ and $b$ are constants).

### 12. **Binomial Distribution**

The binomial distribution describes the number of successes in $n$ independent Bernoulli trials:

$$
P(X = k) = \binom{n}{k} p^k (1-p)^{n-k}
$$

where $p$ is the probability of success on a single trial.

### 13. **Normal Distribution**

A normally distributed random variable $X \sim N(\mu, \sigma^2)$ has the probability density function:

$$
f(x) = \frac{1}{\sigma \sqrt{2\pi}} e^{-\frac{(x-\mu)^2}{2\sigma^2}}
$$
