# Time Series Analysis - Key Concepts and Formulas

## What is Time Series Data?

> [!info] Definition Time series data consists of observations that are serially correlated, meaning observations are not independent and show dependency over time.

### Common Examples

- **Machine monitoring**: Temperature, pressure, acoustic emissions, vibrations
- **Financial data**: Stock prices, exchange rates
- **Environmental observations**: Temperature, humidity, pollution levels
- **Federal statistics**: Population census, income data, accident reports

## Goals of Time Series Analysis

> [!note] Primary Objectives
> 
> 1. **Descriptive Analysis**: Understanding basic properties through statistics and visualizations
> 2. **Modeling and Interpretation**: Understanding underlying processes and quantifying dependencies
> 3. **Decomposition**: Separating trend, seasonality, and irregular components
> 4. **Prediction/Forecasting**: Predicting future values
> 5. **Regression**: Explaining one time series using others (virtual sensoring)

## Key Patterns in Time Series

> [!important] Three Main Patterns
> 
> - **Trend**: Gradually changing average directly correlated with time
> - **Seasonality**: Periodic patterns in the data
> - **Serial Correlation**: Observations are not independent

## Time Series Transformations

### Box-Cox Transformations

> [!formula] Box-Cox Transform For a time series ${x_1, x_2, \ldots}$ with positive values:
> 
> $$g(x) = \begin{cases} \frac{x^\lambda - 1}{\lambda} & \text{if } \lambda \neq 0 \ \log(x) & \text{if } \lambda = 0 \end{cases}$$
> 
> **Purpose**: Correct skewness and stabilize variance

### Time-Shift Transformations

> [!formula] Time-Shift and Backshift 
> **Time-shift by lag k:** $$g(x_i) = x_{i-k}$$
> 
> **Backshift operator (k=1):** $$B(x_i) = x_{i-1}$$
> 
> Used for computing differences: $x_i - x_{i-1} = x_i - B(x_i)$

### Log-Returns

> [!formula] Log-Returns Formula 
> $$y_i = \log(x_i) - \log(x_{i-1}) = \log\left(\frac{x_i}{x_{i-1}}\right)$$
> 
> **Approximation using Taylor series:** $$y_i = \log\left(\frac{x_i - x_{i-1}}{x_{i-1}} + 1\right) \approx \frac{x_i - x_{i-1}}{x_{i-1}}$$
> 
> **Purpose**: Approximates relative increase; often appears random even when original series has patterns

## Time Series Decomposition

### Additive Model

> [!formula] Additive Decomposition 
> $$x_k = m_k + s_k + z_k$$
> 
> Where:
> 
> - $x_k$ = observed time series
> - $m_k$ = trend component
> - $s_k$ = seasonal component
> - $z_k$ = error/remainder term (correlated random variables with mean zero)

### Multiplicative Model

> [!formula] Multiplicative Decomposition 
> $$x_k = m_k \cdot s_k + z_k$$
> 
> **Fully multiplicative (with multiplicative noise):** $$x_k = m_k \cdot s_k \cdot z_k$$
> 
> **Logarithmic transformation makes it additive:** $$\log(x_k) = \log(m_k) + \log(s_k) + \log(z_k)$$

## Moving Average Filter

> [!formula] Moving Average Filter 
> **For odd window width** $p = 2l + 1$: $$g(x_i) = \frac{1}{p}(x_{i-l} + \cdots + x_i + \cdots + x_{i+l})$$
> 
> **For even window width** $p = 2l$: $$g(x_i) = \frac{1}{p}\left(\frac{1}{2}x_{i-l} + x_{i-l+1} + \cdots + x_i + \cdots + x_{i+l-1} + \frac{1}{2}x_{i+l}\right)$$
> 
> **Purpose**: Estimate trend component by averaging over one complete period

## Seasonal Effect Estimation

> [!formula] Seasonal Effect Calculation 
> **Step 1 - Estimate seasonal effect:** $$\hat{s}_k = x_k - \hat{m}_k$$
> 
> **Step 2 - Estimate remainder:** $$\hat{r}_i = x_i - \hat{m}_i - \hat{s}_i$$

> [!tip] Process
> 
> 1. Apply moving average filter to estimate trend $\hat{m}_k$
> 2. Subtract trend to get seasonal component
> 3. Average seasonal component for each cycle position
> 4. Calculate remainder by subtracting both trend and seasonal estimates

## Advanced Methods

### STL Decomposition (Seasonal and Trend decomposition using Loess)

> [!info] STL Advantages **Improvements over basic decomposition:**
> 
> - **Robust to outliers**: Uses iterative reweighting
> - **Flexible seasonal component**: Can change over time
> - **Better smoothing**: Uses loess regression instead of moving averages
> - **Cycle-subseries approach**: Treats each seasonal position separately

> [!note] Loess Regression Local regression method that computes regression line at point $x$ using only observations in a neighborhood of $x$. Provides more flexibility than simple moving averages.

## Important Considerations

> [!warning] Assumptions for Many Methods Time series methods often assume:
> 
> - Gaussian or symmetric distribution
> - Linear trend relationship
> - Constant variance across time (homoscedasticity)

> [!example] Transformation Use Cases
> 
> - **Highly skewed data**: Apply Box-Cox transformation
> - **Heteroscedastic data**: Use transformed series ${g(x_1), g(x_2), \ldots}$ instead of original
> - **Financial analysis**: Use log-returns instead of original prices
> - **Seasonal data**: Apply appropriate window size for moving average (e.g., 12 for monthly data)

## Key Takeaways

> [!success] Remember
> 
> - Time series analysis starts with **exploration and visualization**
> - **Transformations** may be necessary before modeling
> - **Decomposition** helps understand underlying components
> - **STL is preferred** over basic decomposition methods
> - **Log-returns** often appear random even when original series shows patterns

