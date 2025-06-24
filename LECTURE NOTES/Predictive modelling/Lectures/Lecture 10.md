# Spectral Analysis - Key Concepts and Formulas

## Fourier Coefficients

> [!definition] Periodic Function A function $f$ with period $T$ satisfies: $$f(x + T) = f(x) \text{ for all } x \in \mathbb{R}$$ This implies: $f(x + kT) = f(x)$ for all $k \in \mathbb{Z}$

### Fourier Series Decomposition

> [!important] Fourier Series A periodic function $f$ can be decomposed as: $$f(t) = \frac{a_0}{2} + \sum_{k=1}^{+\infty} \left(a_k \cos(k\omega t) + b_k \sin(k\omega t)\right)$$ where $\omega := \frac{2\pi}{T}$ is the angular frequency.

### Fourier Coefficients Formula

> [!formula] Fourier Coefficients $$a_0 = \frac{2}{T} \int_0^T f(t) , dt$$
> 
> $$a_k = \frac{2}{T} \int_0^T f(t) \cos(k\omega t) , dt \quad \text{for } k = 1,2,3,\ldots$$
> 
> $$b_k = \frac{2}{T} \int_0^T f(t) \sin(k\omega t) , dt \quad \text{for } k = 1,2,3,\ldots$$

### Dirichlet Conditions

> [!theorem] Dirichlet-Jordan Test (Simplified) Let $f$ be a bounded $T$-periodic function with:
> 
> 1. A finite number of local maxima and minima in its period
> 2. A finite number of points of discontinuity
> 
> Then: $$S_f(t) = \begin{cases} f(t) & \text{if } f \text{ is continuous at } t \ \frac{\lim_{\varepsilon \to 0} f(t+\varepsilon) + f(t-\varepsilon)}{2} & \text{if } f \text{ is discontinuous at } t \end{cases}$$

## Complex Form and Exponential Representation

### Euler's Formula

> [!formula] Euler's Formula $$e^{i\alpha} = \cos(\alpha) + i\sin(\alpha)$$
> 
> This gives us: $$\cos(k\omega t) = \frac{e^{ik\omega t} + e^{-ik\omega t}}{2}$$ $$\sin(k\omega t) = \frac{e^{ik\omega t} - e^{-ik\omega t}}{2i}$$

### Complex Fourier Coefficients

> [!definition] Complex Fourier Coefficients 
> $$c_0 := \frac{a_0}{2}$$ 
> $$c_k := \frac{1}{2}(a_k - ib_k) \quad \text{for } k \in \mathbb{N}^_$$ 
> $$c_{-k} := \frac{1}{2}(a_k + ib_k) \quad \text{for } k \in \mathbb{N}^_$$

> [!formula] Exponential Form of Fourier Series $$S_f(t) = \sum_{k=-\infty}^{+\infty} c_k e^{ik\omega t}$$

### Properties of Complex Coefficients

> [!note] Key Properties
> 
> - $c_{-k} = \overline{c_k}$ (complex conjugates)
> - $c_k = \frac{1}{T} \int_0^T f(t) e^{-ik\omega t} , dt$ for $k \in \mathbb{Z}$
> - $a_0 = 2c_0$, $a_k = 2\text{Re}(c_k)$, $b_k = -2\text{Im}(c_k)$
> - $c_0$ represents the mean value: $c_0 = \frac{1}{T} \int_0^T f(t) , dt$

### Amplitude Spectrum

> [!important] Amplitude Analysis For $k > 0$, the amplitudes are: $$\sqrt{a_k^2 + b_k^2} = 2|c_k|$$
> 
> These indicate which frequency components (harmonics) are large or small in magnitude.

## Discrete Fourier Transform (DFT)

### Periodic Sequences

> [!definition] Sampling Setup Consider a $T$-periodic function $f$ sampled at points: $$t_j = j \cdot \frac{T}{n} \quad \text{for } j = 0,1,2,\ldots,n-1$$ with values $x_j := f(t_j)$

### Trigonometric Interpolation

> [!formula] Trigonometric Polynomial For odd $n$, interpolate with: $$p(t) = \sum_{k=-\frac{n-1}{2}}^{\frac{n-1}{2}} c_k e^{ik\frac{2\pi}{T}t}$$ such that $p(t_j) = x_j$

### DFT Formula

> [!formula] Discrete Fourier Transform $$c_k = \frac{1}{n} \sum_{j=0}^{n-1} x_j e^{ik\frac{2\pi}{n}j}$$ for $k = -\frac{n-1}{2}, \ldots, 0, \ldots, \frac{n-1}{2}$

## Spectral Analysis of Stochastic Processes

### Spectral Density

> [!definition] Spectral Density If the autocovariance function $\gamma(h)$ of a stationary process satisfies: $$\sum_{h=-\infty}^{+\infty} |\gamma(h)| < \infty$$
> 
> Then it has the representation: $$\gamma(h) = \int_{-\frac{1}{2}}^{\frac{1}{2}} e^{i2\pi sh} f(s) , ds$$
> 
> where the spectral density is: $$f(s) = \sum_{h=-\infty}^{+\infty} \gamma(h) e^{-i2\pi sh} \quad \text{for } s \in \left[-\frac{1}{2}, \frac{1}{2}\right]$$

### Spectral Densities for Common Processes

#### White Noise

> [!formula] White Noise Spectral Density For white noise with $\gamma(h) = \begin{cases} \sigma^2 & h = 0 \ 0 & h \neq 0 \end{cases}$: $$f(s) = \sigma^2$$

#### ARMA Process

> [!formula] ARMA Spectral Density For ARMA(p,q): $\Phi(B)X_n = \Theta(B)W_n$ $$f(s) = \sigma^2 \frac{|\Theta(e^{-i2\pi s})|^2}{|\Phi(e^{-i2\pi s})|^2}$$ where:
> 
> - $\Phi(z) = 1 - \sum_{j=1}^p \phi_j z^j$
> - $\Theta(z) = 1 + \sum_{j=1}^q \vartheta_j z^j$

#### MA(1) Process

> [!formula] MA(1) Spectral Density For $X_n = W_n + \vartheta_1 W_{n-1}$: $$f(s) = \sigma^2(1 + 2\vartheta_1 \cos(2\pi s) + \vartheta_1^2)$$

#### AR(2) Process

> [!formula] AR(2) Spectral Density For $X_n = \phi_1 X_{n-1} + \phi_2 X_{n-2} + W_n$: $$f(s) = \frac{\sigma^2}{1 - 2\phi_1(1-\phi_2)\cos(2\pi s) - 2\phi_2\cos(4\pi s) + \phi_1^2 + \phi_2^2}$$

#### ARMA(2,1) Process

> [!formula] ARMA(2,1) Spectral Density For $X_n = \phi_1 X_{n-1} + \phi_2 X_{n-2} + W_n + \vartheta_1 W_{n-1}$: $$f(s) = \sigma^2 \frac{1 + 2\vartheta_1\cos(2\pi s) + \vartheta_1^2}{1 - 2\phi_1(1-\phi_2)\cos(2\pi s) - 2\phi_2\cos(4\pi s) + \phi_1^2 + \phi_2^2}$$

---

> [!tip] Key Insight **Autocovariance vs Spectral Density:**
> 
> - **Autocovariance function**: Information in terms of lags
> - **Spectral density**: Same information in terms of cycles/frequencies
> 
> Both contain equivalent information but expressed in different domains.