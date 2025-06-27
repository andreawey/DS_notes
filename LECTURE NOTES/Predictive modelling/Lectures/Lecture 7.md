# Mathematical Models for Time Series - Key Concepts

## Fundamental Definitions

> [!important] Time Series vs Stochastic Process
> 
> - **Time Series**: A concrete observation of values {x₁, x₂, ...} - the actual data we observe
> - **Discrete Stochastic Process**: A theoretical construct {X₁, X₂, ...} of random variables that models the underlying mechanism generating the values

> [!note] Key Distinction A time series is a **realization** of a discrete stochastic process. The value xᵢ is a realization of the random variable Xᵢ measured at time tᵢ.

## Types of Stochastic Processes

### 1. White Noise Process

> [!definition] White Noise A white noise process consists of independent and identically distributed random variables {W₁, W₂, ...} where each Wᵢ has:
> 
> - Mean: E[Wᵢ] = 0
> - Variance: Var(Wᵢ) = σ²

**Properties:**

- If normally distributed → **Gaussian white noise**
- Observations are uncorrelated
- Used to describe noise in engineering applications
- Term "white" indicates all possible periodic oscillations are present with equal strength

### 2. Random Walk

> [!definition] Random Walk 
> **Without drift:**
> 
> $$ X_i = X_{i-1} + D_i, \quad X_0 = 0$$
> 
> **With drift:**
> 
> $$ Y_i = \delta + Y_{i-1} + D_i, \quad Y_0 = 0$$
> 
> where Dᵢ are independent Bernoulli random variables taking values ±1 with probability 0.5

**Applications:**

- Random walk with drift models trends in time series data

### 3. Moving Average (MA) Process

> [!definition] Moving Average Process Applying a sliding window filter to white noise:
> 
>$$V_i = \frac{1}{3}(W_{i-1} + W_i + W_{i+1})$$

**Properties:**

- Results in a smoother process
- Higher order oscillations are smoothed out
- Creates serial correlation from uncorrelated white noise

### 4. Autoregressive (AR) Process

> [!definition] Autoregressive Process 
> **AR(2) Example:**
> 
> $$X_i = 1.5X_{i-1} - 0.9X_{i-2} + W_i$$

**Interpretation:**

- Current value is a linear combination of past values plus random component
- Can be interpreted as a harmonic oscillator with random input
- Exhibits oscillatory behavior

> [!note] Differential Equation Connection The AR process corresponds to the finite difference scheme:
> 
> $$\ddot{x} + 2\delta\dot{x} + \omega_0^2 x = W(t)$$

## Measures of Dependence

### Mean Sequence

> [!definition] Mean Sequence 
> The mean sequence {μ(1), μ(2), ...} of a discrete stochastic process {X₁, X₂, ...} is:
> 
> $$\mu(i) = E[X_i]$$

**Examples:**

- White noise: μ(i) = 0 for all i
- Random walk with drift: μ(i) = iδ


> [!note] Covariance Formula
> $$\gamma(i, j) = \mathrm{Cov}(X_i, X_j) = \mathrm{E}[X_i X_j] - \mu(i) \mu(j)$$

### Autocovariance and Autocorrelation

> [!definition] Theoretical Autocovariance and Autocorrelation 
> **Autocovariance:**
> 
> $$\gamma_X(i,j) = \text{Cov}[X_i, X_j] = E[(X_i - \mu(i))(X_j - \mu(j))]$$
> 
> **Autocorrelation:**
> 
> $$\rho_X(i,j) = \frac{\gamma_X(i,j)}{\sqrt{\gamma_X(i,i)\gamma_X(j,j)}}$$

**Properties:**

- Symmetric: γ(i,j) = γ(j,i)
- Autocorrelation: ρ(i,j) ∈ [-1,1]
- Measures linear dependence between observations at different times
- γ(i,j) = 0 means no linear dependence (but nonlinear dependence may exist)

> [!example] White Noise Autocovariance
> 
> $$ \gamma(i,j) = \begin{cases}
> 0 & \text{if } i \neq j \\
> \sigma^2 & \text{if } i = j
> \end{cases}
> $$

> [!example] MA(3) Process Autocovariance
> 
> $$ \gamma(i,j) = \begin{cases}
> \frac{3\sigma^2}{9} & \text{if } i = j \\
> \frac{2\sigma^2}{9} & \text{if } |i-j| = 1 \\
> \frac{\sigma^2}{9} & \text{if } |i-j| = 2 \\
> 0 & \text{otherwise}
> \end{cases}
> $$

## Stationarity

### Strict Stationarity

> [!definition] Strict Stationarity 
> A discrete stochastic process is **strictly stationary** if for each finite collection {X_{i₁}, ..., X_{iₙ}} and each lag h ∈ Z, the shifted collection {X_{i₁+h}, ..., X_{iₙ+h}} has the same distribution:
> 
> $$P(X_{i_1} \leq c_1, ..., X_{i_n} \leq c_n) = P(X_{i_1+h} \leq c_1, ..., X_{i_n+h} \leq c_n)$$

**Implication:** The probabilistic character of the process does not change over time.

### Weak Stationarity

> [!definition] Weak Stationarity A discrete stochastic process Xᵢ is **weakly stationary** if:
> 
> 1. The mean sequence μ_X(i) is constant (does not depend on time index i)
> 2. The autocovariance sequence γ_X(i,j) depends on i and j only through their difference |i-j|

> [!note] Practical Importance Weak stationarity is sufficient for most applications and easier to verify than strict stationarity.

**Testing Stationarity:**

- Plot the time series and look for trends
- Compute mean and autocovariance for different windows
- Look for dramatic changes that reject stationarity hypothesis

Conditions for weak stationarity:
- Constant mean over time
- constant variance over time
- covariance depends on the lag, not on time

## Sample Estimation

### Sample Mean

> [!formula] Sample Mean Estimator 
> For a stationary process with constant mean μ:
> 
> $$\hat{\mu} = \bar{x} = \frac{1}{n}\sum_{i=1}^n x_i$$

### Sample Autocovariance and Autocorrelation

> [!definition] Sample Autocovariance and Autocorrelation 
> **Sample Autocovariance:**
> 
> $$ \hat{\gamma}(h) = \frac{1}{n}\sum_{i=1}^{n-h}(x_{i+h} - \bar{x})(x_i - \bar{x})$$
> 
> with γ̂(-h) = γ̂(h) for h = 0, 1, ..., n-1
> 
> **Sample Autocorrelation:**
> 
> $$ \hat{\rho}(h) = \frac{\hat{\gamma}(h)}{\hat{\gamma}(0)}$$

### White Noise Test

> [!important] White Noise Detection 
> For a white noise process with large n, the sample autocorrelation ρ̂(h) is approximately normal with:
> 
> - Mean: -1/n
> - Standard deviation: 1/√n
> 
> **95% confidence bounds:** -1/n ± 2/√n
> 
> If the series is white noise, it should stay within these limits 95% of the time.

> [!tip] Practical Application These confidence bounds are automatically drawn in R (acf function) and Python as blue lines for white noise testing.

## Key Relationships

> [!summary] Process Characteristics Summary
> 
> - **White Noise**: Independent, no autocorrelation
> - **Random Walk**: Trend behavior, non-stationary
> - **Moving Average**: Smooth, finite autocorrelation structure
> - **Autoregressive**: Oscillatory behavior, infinite autocorrelation structure that decays

> [!warning] Important Notes
> 
> - Stationarity is a property of the **process**, not the observed time series
> - A single time series is one realization - stationarity assumptions allow statistical inference
> - Autocovariance depending only on lag |i-j| is crucial for weak stationarity

