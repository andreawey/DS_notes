# ARIMA Models - Study Notes

## Autoregressive Model AR(p)

> [!definition] AR(p) Model 
> The autoregressive model of order p is a discrete stochastic process that satisfies: $$X_n = a_1X_{n-1} + a_2X_{n-2} + \cdots + a_pX_{n-p} + W_n$$ where $a_1, a_2, \ldots, a_p$ are the model parameters and $W_1, W_2, \ldots$ is a white noise process with variance $\sigma^2$.

> [!info] AR(p) Intuition Autoregressive models are based on the idea that the current value of the series, $X_n$, can be explained as a linear combination of $p$ past values, together with an additive random error.

### Characteristic Polynomial

> [!important] Characteristic Polynomial For a given AR(p) process, the characteristic polynomial is defined as: $$\Phi(z) = 1 - a_1z - a_2z^2 - \cdots - a_pz^p$$

> [!note] Stationarity Condition If $\Phi(z) = 0$ for all $z \in \mathbb{C}$ with $|z| > 1$, then the process is stationary.

### Backshift Operator

> [!definition] Backshift Operator The backshift operator $B$ for a discrete process $X_n$ is defined as: $$B(X_n) = X_{n-1}$$
> 
> Multiple applications: $X_{n-p} = B^p(X_n)$

### Autoregressive Operator

> [!important] Autoregressive Operator $$\Phi(B) = 1 - a_1B - a_2B^2 - \cdots - a_pB^p$$
> 
> With this notation: $\Phi(B)X_n = W_n$

### ACF and PACF Patterns

> [!tip] AR(p) Identification
> 
> - **ACF**: trails off (oscillating)
> - **PACF**: cuts off after lag $h = p$

## Moving Average Model MA(q)

> [!definition] MA(q) Model 
> The moving average model of order q is a discrete stochastic process that satisfies: $$X_n = W_n + b_1W_{n-1} + b_2W_{n-2} + \cdots + b_qW_{n-q}$$ where $b_1, b_2, \ldots, b_q$ are the model parameters and $W_1, W_2, \ldots$ is a white noise process with variance $\sigma^2$.

> [!info] MA(q) Properties
> 
> - MA(q) models are **always stationary**
> - They can model dependencies in the noise structure

### Moving Average Operator

> [!important] Moving Average Operator $$\Theta(B) = 1 + b_1B + b_2B^2 + \cdots + b_qB^q$$
> 
> With this notation: $X_n = \Theta(B)W_n$

### Invertibility

> [!warning] Invertibility Problem Different MA(q) processes may have the same ACF, making them indistinguishable from observations.

> [!important] Invertibility Condition A MA(q) process is invertible if: $$\Theta(z) = 0 \text{ for all } z \in \mathbb{C} \text{ with } |z| > 1$$
> 
> For a given ACF, there is at most one corresponding invertible MA(q) process.

### ACF and PACF Patterns

> [!tip] MA(q) Identification
> 
> - **ACF**: cuts off after lag $h = q$
> - **PACF**: trails off (oscillating)

## ARMA Model

> [!definition] ARMA(p,q) Model 
> The autoregressive moving average model of order (p,q) is a discrete stochastic process that satisfies: $$\Phi(B)X_n = \Theta(B)W_n$$ where $\Phi(z)$ and $\Theta(z)$ are the p and q order characteristic polynomials of the autoregressive and moving average parts, respectively.

### Properties

> [!important] ARMA(p,q) Conditions **Stationarity and Causality**: $\Phi(z) = 0$ for all $z \in \mathbb{C}$ with $|z| > 1$
> 
> **Invertibility**: $\Theta(z) = 0$ for all $z \in \mathbb{C}$ with $|z| > 1$

> [!note] ARMA Identification Challenge ARMA(p,q) processes do not have distinct patterns in ACF and PACF (both are trailing off). Model order selection is not practical with ACF/PACF analysis alone.

## ARIMA Model

> [!definition] ARIMA(p,d,q) Model 
> The autoregressive integrated moving average model of order (p,d,q) is a discrete stochastic process that satisfies: $$\Phi(B)D^dX_n = \Theta(B)W_n$$ where $D = 1 - B$ is the differencing operator.

> [!info] ARIMA Purpose ARIMA combines the strengths of AR and MA models while removing trend in the data through differencing.

### Differencing Operator

> [!important] Differencing $$D = 1 - B$$
> 
> First difference: $y_n = (1-B)x_n = x_n - x_{n-1}$
> 
> d-th difference: $D^dx_n$

## Model Selection and Estimation

### AIC Criterion

> [!important] Akaike Information Criterion $$AIC(p,q) = -2\log(L(\boldsymbol{\theta}; \boldsymbol{x})) + 2(p + q + 1)$$
> 
> Choose the model with the **lowest AIC value**.

### Model Selection Steps

> [!tip] ARIMA Model Selection Process
> 
> 1. **Determine differencing order d**: Use the smallest d such that $D^dx_n$ is stationary
>     - Check stationarity visually or with Dickey-Fuller test
> 2. **Grid search for p and q**: Compute AIC for various combinations
> 3. **Select model**: Choose (p,d,q) with minimum AIC

### Likelihood Function

> [!note] Maximum Likelihood for ARMA(p,q) Assuming Gaussian white noise $W_n \sim N(0, \sigma^2)$: $$L(\boldsymbol{\theta}; \boldsymbol{x}) = (2\pi)^{-n/2} \cdot \det(\Gamma(\boldsymbol{\theta}))^{-1/2} \cdot \exp\left(-\frac{1}{2}\boldsymbol{x}^T\Gamma(\boldsymbol{\theta})^{-1}\boldsymbol{x}\right)$$
> 
> where $\Gamma(\boldsymbol{\theta})$ is the covariance matrix depending on parameters $\boldsymbol{\theta} = (a_1, \ldots, a_p, b_1, \ldots, b_q, \sigma^2)$.

## SARIMA Model

> [!definition] SARIMA(p,d,q)(P,D,Q)_s Model The seasonal ARIMA model satisfies: $$\Phi_P(B^s)\Phi(B)(1-B^s)^D(1-B)^dX_n = \Theta_Q(B^s)\Theta(B)W_n$$

### Operators

> [!important] SARIMA Operators **Non-seasonal operators**:
> 
> - $\Phi(B) = 1 - a_1B - a_2B^2 - \cdots - a_pB^p$
> - $\Theta(B) = 1 + b_1B + b_2B^2 + \cdots + b_qB^q$
> 
> **Seasonal operators**:
> 
> - $\Phi_P(B^s) = 1 - \phi_1B^s - \phi_2B^{2s} - \cdots - \phi_PB^{Ps}$
> - $\Theta_Q(B^s) = 1 + \vartheta_1B^s + \vartheta_2B^{2s} + \cdots + \vartheta_QB^{Qs}$

> [!info] SARIMA Purpose SARIMA models can directly incorporate both trend and seasonality, allowing for convenient forecasting of seasonal time series.

### Seasonal Differencing Example

> [!example] Monthly Data with Seasonality For monthly data with trend and seasonality:
> 
> 1. Remove trend: $Y_n = (1-B)X_n = X_n - X_{n-1}$
> 2. Remove seasonality: $Z_n = Y_n - Y_{n-12} = (1-B^{12})(1-B)X_n$

## Prediction

> [!important] k-step Ahead Predictor $$\hat{X}_{n+k} = E[X_{n+k} | X_1 = x_1, X_2 = x_2, \ldots, X_n = x_n]$$
> 
> This can be computed using the innovations algorithm.

## Key Takeaways

> [!summary] Model Comparison
> 
> |Model|ACF Pattern|PACF Pattern|When to Use|
> |---|---|---|---|
> |AR(p)|Trails off|Cuts off at lag p|Current value depends on past values|
> |MA(q)|Cuts off at lag q|Trails off|Dependencies in noise structure|
> |ARMA(p,q)|Trails off|Trails off|Combination of both effects|
> |ARIMA(p,d,q)|-|-|Non-stationary data with trend|
> |SARIMA|-|-|Seasonal and trending data|

