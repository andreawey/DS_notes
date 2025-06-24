# Predictive Modeling - Multiple Linear Regression

## Multiple Linear Regression Model

> [!important] Multiple Linear Regression Formula $$Y = \beta_0 + \beta_1X_1 + \beta_2X_2 + \ldots + \beta_pX_p + \varepsilon$$
> 
> Where:
> 
> - $X_j$: $j$th predictor variable
> - $\beta_j$: association between $X_j$ and response variable $Y$
> - $\varepsilon$: error term

### Parameter Estimation

> [!note] Least Squares Estimation Parameters are estimated by minimizing the Residual Sum of Squares (RSS):
> 
> $$RSS = \sum_{i=1}^{n} r_i^2 = \sum_{i=1}^{n} (y_i - \hat{\beta}_0 - \hat{\beta}_1x_{i1} - \hat{\beta}_2x_{i2} - \ldots - \hat{\beta}_px_{ip})^2$$

> [!example] Prediction Formula $$\hat{y} = \hat{\beta}_0 + \hat{\beta}_1x_1 + \hat{\beta}_2x_2 + \ldots + \hat{\beta}_px_p$$

## Key Questions in Multiple Linear Regression

1. **Is there a relationship between response and predictors?**
2. **Which predictors are important?**
3. **How well does the model fit the data?**
4. **How accurate are predictions?**

## Hypothesis Testing

### F-Test for Overall Model Significance

> [!important] F-Test Hypotheses **Null Hypothesis:** $H_0: \beta_1 = \beta_2 = \ldots = \beta_p = 0$
> 
> **Alternative Hypothesis:** $H_A$: at least one $\beta_i$ is non-zero

> [!formula] F-Statistic $$F = \frac{(TSS - RSS)/p}{RSS/(n - p - 1)}$$
> 
> Where:
> 
> - $TSS = \sum_{i=1}^{n}(y_i - \bar{y})^2$ (Total Sum of Squares)
> - $RSS = \sum_{i=1}^{n}(y_i - \hat{y}_i)^2$ (Residual Sum of Squares)
> - Under $H_0$: $F \sim F_{p, n-p-1}$

> [!tip] Interpretation
> 
> - If $H_0$ is true: $F \approx 1$
> - If $H_A$ is true: $F > 1$

### F-Test for Subset of Predictors

> [!important] Testing Subset of q Coefficients **Null Hypothesis:** $H_0: \beta_{p-q+1} = \beta_{p-q+2} = \ldots = \beta_p = 0$

> [!formula] F-Statistic for Subset $$F = \frac{(RSS_0 - RSS)/q}{RSS/(n - p - 1)}$$
> 
> Where:
> 
> - $RSS_0$: RSS for reduced model (without q predictors)
> - $RSS$: RSS for full model
> - Under $H_0$: $F \sim F_{q, n-p-1}$

## Model Assessment

### R-Squared

> [!note] Coefficient of Determination $$R^2 = Cor[Y, \hat{Y}]^2$$
> 
> - Fraction of variance explained by the model
> - Values close to 1 indicate good fit

### Residual Standard Error (RSE)

> [!formula] RSE Formula $$RSE = \sqrt{\frac{RSS}{n-p-1}}$$

## Qualitative Predictor Variables

### Two-Level Factor Variables

> [!example] Dummy Variable Encoding For gender (male/female): $$x_i = \begin{cases} 1 & \text{if ith person is female} \ 0 & \text{if ith person is male} \end{cases}$$
> 
> **Model:** $$y_i = \beta_0 + \beta_1x_i + \varepsilon_i = \begin{cases} \beta_0 + \beta_1 + \varepsilon_i & \text{if female} \ \beta_0 + \varepsilon_i & \text{if male} \end{cases}$$

### Three-Level Factor Variables

> [!example] Multiple Dummy Variables For ethnicity (Asian, Caucasian, African American):
> 
> $$x_{i1} = \begin{cases} 1 & \text{if Asian} \ 0 & \text{otherwise} \end{cases}$$
> 
> $$x_{i2} = \begin{cases} 1 & \text{if Caucasian} \ 0 & \text{otherwise} \end{cases}$$
> 
> **Model:** $$y_i = \beta_0 + \beta_1x_{i1} + \beta_2x_{i2} + \varepsilon_i$$

> [!tip] Baseline Category
> 
> - Always one fewer dummy variable than number of levels
> - Category without dummy variable = baseline (reference category)

## Interaction Effects

### Concept of Interaction

> [!warning] Additivity vs. Interaction 
> **Additivity Assumption:** Effect of $X_j$ on $Y$ is independent of other predictors
> 
> **Interaction:** Effect of one predictor depends on values of another predictor

### Two-Way Interaction Model

> [!formula] Interaction Model $$Y = \beta_0 + \beta_1X_1 + \beta_2X_2 + \beta_3(X_1 \cdot X_2) + \varepsilon$$
> 
> Can be rewritten as: $$Y = \beta_0 + (\beta_1 + \beta_3X_2)X_1 + \beta_2X_2 + \varepsilon$$

> [!note] Interpretation of Interaction Term $\beta_3$: Change in the effect of $X_1$ for a one-unit increase in $X_2$

### Qualitative-Quantitative Interaction

> [!example] Student Status × Income **Without Interaction:** $$\text{balance}_i = \beta_0 + \beta_1 \cdot \text{income}_i + \begin{cases} \beta_2 & \text{if student} \ 0 & \text{if not student} \end{cases}$$
> 
> **With Interaction:** $$\text{balance}_i = \beta_0 + \beta_1 \cdot \text{income}_i + \begin{cases} \beta_2 + \beta_3 \cdot \text{income}_i & \text{if student} \ 0 & \text{if not student} \end{cases}$$

## Important Considerations

> [!warning] Variable Selection
> 
> - Task of determining which predictors to include
> - Studied in Linear Model Selection methods
> - Use F-tests to compare nested models

> [!tip] Model Comparison
> 
> - Compare models using F-tests
> - Higher $R^2$ doesn't always mean better model
> - Consider statistical significance of predictors

> [!note] Prediction Intervals
> 
> - **Confidence Interval:** For average response at given predictor values
> - **Prediction Interval:** For individual response at given predictor values
> - Prediction intervals are wider than confidence intervals

