# Graph Anomaly and Change Detection Cheatsheet

## 1. Problem Formulation

### Attributed Graphs

- **Definition**: $g = (V, E, \alpha) \in \mathcal{G}$
    - $V$: finite vertex set
    - $E \subseteq V \times V$: edge set
    - $\alpha : V \cup E \to \mathcal{A}$: attribute function

### Stationarity Assumption

- **Nominal regime**: Process $P$ generates i.i.d. graphs $g_t \sim P_0$ for all $t \in \mathbb{N}$
- **Goal**: Identify whether stationarity changed

### Change Types

#### Abrupt Change in Stationarity

$$g_t \sim \begin{cases} P_0, & t < t^* \ P^* \neq P_0, & t \geq t^* \end{cases}$$

#### Anomalous Observation

$$g_t \sim \begin{cases} P_0, & t \neq t^* \ P^* \neq P_0, & t = t^* \end{cases}$$

---

## 2. Graph-Level Embeddings

### 3-Step Methodology

1. **Embed**: $g_t \in \mathcal{G} \to \phi(g_t) = x_t \in \mathcal{X}$
2. **Analyze**: Statistical analysis on sequence $x_1, x_2, \ldots$
3. **Relate back**: Map conclusions to graph domain

### Distance-Kernel-Embedding Relationship

- **Distance to Kernel**: $\kappa(g_1, g_2) = \frac{1}{2}[d(g_1, g_{ref})^2 + d(g_{ref}, g_2)^2 - d(g_1, g_2)^2]$
- **Embedding**: $\kappa(g_1, g_2) = \langle \phi(g_1), \phi(g_2) \rangle_{\mathcal{H}}$

### Desirable Properties

- **Consistency**: Similar graphs → similar embeddings
- **Discernibility**: Different graphs → different embeddings

---

## 3. Graph Random Neural Features (GRNF)

### Definition

Consider parametric family $\mathcal{F}(W) = {\psi(\cdot; \vec{w}) : \mathcal{G} \to \mathbb{R} | \vec{w} \in W}$

**Key Properties**:

- $\psi$ continuous in $\vec{w}$
- $g_i \neq g_j \Rightarrow \exists \vec{w}^_: \psi(g_i; \vec{w}^_) \neq \psi(g_j; \vec{w}^*)$

### Distance Formula

$$d_P(g_i, g_j) = \left(\mathbb{E}_{\vec{w} \sim P}[|\psi(g_i; \vec{w}) - \psi(g_j; \vec{w})|^2]\right)^{1/2}$$

### GRNF Embedding

$$\phi(g) = \begin{pmatrix} \alpha_1 \cdot \psi(g; \vec{w}_1) \ \vdots \ \alpha_M \cdot \psi(g; \vec{w}_M) \end{pmatrix}$$

#### Training Strategies

- **Training-free**: $\alpha_1, \ldots, \alpha_M = M^{-1/2}$, sample $\vec{w}_1, \ldots, \vec{w}_M \sim P$
- **Learnable**: Tune weights $\alpha_1, \ldots, \alpha_M$ or all parameters

### Quality-Computation Trade-off

$$\mathbb{E}[|\phi(g_i) - \phi(g_j)|_2^2] = d_P(g_i, g_j)^2$$

**Bound**: For $\varepsilon > 0, \delta \in (0,1)$, if $M \geq \frac{16}{\delta \varepsilon^2}$: $$\Pr(|d_P(g_i, g_j)^2 - |\phi(g_i) - \phi(g_j)|_2^2| \geq \varepsilon) < \delta$$

---

## 4. Change Detection Methods

### Sequential Change Detection (CUSUM)

**Local statistic**: $s(t) = |x_t - \mu|$ **Cumulative sum**: $S(t) = \max{0, S(t-1) + s(t) - q}$ **Detection**: Change detected if $S(t) > \gamma(t)$ where $P(S(t) > \gamma(t) | H_0) \leq \alpha$

### Offline Change Point Method

For every $t$, split sequence: ${x_1, \ldots, x_{t-1}} | {x_t, \ldots, x_T}$

#### Maximum Mean Discrepancy (MMD)

$$S(t) = \frac{\sum_{i \neq j=1}^{t-1} k(g_i, g_j)}{(t-1)(t-2)} + \frac{\sum_{i \neq j=t}^T k(g_i, g_j)}{(T-t)(T-t-1)} - 2\frac{\sum_{i=1}^{t-1}\sum_{j=t}^T k(g_i, g_j)}{(t-1)(T-t)}$$

#### Energy Distance (with GRNF)

$$S(t) = \frac{(t-1)(T-t-1)}{T}\left[\frac{\sum_{i \neq j=1}^{t-1} |\phi(g_i) - \phi(g_j)|^2}{(t-1)(t-2)} + \frac{\sum_{i \neq j=t}^T |\phi(g_i) - \phi(g_j)|^2}{(T-t)(T-t-1)} - 2\frac{\sum_{i=1}^{t-1}\sum_{j=t}^T |\phi(g_i) - \phi(g_j)|^2}{(t-1)(T-t)}\right]$$

### Hypotheses Testing

- **$H_0$**: All graphs independently drawn from $P_0$
- **$H_1$**: $\exists t^* \in \mathbb{N}$ s.t. $g_t \sim P_0$ for $t < t^_$ and $g_t \sim P_t$ for $t \geq t^_$ with $P_{t^*} \neq P_0$

---

## 5. Anomaly Detection

### Problem Setup

Given graph data ${g_1, g_2, \ldots, g_N} \subseteq \mathcal{G}$

- **Assumption**: Majority $g_i \sim p_0$
- **Goal**: Find anomalies ($g^* \sim p^* \neq p_0$) or outliers ($p_0(g^*) \approx 0$)

### Anomaly Score

Define: $s_\theta(g): \mathcal{G} \to \mathbb{R}^+$ $$\begin{cases} s_\theta(g) \leq \gamma & \text{consider } g \text{ nominal} \ s_\theta(g) > \gamma & \text{consider } g \text{ anomalous} \end{cases}$$

---

## 6. Linking Graph and Embedding Domains

### Key Questions

1. Does change observed in $\mathcal{X}$ correspond to change in $\mathcal{G}$?
2. Are all changes in $\mathcal{G}$ detectable in $\mathcal{X}$?

### Deterministic Bound

If $\frac{1}{c}d(g_i, g_j) \leq |x_i - x_j|_2 \leq c \cdot d(g_i, g_j)$, then: $$q \cdot \text{cdf}_e\left(\frac{\gamma}{c} - b\right) \leq \text{cdf}_g(\gamma) \leq \frac{1}{q} \cdot \text{cdf}_e(\gamma \cdot c + b)$$

### Probabilistic Bound

If $\Pr(|s_e(t) - s_g(t)| \leq \lambda) \geq q$, then: $$q \cdot \text{cdf}_e(\gamma - \lambda) \leq \text{cdf}_g(\gamma) \leq \frac{1}{q} \cdot \text{cdf}_e(\gamma + \lambda)$$

---

## 7. Model Optimality Assessment

### AZ-Whiteness Test

#### Residual Analysis

**Residuals**: $r_t^i = x_{t:t+H}^i - \hat{x}_{t:t+H}^i$

**Key insight**: If residuals are dependent → model hasn't captured all information → predictions can be improved

#### Test Statistic

$$C({r}) = \sum_t \sum_{(i,j) \in E_t} w_{ijt} \text{sgn}(\langle r_t^i, r_t^j \rangle) + \sum_t \sum_i w_{it} \text{sgn}(\langle r_t^i, r_{t+1}^i \rangle)$$

Where:

- First term: **spatial edges**
- Second term: **temporal edges**
- $\text{sgn}(a) = \begin{cases} +1 & a > 0 \ 0 & a = 0 \ -1 & a < 0 \end{cases}$

**Distribution**: $C({r}) \to \mathcal{N}(0, 1)$

#### Properties

- Distribution-free
- Residuals can be non-identically distributed
- Linear computation in edges and time steps

### Subgraph Analysis

Analyze AZ-whiteness on subgraphs to discover correlation patterns:

- Spatial/temporal edges only
- Edges related to specific nodes
- Edges related to specific time steps
- Node-time combinations

---

## 8. Key Formulas Summary

|**Concept**|**Formula**|
|---|---|
|**GRNF Distance**|$d_P(g_i, g_j) = \left(\mathbb{E}_{\vec{w} \sim P}[|
|**CUSUM Statistic**|$S(t) = \max{0, S(t-1) + s(t) - q}$|
|**MMD Test**|$S(t) = \frac{\sum k(g_i, g_j) \text{ within groups}}{n(n-1)} - 2\frac{\sum k(g_i, g_j) \text{ between groups}}{n_1 n_2}$|
|**Energy Distance**|$S(t) = \frac{(t-1)(T-t-1)}{T}[\text{within-group distances} - 2 \times \text{between-group distances}]$|
|**AZ-Whiteness**|$C({r}) = \sum \text{spatial correlations} + \sum \text{temporal correlations}$|
|**Fréchet Mean**|$\arg\min_{g \in \mathcal{G}} \sum_{i=1}^n d(g, g_i)^2$|

---

## 9. Applications

### Traffic Forecasting Example

- **Models tested**: uvRNN, mvRNN, ttgRNN, GWNet, DCRNN, AGCRN
- **Analysis**: Spatio-temporal correlation patterns in prediction residuals
- **Insights**: Identify where and when models fail to capture dependencies

### Performance Metrics

- Traditional: MAE, MSE comparison between models
- **AZ-test**: Model optimality assessment independent of specific metrics
- **Localized analysis**: Node-specific and time-specific performance evaluation

---

## Key References

- Zambon et al. IEEE TNNLS 2018 (Change detection)
- Zambon et al. ICML 2020 (GRNF)
- Zambon et al. IEEE TSP 2019 (Energy distance)
- Zambon & Alippi NeurIPS 2022 (AZ-whiteness test)