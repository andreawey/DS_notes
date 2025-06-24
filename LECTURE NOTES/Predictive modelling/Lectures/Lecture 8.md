# Predictive Modeling - Forecasting

## Stochastic Processes Review

### White Noise Process

> [!definition] White Noise 
> A white noise process consists of independent and identically distributed random variables ${W_1, W_2, \ldots}$ where each $W_i$ has mean 0 and variance $\sigma^2$.

### Random Walk

> [!definition] Random Walk 
> Choose $n$ independent Bernoulli random variables $D_1, \ldots, D_n$ that take values 1 and -1 with equal probability $p = 0.5$.
> 
> Define $X_i = D_1 + \cdots + D_i$ for each $1 \leq i \leq n$.
> 
> **Random walk with drift:** $$X_i = X_{i-1} + \delta + D_i$$

### Moving Average Process

> [!definition] Moving Average Process 
> Apply a sliding window filter to white noise process ${W_1, W_2, \ldots}$.
> 
> For window length 3: $$V_i = \frac{1}{3}(W_{i-1} + W_i + W_{i+1})$$

### Autoregressive Model

> [!definition] Autoregressive Process 
> Recursively defined sequence where current value depends on past values: $$X_i = 1.5X_{i-1} - 0.9X_{i-2} + W_i$$

## Measures of Dependence

### Mean Sequence

> [!definition] Mean Sequence 
> The mean sequence ${\mu(1), \mu(2), \ldots}$ of a discrete stochastic process ${X_1, X_2, \ldots}$ is: $$\mu(i) = E[X_i]$$

### Autocovariance and Autocorrelation

> [!definition] Autocovariance 
> $$\gamma_X(i,j) = \text{Cov}[X_i, X_j] = E[(X_i - \mu(i))(X_j - \mu(j))]$$

> [!definition] Autocorrelation 
> $$\rho_X(i,j) = \frac{\gamma_X(i,j)}{\sqrt{\gamma_X(i,i)\gamma_X(j,j)}}$$

### Weak Stationarity

> [!important] Weak Stationarity A discrete stochastic process $X_i$ is weakly stationary if:
> 
> 1. The mean sequence $\mu_X(i)$ is constant and does not depend on time index $i$
> 2. The autocovariance sequence $\gamma_X(i,j)$ depends on $i$ and $j$ only through their difference $|i-j|$

### Sample Estimates

> [!formula] Sample Autocovariance 
> $$\hat{\gamma}(h) = \frac{1}{n}\sum_{i=1}^{n-h}(x_{i+h} - \bar{x})(x_i - \bar{x})$$
> 
> with $\hat{\gamma}(-h) = \hat{\gamma}(h)$ for $h = 0, 1, \ldots, n-1$

> [!formula] Sample Autocorrelation 
> $$\hat{\rho}(h) = \frac{\hat{\gamma}(h)}{\hat{\gamma}(0)}$$

### White Noise Test

> [!tip] White Noise Detection For white noise process with large $n$: sample autocorrelation $\hat{\rho}(h)$ is approximately normal with:
> 
> - Mean: $-1/n$
> - Standard deviation: $\sigma_{\hat{\rho}} = 1/\sqrt{n}$
> 
> **95% confidence limits:** $-1/n \pm 2/\sqrt{n}$

## Autoregressive Models

### AR(p) Definition

> [!definition] Autoregressive Model AR(p) 
> $$X_n = a_1 X_{n-1} + a_2 X_{n-2} + \cdots + a_p X_{n-p} + W_n$$
> 
> where $a_1, a_2, \ldots, a_p$ are model parameters and ${W_1, W_2, \ldots}$ is white noise with variance $\sigma^2$.
> 
> **Characteristic polynomial:** $$\Phi(x) = 1 - a_1 x - a_2 x^2 - \cdots - a_p x^p$$

### AR(1) Properties

> [!example] AR(1) Process 
> $$X_n = a_1 X_{n-1} + W_n$$
> 
> **Mean:** $\mu = 0$ (if $a_1 \neq 1$)
> 
> **Variance:** $$\sigma_X^2 = \frac{\sigma^2}{1 - a_1^2}$$
> 
> **Stationarity condition:** $|a_1| < 1$

### Stationarity Condition for AR(p)

> [!important] Stationarity of AR(p) 
> An AR(p) process is weakly stationary if all (complex) roots of the characteristic polynomial $$\Phi(x) = 1 - a_1 x - a_2 x^2 - \cdots - a_p x^p$$ exceed 1 in absolute value.

### Autocorrelation Properties

> [!note] AR Process Autocorrelation
> 
> - **AR(1):** Theoretical autocorrelation at lag $h$ is $a_1^h$
> - **General AR(p):** Autocorrelation decays exponentially (possibly with oscillations)
> - Nonzero for wide span of lags due to correlation propagation

## Partial Autocorrelation

> [!definition] Partial Autocorrelation For a weakly stationary process ${X_1, X_2, \ldots}$: $$\pi(h) = \text{Cor}[X_k, X_{k+h} | X_{k+1}, \ldots, X_{k+h-1}]$$

> [!important] Key Properties
> 
> 1. The $p$-th coefficient $a_p$ of an AR(p) process equals $\pi(p)$
> 2. For AR(p) process: $\pi(k) = 0$ for $k > p$

### Model Order Determination

> [!tip] Determining AR(p) Order
> 
> 1. Compute partial autocorrelation
> 2. Choose largest lag $k$ for which $\pi(k) \neq 0$
> 3. This gives the order $p$ of the AR(p) model

## Model Fitting Procedure

> [!workflow] AR Model Fitting Steps
> 
> 1. **Model Selection:** Check if autocorrelation decays exponentially and partial autocorrelation is zero for larger lags
> 2. **Order Selection:** Choose largest lag $p$ where partial autocorrelation is non-zero
> 3. **Parameter Estimation:** Fit parameters $a_1, \ldots, a_p$ using least squares or other methods

### Parameter Estimation Methods

> [!note] Estimation Methods
> 
> - **Ordinary Least Squares**
> - **Burg's Algorithm**
> - **Yule-Walker**
> - **Maximum Likelihood Method**

### Least Squares System

For AR(p) with data ${x_1, x_2, \ldots, x_n}$:

$$\begin{align} x_{p+1} &= a_1 x_p + a_2 x_{p-1} + \cdots + a_p x_1 + W_{p+1}\ x_{p+2} &= a_1 x_{p+1} + a_2 x_p + \cdots + a_p x_2 + W_{p+2}\ &\vdots\ x_n &= a_1 x_{n-1} + a_2 x_{n-2} + \cdots + a_p x_{n-p} + W_n \end{align}$$

## Forecasting

### k-step Ahead Forecast

> [!definition] k-step Ahead Forecast For stationary process with observed data ${x_1, x_2, \ldots, x_n}$: $$\hat{X}_{n+k} = E[X_{n+k} | X_1 = x_1, \ldots, X_n = x_n]$$

### AR(1) Forecasting

> [!example] AR(1) Forecast For AR(1): $X_k = a_1 X_{k-1} + W_k$ with $|a_1| < 1$
> 
> **1-step ahead:** $$\hat{X}_{n+1} = a_1 x_n$$
> 
> **k-step ahead:** $$\hat{X}_{n+k} = a_1^k x_n$$
> 
> The forecast decays exponentially to zero from the last observation.

### Confidence Intervals

> [!formula] Forecast Confidence Intervals **Standard error:** $\sigma_k = \sqrt{\text{Var}[X_{n+k} | X_1 = x_1, \ldots, X_n = x_n]}$
> 
> **95% Confidence Interval:** $$\hat{X}_{n+k} \pm 1.96\sigma_k$$
> 
> Note: $\sigma_k$ increases with $k$ and converges to process variance $\sigma_X^2$

## Forecasting Procedure

> [!workflow] Three-Step Forecasting Process
> 
> 1. **Predictability Check:** Ensure the process will continue probabilistically as observed
> 2. **Model Selection & Fitting:**
>     - Choose model class through exploratory data analysis
>     - Fit model to training data
>     - Obtain model parameters
> 3. **Prediction:** Use fitted model to predict future values

---

## Key Takeaways

> [!summary] Important Points
> 
> - **Stationarity is crucial** for AR modeling and forecasting
> - **Partial autocorrelation** is the key tool for determining AR model order
> - **Exponentially decaying autocorrelation** suggests AR process
> - **Forecast uncertainty increases** with prediction horizon
> - **Model validation** through residual analysis is essential

