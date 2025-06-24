# State Space Models

> [!abstract] Definition State space formulation is a general framework for time series models where there is a stochastic process/time series which we cannot directly observe, only through the addition of measurement noise.

## Core Components

> [!info] Basic Structure State space models consist of two key equations:
> 
> 1. **State Equation**: Describes the evolution of the hidden state
> 2. **Observation Equation**: Relates observations to the hidden state

### State Equation

$$X_t = G_t X_{t-1} + W_t$$ where $W_t \sim N(0, w_t)$ (process noise)

### Observation Equation

$$Y_t = F_t X_t + V_t$$ where $V_t \sim N(0, v_t)$ (measurement noise)

> [!note] Key Properties
> 
> - $G_t$, $F_t$, $w_t$, $v_t$ can be time-varying matrices
> - Often they are time-constant; if anything, $F_t$ adapts over time
> - $W_t$ and $V_t$ are independent

## Fundamental Principles

> [!important] Two Core Principles
> 
> 1. **Hidden State Process**: $X_t$ is a Markov process - future states depend only on the present state, not the past sequence
> 2. **Conditional Independence**: Observations $Y_t$ are independent given the states $X_t$

## Example: AR(1) with Measurement Noise

### State Equation

$$X_t = \alpha_1 X_{t-1} + W_t$$ where $W_t \sim N(0, \sigma_W^2)$

### Observation Equation

$$Y_t = X_t + V_t$$ where $V_t \sim N(0, \sigma_V^2)$

> [!tip] Process vs Observation Noise
> 
> - **Process noise** $W_t$: Affects all future values $X_{t+k}$ and thus $Y_{t+k}$
> - **Observation noise** $V_t$: Only influences current observation $Y_t$

## Kalman Filter Objectives

> [!success] Three Main Goals
> 
> - **Filtering**: Best estimate of current state from observations up to now
> - **Smoothing**: Best estimate of past states given all observations
> - **Forecasting**: Prediction of future state values

## Kalman Filter Algorithm (1D Case)

### Notation

- $\mu_{t|t} = E[X_t | y_{1:t}]$ (filtered mean)
- $\mu_{t|t-1} = E[X_t | y_{1:t-1}]$ (predicted mean)
- $\sigma^2_{t|t} = \text{Var}[X_t | y_{1:t}]$ (filtered variance)
- $\sigma^2_{t|t-1} = \text{Var}[X_t | y_{1:t-1}]$ (predicted variance)

### Prediction Step

$$\mu_{t|t-1} = \mu_{t-1|t-1}$$ $$\sigma^2_{t|t-1} = \sigma^2_{t-1|t-1} + \sigma^2_W$$

### Update Step

$$\mu_{t|t} = \mu_{t|t-1} + \frac{\sigma^2_{t|t-1}}{\sigma^2_{t|t-1} + \sigma^2_V}(y_t - \mu_{t|t-1})$$

$$\sigma^2_{t|t} = \sigma^2_{t|t-1} - \frac{(\sigma^2_{t|t-1})^2}{\sigma^2_{t|t-1} + \sigma^2_V}$$

## General Kalman Filter (Multivariate)

### Prediction Step

$$\boldsymbol{\mu}_{t|t-1} = G_t \boldsymbol{\mu}_{t-1|t-1}$$ $$\Sigma_{t|t-1} = G_t \Sigma_{t-1|t-1} G_t^T + w_t$$

### Update Step

$$\boldsymbol{\mu}_{t|t} = \boldsymbol{\mu}_{t|t-1} + K_t(\mathbf{y}_t - F_t\boldsymbol{\mu}_{t|t-1})$$ $$\Sigma_{t|t} = \Sigma_{t|t-1} - K_t F_t \Sigma_{t|t-1}$$

where the **Kalman Gain** is: $$K_t = \Sigma_{t|t-1} F_t^T (F_t \Sigma_{t|t-1} F_t^T + v_t)^{-1}$$

## RTS Kalman Smoother

> [!info] Smoothing Objective Estimate hidden state $X_t$ given all observations $y_{1:T}$: $$f(X_t | y_{1:T}) = N(X_t; \mu_{t|T}, \Sigma_{t|T})$$

### Backward Recursion

For $t = T-1, \ldots, 0$:

$$J_t = \Sigma_{t|t} G_{t+1}^T \Sigma_{t+1|t}^{-1}$$ $$\mu_{t|T} = \mu_{t|t} + J_t(\mu_{t+1|T} - \mu_{t+1|t})$$ $$\Sigma_{t|T} = \Sigma_{t|t} + J_t(\Sigma_{t+1|T} - \Sigma_{t+1|t})J_t^T$$

## Example Applications

### AR(2) State Space Formulation

$$\begin{pmatrix} X_t \ X_{t-1} \end{pmatrix} = \begin{pmatrix} \alpha_1 & \alpha_2 \ 1 & 0 \end{pmatrix} \begin{pmatrix} X_{t-1} \ X_{t-2} \end{pmatrix} + \begin{pmatrix} 1 \ 0 \end{pmatrix} W_t$$

$$Y_t = (1 \quad 0) \begin{pmatrix} X_t \ X_{t-1} \end{pmatrix} + V_t$$

### Dynamic Linear Models

For time-varying regression: $S_t = L_t + \beta_t P_t + V_t$

**State vector**: $X_t = \begin{pmatrix} L_t \ \beta_t \end{pmatrix}$

**State equation**: $X_t = \begin{pmatrix} 1 & 0 \ 0 & 1 \end{pmatrix} X_{t-1} + \begin{pmatrix} \Delta L_t \ \Delta \beta_t \end{pmatrix}$

**Observation equation**: $Y_t = (1 \quad P_t) X_t + V_t$

## Parameter Estimation

> [!warning] Maximum Likelihood Estimation (MLE) Unknown parameters (e.g., $\sigma^2_W$, $\sigma^2_V$) can be estimated through MLE using the likelihood function derived from the Kalman filter innovations.

## Real-World Applications

> [!example] Common Applications
> 
> - **Speech Recognition**: Hidden word sequences, observed sound measurements
> - **Ion Channel Analysis**: Gate states (on/off), noisy current measurements
> - **Weather/Climate Modeling**: Atmospheric variables, satellite/sensor data
> - **Projectile Tracking**: Position/velocity/acceleration states, noisy position observations
> - **Financial Modeling**: Hidden market states, observed prices/returns