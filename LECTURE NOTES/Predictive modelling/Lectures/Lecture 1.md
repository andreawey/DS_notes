## Introduction to Regression Analysis

### Statistical Regression Analysis

Regression analysis is a statistical method to study and model relationships between a **response variable** and **predictor variables**.

#### Principal Goals:

- Predict data points for new values of predictor variables (**prediction**)
- Understand how the response variable is affected by changes in predictor variables (**inference**)

#### Mathematical Formalization:

> [!info] $$Y = f(X_1, X_2, ..., X_p) + \varepsilon$$
> 
> Where:
> 
> - $f$ is some fixed but unknown function of $X_1, X_2, ..., X_p$
> - $\varepsilon$ is a random error term which is:
>     - Independent of $X_1, X_2, ..., X_p$
>     - Has mean zero

- Error $\varepsilon$ is described using probability distribution
- $\varepsilon$ may contain unmeasured/unmeasurable variables
- Simplest assumption: $f$ is linear (though linear regression can fit non-linear curves too)

## Simple Linear Regression

### Linear Regression Model

We assume there is approximately a linear relationship between X and Y:

> [!note] Simple Linear Regression Model $$Y \approx \beta_0 + \beta_1X$$
> 
> Where:
> 
> - $\beta_0$ represents the intercept of the regression line
> - $\beta_1$ measures the slope of the regression line

Example: Advertising data to predict sales based on TV advertising budget: $$\text{sales} \approx \beta_0 + \beta_1 \cdot \text{TV}$$

### Estimating the Coefficients

Goal: Obtain coefficient estimates $\hat{\beta}_0$ and $\hat{\beta}_1$ such that the linear model fits available data as well as possible.

#### Least Squares Method:

- Let $\hat{y}_i = \hat{\beta}_0 + \hat{\beta}_1x_i$ be the prediction for Y based on ith value of X
- ith residual: $r_i = y_i - \hat{y}_i$ (difference between observed and predicted values)
- Least squares approach: choose $\hat{\beta}_0$ and $\hat{\beta}_1$ to minimize residual sum of squares (RSS)

> [!important] Residual Sum of Squares $$\text{RSS} = r_1^2 + r_2^2 + ... + r_n^2$$
> 
> or equivalently:
> 
> $$\text{RSS} = (y_1 - \hat{\beta}_0 - \hat{\beta}_1x_1)^2 + ... + (y_n - \hat{\beta}_0 - \hat{\beta}_1x_n)^2$$

#### Least Squares Coefficient Estimates:

> [!formula] Coefficient Formulas $$\hat{\beta}_1 = \frac{\sum_{i=1}^{n}(x_i - \bar{x})(y_i - \bar{y})}{\sum_{i=1}^{n}(x_i - \bar{x})^2}$$
> 
> $$\hat{\beta}_0 = \bar{y} - \hat{\beta}_1\bar{x}$$
> 
> Where: $$\bar{x} = \frac{1}{n}\sum_{i=1}^{n}x_i \text{ and } \bar{y} = \frac{1}{n}\sum_{i=1}^{n}y_i$$

### Standard Errors of Coefficient Estimates

> [!formula] Standard Errors $$\text{se}(\hat{\beta}_0)^2 = \sigma^2\left[\frac{1}{n} + \frac{\bar{x}^2}{\sum_{i=1}^{n}(x_i - \bar{x})^2}\right]$$
> 
> $$\text{se}(\hat{\beta}_1)^2 = \frac{\sigma^2}{\sum_{i=1}^{n}(x_i - \bar{x})^2}$$
> 
> Where $\sigma^2 = \text{Var}[\varepsilon]$

### Residuals and Variance Estimation

- Error term $\varepsilon$ cannot be directly observed
- Approximation for $\varepsilon$: residuals $r_i = y_i - (\hat{\beta}_0 + \hat{\beta}_1x_i)$
- Estimation of $\sigma$:

> [!formula] Residual Standard Error (RSE) $$\hat{\sigma} = \text{RSE} = \sqrt{\frac{\text{RSS}}{n-2}} = \sqrt{\frac{r_1^2 + r_2^2 + ... + r_n^2}{n-2}}$$
> 
> Factor $1/(n-2)$ ensures unbiased estimation

## Hypothesis Testing in Regression

### Testing for Relationship Between X and Y

Most common hypothesis test:

- $H_0$: There is no relationship between X and Y
- $H_A$: There is some relationship between X and Y

Mathematically:

- $H_0: \beta_1 = 0$
- $H_A: \beta_1 \neq 0$

If $\beta_1 = 0$, then model reduces to $Y = \beta_0 + \varepsilon$ and X is not associated with Y.

### Test Statistic and P-Value

> [!formula] t-Statistic $$T = \frac{\hat{\beta}_1 - 0}{\text{se}(\hat{\beta}_1)}$$
> 
> Measures the number of standard deviations that $\hat{\beta}_1$ is away from 0

- If $H_0$ is true, T follows a t-distribution with n-2 degrees of freedom
- p-value: probability of observing any value of T larger than |t|
- If p-value < $\alpha$ (typically 0.05), reject $H_0$ and conclude relationship exists
- If p-value > $\alpha$, retain $H_0$ (no relationship)

### Summary of t-Test in Linear Regression

1. Model: $Y = \beta_0 + \beta_1X + \varepsilon$, $\varepsilon \sim N(0, \sigma^2)$
2. Hypothesis: $H_0: \beta_1 = 0$ vs $H_A: \beta_1 \neq 0$ (two-sided test)
3. Test statistic: $T = \frac{\hat{\beta}_1 - 0}{\text{se}(\hat{\beta}_1)}$
4. Null Distribution: $T \sim t_{n-2}$
5. Significance Level: $\alpha$
6. Rejection Region: $K = (-\infty, t_{n-2; \frac{\alpha}{2}}] \cup [t_{n-2;1-\frac{\alpha}{2}}, \infty)$

## Confidence and Prediction Intervals

### Confidence Intervals for Regression Coefficients

> [!definition] Confidence Interval A 95% confidence interval is a range of values such that with 95% probability, the range will contain the true unknown parameter.

For linear regression, the 95% confidence interval for $\beta_1$ is approximately:

> [!formula] $$[\hat{\beta}_1 - 2 \cdot \text{se}(\hat{\beta}_1), \hat{\beta}_1 + 2 \cdot \text{se}(\hat{\beta}_1)]$$
> 
> Exact formula replaces factor 2 with $t_{0.975;n-2}$ (the 0.975 quantile of t-distribution with n-2 degrees of freedom)

### Confidence Interval for Response

For a given value $x_0$ of the predictor, the expected value of the predicted response is: $$E[\hat{y}|x_0] = E[\hat{y}_0] = E[\hat{\beta}_0 + \hat{\beta}_1 \cdot x_0] = \beta_0 + \beta_1x_0$$

> [!formula] Confidence Interval for Expected Response $$[\hat{y}_0 - 2 \cdot \text{se}(\hat{y}_0), \hat{y}_0 + 2 \cdot \text{se}(\hat{y}_0)]$$
> 
> Where: $$\text{se}(\hat{y}_0)^2 = \hat{\sigma}^2\left(\frac{1}{n} + \frac{(x_0 - \bar{x})^2}{\sum_{i=1}^{n}(x_i - \bar{x})^2}\right)$$

### Prediction Intervals

> [!important] Prediction intervals account for both the uncertainty in the mean value and the scatter of data around the regression line.

> [!formula] Prediction Interval $$[\hat{y}_0 - 2 \cdot \text{se}(y_0), \hat{y}_0 + 2 \cdot \text{se}(y_0)]$$
> 
> Where: $$\text{se}(y_0)^2 = \hat{\sigma}^2\left(1 + \frac{1}{n} + \frac{(x_0 - \bar{x})^2}{\sum_{i=1}^{n}(x_i - \bar{x})^2}\right)$$

| Use Case                                | Formula                                       |
| --------------------------------------- | --------------------------------------------- |
| Predicting **mean** income at `x₀`      | $$σ² * (1/n + (x₀ - x̄)² / Σ(xᵢ - x̄)²)$$     |
| Predicting **new income value** at `x₀` | $$σ² * (1 + 1/n + (x₀ - x̄)² / Σ(xᵢ - x̄)²)$$ |
|                                         |                                               |

> [!note] Prediction intervals are always wider than confidence intervals for the same predictor value.


$$ \sum_{i=1}^n (x_i - \bar{x})^2 = (n - 1)s_x^2 $$
where $s_x$ is the sample standard deviation of the predictor variable


## Examples

### Advertising Dataset

- Data: sales (in thousands of units) related to advertising budgets (TV, radio, newspaper in thousands of CHF)
- Goal: develop model to predict sales based on advertising budgets

### Cosmology (Edwin Hubble's 1929 Data)

- Linear relationship between galaxy distance and recession velocity supports Big Bang Theory
- Hubble constant relates to the age of the universe

### Medical Example: Small Round Blue Cell Tumors (SRBCTs)

- Challenge: Tumors with similar appearance require different treatments
- Solution: Predictive model using gene microarray data to classify tumor types