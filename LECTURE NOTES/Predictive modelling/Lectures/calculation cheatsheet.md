# 📘 Time Series: Autocovariance & Stationarity Toolbox

## 🧾 Assumptions

### Stationarity (Weak/Second-Order)

- Constant mean:  
  $$ \mathbb{E}[X_n] = \mu $$
- Constant variance:  
  $$ \text{Var}(X_n) = \sigma_X^2 $$
- Covariance depends only on lag:  
  $$ \text{Cov}(X_n, X_{n+k}) = \gamma(k) $$

---

## 📐 Definitions

### Autocovariance Function

$$
\gamma(k) = \text{Cov}(X_n, X_{n+k})
$$

### Variance

$$
\gamma(0) = \text{Var}(X_n) = \sigma_X^2
$$

---

## 🧮 Common Models and Formulas

### 🔹 AR(1) Process

**Model**:  
$$
X_n = \phi X_{n-1} + \epsilon_n
$$

**Assumptions**:  
- $\epsilon_n \sim \text{i.i.d.}(0, \sigma_\epsilon^2)$  
- $|\phi| < 1$ for stationarity

**Variance**:  
$$
\sigma_X^2 = \frac{\sigma_\epsilon^2}{1 - \phi^2}
$$

**Autocovariance**:  
$$
\gamma(k) = \phi^k \sigma_X^2
$$

**Lag-1 Autocovariance**:  
$$
\gamma(1) = \phi \sigma_X^2
$$

---

### 🔹 AR(2) Process

**Model**:  
$$
X_n = a_1 X_{n-1} + a_2 X_{n-2} + \epsilon_n
$$

**Lag-1 Autocovariance Identity**:
$$
\gamma(1) = a_1 \sigma_X^2 + a_2 \gamma(1)
$$

**Solved**:
$$
\gamma(1) = \frac{a_1}{1 - a_2} \sigma_X^2
$$

---

### 🔹 Linear Process (MA(∞)-type)

**Model**:
$$
X_n = \sum_{i=0}^{\infty} \psi_i \epsilon_{n-i}
$$

**Assumptions**:
-  $\epsilon_n \sim \text{i.i.d.}(0, \sigma_\epsilon^2)$
- $\sum \psi_i^2 < \infty$

**Variance**:
$$
\sigma_X^2 = \sigma_\epsilon^2 \sum_{i=0}^\infty \psi_i^2
$$

**Autocovariance**:
$$
\gamma(k) = \sigma_\epsilon^2 \sum_{i=0}^\infty \psi_i \psi_{i+k}
$$

---

## 🧠 Useful Properties

- **Symmetry**:  
  $$ \gamma(-k) = \gamma(k) $$
- **Linearity of Covariance**:  
  $$ \text{Cov}(X, aY + bZ) = a \cdot \text{Cov}(X, Y) + b \cdot \text{Cov}(X, Z) $$
- **Independence of noise**:  
  $$ \text{Cov}(X_n, W_{n+k}) = 0 \quad \text{if } W_{n+k} \perp X_n $$

---

## 🧩 Solving Lag-1 Autocovariance for AR(2)-like Models

### General Model

$$
X_n = a_1 X_{n-1} + a_2 X_{n-2} + W_n
$$

**Assumptions**:
- $W_n$ is white noise:
  - $\mathbb{E}[W_n] = 0$
  - $\text{Var}(W_n) = \sigma_W^2$
  - $\text{Cov}(X_k, W_n) = 0$ for $k \leq n-1$
- $X_n$ is stationary

---

### Derivation of \( \gamma(1) \)

Given:

$$
X_{n+1} = a_1 X_n + a_2 X_{n-1} + W_{n+1}
$$

Then:

$$
\begin{aligned}
\gamma(1) &= \text{Cov}(X_n, X_{n+1}) \\
&= \text{Cov}(X_n, a_1 X_n + a_2 X_{n-1} + W_{n+1}) \\
&= a_1 \text{Cov}(X_n, X_n) + a_2 \text{Cov}(X_n, X_{n-1}) \\
&= a_1 \sigma_X^2 + a_2 \gamma(1)
\end{aligned}
$$

Solve:

$$
(1 - a_2)\gamma(1) = a_1 \sigma_X^2 \\
\Rightarrow \gamma(1) = \frac{a_1}{1 - a_2} \sigma_X^2
$$

---

### ✅ Example

Given:

$$
X_n = \frac{1}{2} X_{n-1} + \frac{1}{3} X_{n-2} + W_n
$$

Then:

- \( a_1 = \frac{1}{2} \)
- \( a_2 = \frac{1}{3} \)

Compute:

$$
\gamma(1) = \frac{\frac{1}{2}}{1 - \frac{1}{3}} \sigma_X^2 = \frac{\frac{1}{2}}{\frac{2}{3}} \sigma_X^2 = \frac{3}{4} \sigma_X^2
$$

✔️ Done.

---

## 🧰 Quick Lookup Table

| Property                       | Formula                                                             |
| ------------------------------ | ------------------------------------------------------------------- |
| Variance                       | $\gamma(0) = \sigma_X^2$                                            |
| AR(1) Lag-1                    | $\gamma(1) = \phi \sigma_X^2$                                       |
| AR(2) Lag-1                    | $\gamma(1) = \frac{a_1}{1 - a_2} \sigma_X^2$                        |
| Linear Process \( \gamma(k) \) | $\gamma(k) = \sigma_\epsilon^2 \sum_{i=0}^\infty \psi_i \psi_{i+k}$ |
