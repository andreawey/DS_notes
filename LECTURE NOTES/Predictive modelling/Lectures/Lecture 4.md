# Predictive Modeling - Polynomial Regression & Model Selection

## Polynomial Regression

> [!info] Non-linear Relationships When the relationship between predictor and response variables is non-linear, polynomial regression can be used to capture curved relationships.

### Basic Polynomial Model

For a quadratic relationship: $$\text{mpg} = \beta_0 + \beta_1 \cdot \text{horsepower} + \beta_2 \cdot \text{horsepower}^2 + \varepsilon$$

> [!example] Auto Dataset Example The relationship between miles per gallon (mpg) and horsepower shows a non-linear pattern that can be better captured with polynomial terms rather than simple linear regression.

## Residual Analysis in Multiple Linear Regression

### Key Diagnostic Plots

> [!note] Tukey-Anscombe Plot (Residuals vs Fitted)
> 
> - Shows residuals plotted against fitted values
> - Should show no pattern if model assumptions are met
> - Curved patterns indicate non-linear relationships

> [!note] Scale-Location Plot
> 
> - Plots standardized residuals against fitted values
> - Used to check for homoscedasticity (constant variance)
> - Horizontal line with random scatter indicates good fit

### Leverage and Influence

> [!important] Cook's Distance Measures the influence of individual observations on the regression results. Values > 0.5 or 1.0 may indicate influential points.

## Collinearity

> [!warning] Collinearity Problem Occurs when two or more predictor variables are closely related, causing:
> 
> - Increased standard errors
> - Unstable coefficient estimates
> - Difficulty interpreting individual predictor effects

### Variance Inflation Factor (VIF)

$$\text{VIF}(\hat{\beta}_j) = \frac{1}{1 - R^2_{X_j|X_{-j}}}$$

> [!tip] VIF Interpretation
> 
> - VIF = 1: No collinearity
> - VIF between 5-10: Problematic collinearity
> - VIF > 10: Severe collinearity

## Variable Selection Methods

### Forward Stepwise Selection

> [!abstract] Algorithm: Forward Stepwise Selection
> 
> 1. Start with null model $M_0$ (no predictors)
> 2. For $k = 0, \ldots, p-1$:
>     - Consider all $p-k$ models that add one predictor to $M_k$
>     - Choose the best model (lowest RSS or highest $R^2$) as $M_{k+1}$
> 3. Select best model using $C_p$, AIC, BIC, or adjusted $R^2$

### Backward Stepwise Selection

> [!abstract] Algorithm: Backward Stepwise Selection
> 
> 1. Start with full model $M_p$ (all predictors)
> 2. For $k = p, p-1, \ldots, 1$:
>     - Consider all $k$ models that remove one predictor from $M_k$
>     - Choose the best model (lowest RSS or highest $R^2$) as $M_{k-1}$
> 3. Select best model using model selection criteria

### Best Subset Selection

> [!warning] Computational Complexity
> 
> - With $p$ predictors, there are $2^p$ possible models
> - Computationally infeasible for $p > 40$
> - Forward/backward stepwise: only $1 + p(p+1)/2$ models for $p$ predictors

## Model Selection Criteria

### Adjusted R²

$$\text{adjusted } R^2 = 1 - \frac{\text{RSS}/(n-p-1)}{\text{TSS}/(n-1)}$$

> [!info] Purpose Penalizes the addition of unnecessary predictors, unlike regular $R^2$ which always increases with more predictors.

### Akaike Information Criterion (AIC)

$$\text{AIC} = -2\log(L) + 2q$$

For linear regression with normal errors: $$\text{AIC} = \frac{1}{n\hat{\sigma}^2}(\text{RSS} + 2p\hat{\sigma}^2)$$

### Mallow's Cp Statistic

$$C_p = \frac{1}{n}(\text{RSS} + 2p\hat{\sigma}^2)$$

> [!note] Relationship to AIC For least squares models, AIC is proportional to Mallow's $C_p$ statistic.

### Bayesian Information Criterion (BIC)

$$\text{BIC} = -2\log(L) + \log(n)q$$

For linear regression: $$\text{BIC} = \frac{1}{n}(\text{RSS} + \log(n)p\hat{\sigma}^2)$$

> [!tip] BIC vs AIC
> 
> - BIC penalizes model complexity more heavily than AIC
> - BIC tends to select simpler models
> - AIC optimizes prediction accuracy
> - BIC optimizes model parsimony

## The Marketing Plan Framework

> [!example] Key Questions for Model Building
> 
> 1. **Is there a relationship?** → F-test for overall significance
> 2. **How strong is the relationship?** → $R^2$ and RSE
> 3. **Which variables contribute?** → Individual t-tests and p-values
> 4. **How large is the effect?** → Confidence intervals for coefficients
> 5. **How accurate are predictions?** → Prediction vs confidence intervals
> 6. **Is the relationship linear?** → Residual analysis
> 7. **Is there synergy?** → Interaction terms

### Prediction vs Confidence Intervals

> [!important] Two Types of Intervals
> 
> - **Confidence Interval**: For average response $\bar{Y}$ (narrower)
> - **Prediction Interval**: For individual response $Y$ (wider, includes $\varepsilon$)

## Interaction Effects

### Including Interaction Terms

$$\text{sales} = \beta_0 + \beta_1 \cdot \text{TV} + \beta_2 \cdot \text{radio} + \beta_3 \cdot \text{TV} \times \text{radio} + \varepsilon$$

> [!success] Benefits of Interaction Terms
> 
> - Can capture synergistic effects between predictors
> - Often leads to substantial improvement in $R^2$
> - Helps address non-linear patterns in residuals

---

> [!summary] Key Takeaways
> 
> - Use polynomial regression for non-linear relationships
> - Check residual plots to validate model assumptions
> - Address collinearity using VIF diagnostics
> - Use stepwise selection for manageable variable selection
> - Compare models using AIC, BIC, $C_p$, or adjusted $R^2$
> - Consider interaction terms for synergistic effects

