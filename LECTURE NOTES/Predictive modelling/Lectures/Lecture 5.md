# Predictive Modeling - Classification and Logistic Regression

## Classification vs Regression

> [!info] Predictive Model Definition A predictive model is a functional relation between a dependent response variable Y and independent predictor variables $X_1, \ldots, X_p$:
> 
> $$Y = f(X_1, \ldots, X_p)$$

> [!important] Key Difference
> 
> - **Regression**: Used for quantitative (numeric) response variables
> - **Classification**: Used for qualitative (categorical) response variables with finite number of classes

## Examples of Classification Problems

> [!example] Common Classification Tasks
> 
> - Email spam detection (spam vs ham)
> - Predictive maintenance (okay vs defect)
> - Medical diagnosis (multiple conditions)
> - Student performance (pass vs fail)

## Simple Logistic Regression

### The Logistic Function

> [!note] Logistic Function The logistic function transforms any real number to the interval [0,1]:
> 
> $$p(t) = \frac{e^t}{1 + e^t}$$
> 
> where $t \in \mathbb{R}$

### Simple Logistic Regression Model

> [!formula] Simple Logistic Regression Given a binary response variable Y and quantitative predictor X:
> 
> $$P(Y = 1|X) = \frac{e^{\beta_0 + \beta_1 X}}{1 + e^{\beta_0 + \beta_1 X}}$$
> 
> Where $\beta_0$ and $\beta_1$ are regression coefficients estimated using maximum likelihood method.

## Key Concepts

### Odds

> [!definition] Odds The odds is an equivalent way of expressing probability:
> 
> $$\text{Odds} = \frac{p(X)}{1 - p(X)} = e^{\beta_0 + \beta_1 X}$$

> [!example] Odds Examples
> 
> - If $p(X) = 0.2$ (1 out of 5 default), then odds = $\frac{0.2}{0.8} = \frac{1}{4}$
> - If $p(X) = 0.9$ (9 out of 10 default), then odds = $\frac{0.9}{0.1} = 9$

### Logit (Log-Odds)

> [!formula] Logit Taking the natural logarithm of odds:
> 
> $$\ln\left(\frac{p(X)}{1 - p(X)}\right) = \beta_0 + \beta_1 X$$
> 
> This is called the **log-odds** or **logit**.

> [!important] Coefficient Interpretation A change of predictor X by one unit results in an average change of $\beta_1$ in the logit of the response being true.

## Model Prediction

### Classification Rule

> [!formula] Prediction Rule For a given value x of X, the predicted response is:
> 
> $$\hat{y} = \hat{f}(x) = \begin{cases} 1 & \text{if } \hat{p}(x) > 0.5 \ 0 & \text{otherwise} \end{cases}$$

> [!warning] Threshold Selection The choice of threshold (0.5) is crucial and context-dependent. For avoiding false positives (e.g., in legal contexts), use a higher threshold.

## Model Evaluation

### Classification Error

> [!formula] Classification Error $$\text{Err} = \frac{1}{n} \sum_{i=1}^{n} I(y_i \neq \hat{y}_i)$$
> 
> Where $I$ is the indicator function (1 if true, 0 otherwise).

### Confusion Matrix

> [!info] Confusion Matrix Components
> 
> - **True Positive (TP)**: Correctly classified as 1
> - **False Positive (FP)**: Classified as 1 but truly 0
> - **False Negative (FN)**: Classified as 0 but truly 1
> - **True Negative (TN)**: Correctly classified as 0

### Performance Metrics

> [!formula] Accuracy $$\text{Accuracy} = \frac{TP + TN}{TP + FP + FN + TN}$$

> [!formula] Precision $$\text{Precision} = \frac{TP}{TP + FP}$$
> 
> Answers: "Among all predicted positives, how many are actually positive?"

> [!formula] Recall (Sensitivity) $$\text{Recall} = \frac{TP}{TP + FN}$$
> 
> Answers: "Among all actual positives, how many did we predict correctly?"

> [!formula] F1 Score $$\text{F1 Score} = \frac{2 \cdot (\text{Recall} \cdot \text{Precision})}{\text{Recall} + \text{Precision}}$$
> 
> Weighted average of Precision and Recall, accounting for both false positives and false negatives.

## Class Imbalance Problem

> [!warning] Imbalanced Classes When one class dominates the dataset:
> 
> - The likelihood function is dominated by the majority class
> - Parameters are chosen to match mainly the majority class
> - Poor performance on minority class prediction

> [!tip] Solutions for Class Imbalance
> 
> - **Down-sampling**: Reduce the majority class
> - **Up-sampling**: Increase the minority class
> - **Adjust threshold**: Use different classification thresholds
> - **Cost-sensitive learning**: Assign different costs to misclassification types

## Cross-Validation

> [!important] Why Cross-Validation? Validating model accuracy on the same data used for training is not reliable. Cross-validation provides better estimates of model performance.

### k-Fold Cross-Validation

> [!method] k-Fold Cross-Validation Process
> 
> 1. Split data into training and test sets
> 2. Split training data into k folds
> 3. Train on k-1 folds, validate on remaining fold
> 4. Repeat k times
> 5. Average the results

> [!formula] Cross-Validation Error $$CV^{(k)} = \frac{1}{k} \sum_{i=1}^{k} \text{Err}_i$$
> 
> Common choices: k = 5, 10 Special case: k = n (leave-one-out cross-validation)

## Multiple Logistic Regression

> [!formula] Multiple Logistic Regression For multiple predictors $X_1, \ldots, X_p$:
> 
> $$P(Y|X_1, \ldots, X_p) = \frac{e^{\beta_0 + \beta_1 X_1 + \beta_2 X_2 + \cdots + \beta_p X_p}}{1 + e^{\beta_0 + \beta_1 X_1 + \beta_2 X_2 + \cdots + \beta_p X_p}}$$
> 
> Parameters $\beta_0, \beta_1, \ldots, \beta_p$ are estimated using maximum likelihood method.

## Example: Default Prediction

> [!example] Default Data Example **Variables:**
> 
> - `default`: Binary response (Yes/No)
> - `income`: Annual income (predictor)
> - `balance`: Monthly credit card balance (predictor)
> 
> **Model estimates:**
> 
> - $\hat{\beta}_0 = -10.6513$
> - $\hat{\beta}_1 = 0.0055$
> 
> **Predictions:**
> 
> - Balance = 1000: $\hat{p}(1000) \approx 0.00577$
> - Balance = 2000: $\hat{p}(2000) \approx 0.59$

