# Predictive Modeling: Testing Model Assumptions - Residual Analysis

## Simple Linear Regression Review

### Model Definition

- Simple Linear Regression Model: $Y = \beta_0 + \beta_1 X + \varepsilon$, where $\varepsilon \sim N(0, \sigma^2)$
- $Y$: response variable
- $X$: predictor variable
- $\varepsilon$: error term

### Statistical Measures

- 95% confidence interval for $\beta_1$: $[\hat{\beta}_1 - 2 \cdot se(\hat{\beta}_1), \hat{\beta}_1 + 2 \cdot se(\hat{\beta}_1)]$
- Standard error: $se(\hat{\beta}_1)^2 = \frac{\sigma^2}{\sum_{i=1}^{n} (x_i - \bar{x})^2}$
- Residuals: $r_i = y_i - (\hat{\beta}_0 + \hat{\beta}_1 x_i)$
- Residual Standard Error (RSE): $RSE = \sqrt{\frac{RSS}{n-2}} = \sqrt{\frac{r_1^2 + r_2^2 + ... + r_n^2}{n-2}}$
- $\hat{\sigma} = RSE$

### Confidence and Prediction Intervals

- 95% confidence interval for expected value of $\hat{y} = \hat{\beta}_0 + \hat{\beta}_1 \cdot x$ for given value $x_0$: $[\hat{y}_0 - 2 \cdot se(\hat{y}_0), \hat{y}_0 + 2 \cdot se(\hat{y}_0)]$ where $se(\hat{y}_0)^2 = \hat{\sigma}^2 \left[ \frac{1}{n} + \frac{(x_0 - \bar{x})^2}{\sum_{i=1}^{n} (x_i - \bar{x})^2} \right]$
    
- 95% prediction interval for future observation $y_0$ at given value $x_0$: $[\hat{y}_0 - 2 \cdot se(y_0), \hat{y}_0 + 2 \cdot se(y_0)]$ where $se(y_0)^2 = \hat{\sigma}^2 \left[ 1 + \frac{1}{n} + \frac{(x_0 - \bar{x})^2}{\sum_{i=1}^{n} (x_i - \bar{x})^2} \right]$
    

> [!note] These confidence and prediction intervals rely on the assumption $\varepsilon \sim N(0, \sigma^2)$

## Model Assumptions for Error Terms $\varepsilon_i$

All test and estimation methods rely on the following assumptions:

1. The expected value of all $\varepsilon_i$ is zero: $E[\varepsilon_i] = 0$
2. The error terms $\varepsilon_i$ all have the same constant variance: $Var[\varepsilon_i] = \sigma^2$
3. The error terms $\varepsilon_i$ are normally distributed
4. The error terms $\varepsilon_i$ are independent

## Residual Analysis

Since the error term $\varepsilon_i = y_i - (\beta_0 + \beta_1 x_i)$ is unknown, we use residuals $r_i = y_i - (\hat{\beta}_0 + \hat{\beta}_1 x_i)$ to evaluate model assumptions.

> [!important] If model assumptions are violated, we should use this as an opportunity to adapt and extend our regression model to find a better fit.

### Measures of Model Fit

#### Residual Standard Error (RSE)

- Estimates the standard deviation of $\varepsilon$
- Average amount the response will deviate from the true regression line
- $RSE = \sqrt{\frac{r_1^2 + ... + r_n^2}{n-2}} = \sqrt{\frac{(y_1 - \hat{y}_1)^2 + ... + (y_n - \hat{y}_n)^2}{n-2}}$

#### R² Statistic

- Provides alternative measure of fit
- $R^2 = 1 - \frac{\sum_{i=1}^{n} (y_i - \hat{y}_i)^2}{\sum_{i=1}^{n} (y_i - \bar{y})^2} = 1 - \frac{\text{variance left after regression fit}}{\text{total variance}}$
- Proportion of variance explained by the model (values between 0 and 1)

#### Correlation Coefficient

- $r = Cor[X, Y] = \frac{\sum_{i=1}^{n} (x_i - \bar{x})(y_i - \bar{y})}{\sqrt{\sum_{i=1}^{n} (x_i - \bar{x})^2} \sqrt{\sum_{i=1}^{n} (y_i - \bar{y})^2}}$
- In simple linear regression: $r^2 = R^2$

## Diagnostic Tools for Model Assumptions

### Testing $E[\varepsilon] = 0$ (Linearity)

#### Tukey-Anscombe Plot

- Plot residuals $r_i = y_i - \hat{y}_i$ vs. fitted values $\hat{y}_i$
- If model is appropriate, points should scatter evenly around $r = 0$ line
- LOESS smoother can help visualize patterns

> [!tip] To determine if deviations are systematic or due to random variation, simulate multiple smoothing curves through resampling and create a band of plausible curves. If the original smoothing curve falls outside this band, there's evidence of systematic deviation from linearity.

### Testing $Var[\varepsilon_i] = constant$ (Homoscedasticity)

#### Scale-Location Plot

- Plot $\sqrt{|\tilde{r}_i|}$ vs. fitted values $\hat{y}_i$
- Where $\tilde{r}_i$ are standardized residuals: $\tilde{r}_i = \frac{r_i}{\hat{\sigma}\sqrt{1-\left(\frac{1}{n} + \frac{(x_i-\bar{x})^2}{\sum_i (x_i-\bar{x})^2}\right)}}$
- If error terms have constant variance, plot should show random scatter

### Testing Normal Distribution of Errors

- Use Normal Q-Q plot of standardized residuals
- Points should fall along the diagonal line if residuals are normally distributed

### Testing Independence of Errors

- For time-ordered observations:
    - Plot residuals as function of time
    - Create scatter plot of residuals $r_{t+1}$ vs. $r_t$
- Patterns in these plots suggest correlation among error terms

## Outliers and Influential Observations

### Outliers

- Points where $y_i$ is far from predicted value $\hat{y}_i$
- Have relatively small effect on regression coefficients in large datasets
- Can significantly affect RSE and $R^2$

### High Leverage Points

- Points with unusual values for $x_i$
- Have substantial impact on regression line
- Leverage statistic: $h_i = \frac{1}{n} + \frac{(x_i - \bar{x})^2}{\sum_{i'=1}^{n} (x_{i'} - \bar{x})^2}$
- Properties:
    - Increases with distance of $x_i$ from $\bar{x}$
    - Always between $1/n$ and 1
    - Average leverage for all observations equals $2/n$ (simple linear regression)
    - Points with $h_i$ greatly exceeding $2/n$ are suspected high leverage points

### Cook's Distance

- Measures influence of observation $i$
- $d_i = \frac{1}{\hat{\sigma}^2} \cdot (\hat{y}^{(-i)} - \hat{y})^T (\hat{y}^{(-i)} - \hat{y})$
- Can be expressed as: $d_i = \frac{\tilde{r}_i^2 h_i}{2(1-h_i)}$
- Observations with $d_i > 1$ considered dangerously influential

## Therapeutic Treatments

### For Non-Linear Relationships ($E[\varepsilon_i] \neq 0$)

Try transforming predictor variables:

- $\tilde{X} = \log(X)$
- $\tilde{X} = \sqrt{X}$
- $\tilde{X} = X^2$

### For Non-Constant Variance ($Var[\varepsilon_i] \neq constant$)

Try transforming response variable:

- Log transformation: useful when standard deviation is proportional to predicted values
- Square root transformation: for count data
- Arcsine transformation $\tilde{Y} = \arcsin(\sqrt{Y})$ or logit transformation $\tilde{Y} = \log\left(\frac{Y+0.005}{1.01-Y}\right)$ for percentage data

### For Outliers and High Leverage Points

1. Check for data collection errors
    - If error confirmed: omit the data point
    - If no error: proceed to step 2
2. Try variable transformations to address the outlier
3. If transformations don't help and the outlier affects parameter estimates too much, consider removing the observation (document this in reports)