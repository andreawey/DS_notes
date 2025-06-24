# Spatiotemporal Graph Deep Learning Cheatsheet

## Overview

Spatiotemporal Graph Neural Networks (STGNNs) combine temporal and spatial processing for time series forecasting on graph-structured data.

**Applications:**

- Traffic monitoring
- Smart cities
- Energy analytics
- Physics simulations
- Stock markets

---

## 1. Spatiotemporal Time Series

### Core Components

- **N correlated time series** where each i-th series has:
    - $x_t^i \in \mathbb{R}^{d_x}$: observation vector at time t
    - $u_t^i \in \mathbb{R}^{d_u}$: exogenous variables at time t
    - $v^i \in \mathbb{R}^{d_v}$: static attributes (time-independent)

### Notation

- $X_t \in \mathbb{R}^{N \times d_x}$: stacked observations
- $X_{t:t+T} = [X_t, \ldots, X_{t+T-1}]$: time window
- $X_{<t} = [X_0, \ldots, X_{t-1}]$: past observations

### Relational Information

- **Adjacency matrix**: $A_t \in {0,1}^{N \times N}$ (can be dynamic)
- **Edge attributes**: $e_{ij}^t \in \mathbb{R}^{d_e}$
- **Edge set**: $E_t = {\langle(i,j), e_{ij}^t\rangle | \forall i,j: A_t[i,j] \neq 0}$
- **Spatiotemporal graph**: $G_t = \langle X_t, U_t, E_t, V \rangle$

---

## 2. Time Series Correlation

### Pearson Correlation

$$\hat{\rho} = \frac{\sum_{t=1}^T (X_t^a - \bar{X}^a)(X_t^b - \bar{X}^b)}{\sqrt{\sum_{t=1}^T (X_t^a - \bar{X}^a)^2 \sum_{t=1}^T (X_t^b - \bar{X}^b)^2}}$$

**Properties:**

- Normalized: $-1 \leq \rho \leq 1$
- Magnitude indicates strength, sign indicates direction
- Can use lagged version: $\hat{\rho}(\tau)$

### Granger Causality

Tests if $X^a$ "causes" $X^b$ by comparing models:

**Restricted model:** $$X_t^a = \alpha_0 + \sum_{i=1}^p \alpha_i X_{t-i}^a + \epsilon_t$$

**Unrestricted model:** $$X_t^a = \beta_0 + \sum_{i=1}^p \beta_i X_{t-i}^a + \sum_{j=1}^p \gamma_j X_{t-j}^b + \eta_t$$

**F-statistic:** $$F = \frac{(RSS_R - RSS_U)/p}{RSS_U/(T-2p-1)}$$

---

## 3. Forecasting Problem

### Multi-step Forecasting

Given window $X_{t-W:t}$, predict $H$ future steps: $$\hat{x}_{t+h}^i = F(G_{t-W:t}, U_{t:t+h}; \theta)$$

### Loss Function

$$\hat{\theta} = \arg\min_\theta \frac{1}{NT} \sum_{t=1}^T |X_{t:t+H} - \hat{X}_{t:t+H}|_2^2$$

---

## 4. Message Passing Framework

### Standard Message Passing

$$h_i^{l+1} = \text{UP}^l\left(h_i^l, \text{AGGR}_{j \in \mathcal{N}(i)} {\text{MSG}^l(h_i^l, h_j^l, e_{ji})}\right)$$

### Spatiotemporal Message Passing (STMP)

$$h_{i,t}^{l+1} = \text{UP}^l\left(h_{i,\leq t}^l, \text{AGGR}_{j \in \mathcal{N}_t(i)} {\text{MSG}^l(h_{i,\leq t}^l, h_{j,\leq t}^l, e_{ji,\leq t})}\right)$$

---

## 5. STGNN Architecture

### General Recipe

1. **Encoder**: $h_{i,t-1}^0 = \text{ENCODER}(x_{i,t-1}, u_{i,t-1}, v_i)$
2. **STMP layers**: $H_{t-1}^{l+1} = \text{STMP}^l(H_{\leq t-1}^l, E_{\leq t-1})$
3. **Decoder**: $\hat{x}_{i,t:t+H} = \text{DECODER}(h_{i,t-1}^L, u_{i,t:t+H})$

---

## 6. Design Paradigms

### Time-and-Space (T&S)

- **Characteristics**: Joint temporal and spatial processing
- **Examples**: Graph Convolutional RNNs (GCRNNs), Spatiotemporal CNNs

#### GCRNN Example (DCRNN)

Diffusion convolution: $$H'_t = \sum_{k=0}^K \left[(D_{t,out}^{-1}A_t)^k H_t \Theta_1^{(k)} + (D_{t,in}^{-1}A_t^T)^k H_t \Theta_2^{(k)}\right]$$

### Time-then-Space (TTS)

1. **Sequence encoding**: $h_{i,t}^1 = \text{SEQENC}(h_{i,\leq t}^0)$
2. **Spatial propagation**: $H_t^{l+1} = \text{MP}^l(H_t^l, E_t)$

**Pros**: ✓ Easy to implement, computationally efficient **Cons**: ✗ Information bottlenecks, dynamic topology issues

### Space-then-Time (STT)

1. **Spatial propagation**: $H_{i,t}^l = \text{MP}^l(H_{i,t}^{l-1}, E_t)$
2. **Sequence encoding**: $h_{i,t}^L = \text{SEQENC}(h_{i,t-W:t}^{L-1})$

---

## 7. Model Quality Assessment

### Residual Correlation Analysis

**Residuals**: $r_t^i = x_{t:t+H}^i - \hat{x}_{t:t+H}^i$

If residuals are dependent → model hasn't captured all information

### AZ-Whiteness Test

$$C({r}) = \sum_t \sum_{(i,j) \in E_t} w_{ijt} \text{sgn}(\langle r_t^i, r_t^j \rangle) + \sum_t \sum_i w_{it} \text{sgn}(\langle r_t^i, r_{t+1}^i \rangle)$$

**Properties**:

- Distribution-free
- Linear computation complexity
- Tests both spatial and temporal correlations

---

## 8. Missing Data Handling

### Time Series Imputation (TSI)

**Goal**: Reconstruct missing values using mask $m_t^i \in {0,1}$ $$x_t^i \sim p(x_t^i | X_{<T}, A) \quad \forall i,t \text{ s.t. } m_t^i = 0$$

### Graph Recurrent Imputation Network (GRIN)

1. **Encoding**: $Z_t = \text{STMP}(H_{<t} \odot M_{<t}, E_{<t})$
2. **Spatial decoding**: $\hat{x}_t^i = \text{DEC}(z_t^i, \text{AGGR}_{j \in \mathcal{N}(i)\setminus{i}} {\text{MSG}(z_t^j, x_t^j)})$

### Virtual Sensing

- Add nodes with no data
- Let model infer corresponding time series
- Requires high sensor homogeneity

---

## 9. Key Formulas Summary

|Concept|Formula|
|---|---|
|**Pearson Correlation**|$\hat{\rho} = \frac{\text{Cov}(X^a, X^b)}{\sigma_{X^a} \sigma_{X^b}}$|
|**Message Passing**|$h_i^{l+1} = \text{UP}^l(h_i^l, \text{AGGR}_{j \in \mathcal{N}(i)} {\text{MSG}^l(\cdot)})$|
|**Forecasting Loss**|$\mathcal{L} = \frac{1}{NT} \sum_{t=1}^T \|X_{t:t+H} - \hat{X}_{t:t+H}\|_2^2$|
|**STMP**|$h_{i,t}^{l+1} = \text{UP}^l(h_{i,\leq t}^l, \text{AGGR}_{j \in \mathcal{N}_t(i)} {\cdot})$|
|**AZ-Whiteness**|$C = \sum_t \sum_{(i,j) \in E_t} w_{ijt} \text{sgn}(\langle r_t^i, r_t^j \rangle) + \sum_t \sum_i w_{it} \text{sgn}(\langle r_t^i, r_{t+1}^i \rangle)$|

---

## 10. Implementation

### TSL Library

- **tsl** (Torch Spatiotemporal): PyTorch library for STGNN development
- Built on PyTorch and PyG
- Accelerates research on spatiotemporal data processing

