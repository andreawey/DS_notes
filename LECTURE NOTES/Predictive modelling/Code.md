```python
# Essential imports for statistical analysis
import pandas as pd 
import numpy as np 
import matplotlib.pyplot as plt 
import seaborn as sns
import statsmodels.api as sm
import statsmodels.formula.api as smf 
from statsmodels.graphics.gofplots import ProbPlot
from statsmodels.stats.outliers_influence import variance_inflation_factor, OLSInfluence 
from statsmodels.tsa.seasonal import seasonal_decompose, STL 
from statsmodels.tsa.arima.model import ARIMA 
from statsmodels.tsa.stattools import acf, pacf 
from scipy.stats import norm, chi2 
import random 
from itertools import product 
import warnings
```

## Model diagnostic functions

```python
def plot_residuals(axes, res, yfit, n_samp=100):
    """
    Plot residuals vs fitted values with confidence band
    
    Parameters:
    - axes: matplotlib axes object
    - res: residuals (array-like)
    - yfit: fitted values (array-like) 
    - n_samp: number of resamples for confidence band
    """
    # Add confidence band through resampling
    for i in range(n_samp):
        samp_res_id = random.sample(list(res), len(res))
        sns.regplot(x=yfit, y=samp_res_id, scatter=False, ci=False, lowess=True,
                    line_kws={'color': 'lightgrey', 'lw': 1, 'alpha': 0.8}, ax=axes)
    
    # Main residual plot
    dataframe = pd.concat([yfit, res], axis=1)
    sns.residplot(x=yfit, y=res, data=dataframe, lowess=True,
                  scatter_kws={'alpha': 0.5}, 
                  line_kws={'color': 'red', 'lw': 2, 'alpha': 0.8}, ax=axes)
    axes.set_title('Residuals vs Fitted Values')
    axes.set_ylabel('Residuals')
    axes.set_xlabel('Fitted Values')
    axes.grid(True, alpha=0.3)

def plot_QQ(axes, res_standard, n_samp=100):
    """
    Normal Q-Q plot for standardized residuals
    
    Parameters:
    - axes: matplotlib axes object
    - res_standard: standardized residuals
    - n_samp: number of resamples for confidence band
    """
    QQ = ProbPlot(res_standard)
    qqx = pd.Series(sorted(QQ.theoretical_quantiles), name="x")
    qqy = pd.Series(QQ.sorted_data, name="y")

    # Add confidence band
    if n_samp > 0:
        mu = np.mean(qqy)
        sigma = np.std(qqy)
        for _ in range(n_samp):
            samp_res_id = np.random.normal(mu, sigma, len(qqx))
            sns.regplot(x=qqx, y=sorted(samp_res_id), lowess=True, ci=False, scatter=False,
                        line_kws={'color': 'lightgrey', 'lw': 1, 'alpha': 0.8}, ax=axes)
    
    # Main Q-Q plot
    sns.regplot(x=qqx, y=qqy, scatter=True, lowess=False, ci=False,
                scatter_kws={'s': 40, 'alpha': 0.7}, 
                line_kws={'color': 'red', 'lw': 2, 'alpha': 0.8}, ax=axes)
    axes.plot(qqx, qqx, '--k', alpha=0.5, label='Perfect Normal')
    axes.set_title('Normal Q-Q Plot')
    axes.set_xlabel('Theoretical Quantiles')
    axes.set_ylabel('Standardized Residuals')
    axes.legend()
    axes.grid(True, alpha=0.3)

def plot_scale_location(axes, yfit, res_stand_sqrt, n_samp=100):
    """
    Scale-Location plot (sqrt of standardized residuals vs fitted)
    
    Parameters:
    - axes: matplotlib axes object
    - yfit: fitted values
    - res_stand_sqrt: sqrt of absolute standardized residuals
    - n_samp: number of resamples for confidence band
    """
    # Add confidence band
    for i in range(n_samp):
        samp_res_id = random.sample(list(res_stand_sqrt), len(res_stand_sqrt))
        sns.regplot(x=yfit, y=samp_res_id, scatter=False, ci=False, lowess=True,
                    line_kws={'color': 'lightgrey', 'lw': 1, 'alpha': 0.8}, ax=axes)
    
    # Main scale-location plot
    sns.regplot(x=yfit, y=res_stand_sqrt, scatter=True, ci=False, lowess=True,
                scatter_kws={'s': 40, 'alpha': 0.7}, 
                line_kws={'color': 'red', 'lw': 2, 'alpha': 0.8}, ax=axes)
    axes.set_title('Scale-Location Plot')
    axes.set_xlabel('Fitted Values')
    axes.set_ylabel('√|Standardized Residuals|')
    axes.grid(True, alpha=0.3)

def plot_leverage_cooks(axes, leverage, res_standard, n_pred=1, n_levels=4):
    """
    Residuals vs Leverage plot with Cook's distance contours
    
    Parameters:
    - axes: matplotlib axes object
    - leverage: leverage values
    - res_standard: standardized residuals
    - n_pred: number of predictors
    - n_levels: number of Cook's distance contour levels
    """
    sns.regplot(x=leverage, y=res_standard, scatter=True, ci=False, lowess=True,
                scatter_kws={'s': 40, 'alpha': 0.7}, 
                line_kws={'color': 'red', 'lw': 2, 'alpha': 0.8}, ax=axes)
    
    # Set axis limits
    x_min, x_max = min(leverage)*0.95, max(leverage)*1.05
    y_min, y_max = min(res_standard)*0.95, max(res_standard)*1.05
    
    # Add horizontal line at y=0
    axes.axhline(y=0, color='green', linestyle='--', alpha=0.8)

    # Cook's distance contours
    n_grid = 100
    cooks_distance = np.zeros((n_grid, n_grid))
    x_cooks = np.linspace(x_min, x_max, n_grid)
    y_cooks = np.linspace(y_min, y_max, n_grid)
    
    for xi in range(n_grid):
        for yi in range(n_grid):
            if x_cooks[xi] < 1:  # Avoid division by zero
                cooks_distance[yi][xi] = (y_cooks[yi]**2 * x_cooks[xi] / 
                                        (1 - x_cooks[xi]) / (n_pred + 1))
    
    CS = axes.contour(x_cooks, y_cooks, cooks_distance, levels=n_levels, alpha=0.6)
    axes.clabel(CS, inline=True, fontsize=10)
    axes.set_xlim(x_min, x_max)
    axes.set_ylim(y_min, y_max)
    axes.set_title("Residuals vs Leverage (Cook's Distance)")
    axes.set_xlabel('Leverage')
    axes.set_ylabel('Standardized Residuals')
    axes.grid(True, alpha=0.3)

def comprehensive_model_diagnostics(model, figsize=(15, 12)):
    """
    Generate all four standard diagnostic plots for a regression model
    
    Parameters:
    - model: fitted statsmodels regression model
    - figsize: figure size tuple
    
    Returns:
    - fig: matplotlib figure object
    """
    # Extract necessary components
    yfit = model.fittedvalues
    res = model.resid
    res_inf = model.get_influence()
    res_standard = res_inf.resid_studentized_internal
    res_stand_sqrt = np.sqrt(np.abs(res_standard))
    leverage = res_inf.hat_matrix_diag

    # Create subplot figure
    fig, ((ax1, ax2), (ax3, ax4)) = plt.subplots(2, 2, figsize=figsize)
    
    # Generate all plots
    plot_residuals(ax1, res, yfit)
    plot_QQ(ax2, res_standard)
    plot_scale_location(ax3, yfit, res_stand_sqrt)
    plot_leverage_cooks(ax4, leverage, res_standard, n_pred=model.df_model)
    
    plt.tight_layout()
    return fig


# don't leave this in the notebook :
def summarize_model_diagnostics(model):
    """
    Generate textual summary for regression diagnostics based on residual patterns.

    Parameters:
    - model: fitted statsmodels regression model

    Returns:
    - report: dictionary with plot-type keys and diagnostic comment values
    """
    report = {}

    # Extract components
    yfit = model.fittedvalues
    res = model.resid
    res_inf = model.get_influence()
    res_standard = res_inf.resid_studentized_internal
    res_stand_sqrt = np.sqrt(np.abs(res_standard))
    leverage = res_inf.hat_matrix_diag
    cooks_d = res_inf.cooks_distance[0]
    
    # 1. Residuals vs Fitted
    if np.corrcoef(yfit, res)[0, 1] > 0.1:
        comment = "Residuals show correlation with fitted values — possible nonlinearity or model misspecification."
    elif np.var(res) < 1e-5:
        comment = "Residuals show very low variance — check for overfitting or poor model fit."
    else:
        comment = "Residuals are roughly uncorrelated with fitted values — linearity assumption seems reasonable."
    report['Residuals vs Fitted'] = comment

    # 2. Q-Q Plot
    sorted_resid = np.sort(res_standard[~np.isnan(res_standard)])
    norm_quantiles = np.sort(np.random.normal(0, 1, size=len(sorted_resid)))
    qq_diff = np.mean(np.abs(sorted_resid - norm_quantiles))
    if qq_diff > 0.5:
        comment = "Standardized residuals deviate strongly from normal — normality assumption likely violated."
    elif qq_diff > 0.2:
        comment = "Mild deviations from normality in residuals — may impact inference."
    else:
        comment = "Residuals appear approximately normal — normality assumption likely satisfied."
    report['Normal Q-Q Plot'] = comment

    # 3. Scale-Location Plot
    trend = np.corrcoef(yfit, res_stand_sqrt)[0, 1]
    if trend > 0.2:
        comment = "Spread of residuals increases with fitted values — possible heteroscedasticity."
    elif trend < -0.2:
        comment = "Spread of residuals decreases with fitted values — possible heteroscedasticity."
    else:
        comment = "Spread of residuals appears constant — homoscedasticity assumption likely satisfied."
    report['Scale-Location Plot'] = comment

    # 4. Residuals vs Leverage (Cook’s Distance)
    high_leverage = np.sum(leverage > 2 * np.mean(leverage))
    high_cook = np.sum(cooks_d > 4 / len(leverage))
    issues = []
    if high_leverage > 0:
        issues.append(f"{high_leverage} high-leverage point(s)")
    if high_cook > 0:
        issues.append(f"{high_cook} influential point(s) (Cook's D > 4/n)")
    if issues:
        comment = "Potential influence issues: " + ", ".join(issues) + ". Check for outliers or leverage."
    else:
        comment = "No major leverage or influential point concerns."
    report["Residuals vs Leverage"] = comment

    return report

# usage :
diagnostic_summary = summarize_model_diagnostics(model)
for plot, comment in diagnostic_summary.items():
    print(f"{plot}:\n  → {comment}\n")

```

regression plot
```python
sns.regplot(x=x, y=y, scatter=True, ci=False, lowess=False, scatter_kws={'s': 40, 'alpha': 0.5}, line_kws={'color': 'red', 'lw': 1, 'alpha': 0.8})
```
## Basic functions

```python
y = clocks['price']
x = clocks['age']
x_sm = sm.add_constant(x)
model = sm.OLS(y, x_sm).fit()
print(model.summary())

y_reg = model.params.iloc[0] + x*model.params.iloc[1]

plt.scatter(clocks['age'], clocks['price'])
plt.plot(x, y_reg)
plt.show()
```

## feature encoding (one-hot)

```python
dummies = pd.get_dummies(df['feat'])
df = df.join(dummies)

# change to int instead of bool
bool_cols = df_dummies.select_dtypes(include='bool').columns
df_dummies[bool_cols] = df_dummies[bool_cols].astype(int)
```
## Confidence interval
```python
model.conf_int(0.01)
```

## Expected value of $y$ given $x_0$
```python
x0 = [[1, x_0]] # always add an 1, before the values ( one for each predictor )
pred0 = model.get_prediction(x0)

# get the 95% confidence interval summary frame
pred0.summary_frame(alpha=0.05)
```

- mean_ci_lower / upper : confidence interval
- obs_ci_lower / upper : prediction interval


## Outlier influence
```python
from statsmodels.stats.outliers_influence import OLSInfluence
model_inf = OLSInfluence(model)
print(model_inf.summary_table())
```

## Outlier Leverage
```python
res_inf = model.get_influence()
res_inf_leverage = res_inf.hat_matrix_diag
index = np.argsort(res_inf_leverage)
# print last 3
last3 = {'Country': savings.iloc[index, 0].iloc[-3:],
'Leverage': res_inf_leverage[index][-3:]}
last3 = pd.DataFrame(data=last3)
print(last3)
```

## Find correlation

```python
# Find correlations:
corr = senic.drop('length', axis=1).corr()
import seaborn as sns
import matplotlib.pyplot as plt
fig = plt.figure(figsize = (10,8))
ax1 = fig.add_subplot(1, 1, 1)
sns.heatmap(corr)
plt.show()
```
## Anova test
```python
table = sm.stats.anova_lm(model_ii, model_opt)
```
## Multicollinearity analysis

```python
def calculate_vif(X):
    """
    Correct VIF calculation 
    """
    X_with_const = sm.add_constant(X)
    vif_data = pd.DataFrame()
    vif_data["Variable"] = X_with_const.columns
    vif_data["VIF"] = [variance_inflation_factor(X_with_const.values, i)
                       for i in range(X_with_const.shape[1])]
    vif_data = vif_data.sort_values('VIF', ascending=False)
    vif_data = vif_data[vif_data['Variable'] != 'const']
    return vif_data


def interpret_vif(vif_df):
    """
    Provide interpretation of VIF scores
    
    Parameters:
    - vif_df: DataFrame from calculate_vif()
    """
    print("VIF Interpretation:")
    print("==================")
    for _, row in vif_df.iterrows():
        var_name = row['Variable']
        vif_val = row['VIF']
        
        if vif_val < 5:
            status = "✅ Low multicollinearity"
        elif vif_val < 10:
            status = "⚠️ Moderate multicollinearity"
        else:
            status = "❌ High multicollinearity - consider removal"
        
        print(f"{var_name}: VIF = {vif_val:.2f} - {status}")

```

## Random downsampling for class imbalance
```python
def downsample_to_balance(df, target_col, majority_label=None, seed=42):
    np.random.seed(seed)
    if majority_label is None:
        majority_label = df[target_col].value_counts().idxmax()
    minority_label = 1 - majority_label
    
    df_majority = df[df[target_col] == majority_label]
    df_minority = df[df[target_col] == minority_label]
    
    df_majority_down = df_majority.sample(n=len(df_minority), replace=False, random_state=seed)
    return pd.concat([df_majority_down, df_minority]).sample(frac=1, random_state=seed)  # shuffle
```


## Logistic regression model
```python
X_const= sm.add_constant(X)
model = sm.GLM(y, X_const, family=sm.families.Binomial()).fit()
print(model.summary_table())

#calculate probabilities
prob_stud = model.predict([[1, 1]])
prob_nonstud = model.predict([[1, 0]])

print(f"P(default | student):     {prob_stud:.4f}")
print(f"P(default | non-student): {prob_nonstud:.4f}")
```

## Confusion matrix and accuracy
```python
def confusion_and_accuracy(y_true, y_pred):
    from sklearn.metrics import confusion_matrix, accuracy_score
    cm = confusion_matrix(y_true, y_pred)
    acc = accuracy_score(y_true, y_pred)
    print("Confusion Matrix:\n", cm)
    print(f"Accuracy: {acc:.2%}")

def classification_error(y_true, y_pred):
    return np.mean(y_true != y_pred)

## easy confusion matrix
confusion = pd.DataFrame({'predicted': y_pred,
'true': y_train})
confusion = pd.crosstab(confusion.predicted, confusion.true,
margins=True, margins_name="Sum")
print(confusion)
```

## Roc curve + AUC

```python
def plot_roc_curve(y_true, y_prob):
    from sklearn.metrics import roc_curve, auc
    fpr, tpr, _ = roc_curve(y_true, y_prob)
    auc_score = auc(fpr, tpr)
    
    plt.plot(fpr, tpr, label=f'ROC curve (AUC = {auc_score:.2f})')
    plt.plot([0, 1], [0, 1], 'k--')
    plt.xlabel("False Positive Rate")
    plt.ylabel("True Positive Rate")
    plt.title("ROC Curve")
    plt.legend()
    plt.grid()
    plt.show()

```


## Model selection function

```python
def fit_and_score_model(X, y, metric='aic'):
    """
    Fit OLS model and return scoring metrics
    
    Parameters:
    - X: predictor variables (DataFrame)
    - y: response variable
    - metric: scoring metric ('aic', 'bic', 'rsquared', 'rsquared_adj', 'ssr')
    
    Returns:
    - Dictionary of model scores
    """
    X_const = sm.add_constant(X)
    model = sm.OLS(y, X_const).fit()
    
    scores = {
        'aic': model.aic,
        'bic': model.bic,
        'rsquared': model.rsquared,
        'rsquared_adj': model.rsquared_adj,
        'ssr': model.ssr,
        'n_predictors': X.shape[1]
    }
    return scores

def forward_selection(X, y, metric='aic', verbose=True):
    """
    Forward stepwise selection
    
    Parameters:
    - X: DataFrame of all available predictors
    - y: response variable
    - metric: selection criterion
    - verbose: print progress
    
    Returns:
    - List of selected features in order
    - DataFrame of selection history
    """
    remaining_features = list(X.columns)
    selected_features = []
    selection_history = []
    
    if metric in ['aic', 'bic', 'ssr']:
        best_score = np.inf
        minimize = True
    else:
        best_score = -np.inf
        minimize = False
    
    while remaining_features:
        scores = []
        
        for feature in remaining_features:
            current_features = selected_features + [feature]
            score_dict = fit_and_score_model(X[current_features], y, metric)
            scores.append((feature, score_dict[metric], score_dict))
        
        # Find best feature to add
        if minimize:
            best_feature, score, full_scores = min(scores, key=lambda x: x[1])
        else:
            best_feature, score, full_scores = max(scores, key=lambda x: x[1])
        
        # Check if improvement
        if (minimize and score < best_score) or (not minimize and score > best_score):
            selected_features.append(best_feature)
            remaining_features.remove(best_feature)
            best_score = score
            
            selection_history.append({
                'step': len(selected_features),
                'added': best_feature,
                'features': selected_features.copy(),
                **full_scores
            })
            
            if verbose:
                print(f"Step {len(selected_features)}: Added '{best_feature}' - {metric.upper()}: {score:.4f}")
        else:
            if verbose:
                print(f"No improvement found. Stopping forward selection.")
            break
    
    return selected_features, pd.DataFrame(selection_history)

def backward_elimination(X, y, metric='aic', verbose=True):
    """
    Backward stepwise elimination
    
    Parameters:
    - X: DataFrame of all predictors
    - y: response variable  
    - metric: selection criterion
    - verbose: print progress
    
    Returns:
    - List of remaining features
    - DataFrame of elimination history
    """
    current_features = list(X.columns)
    elimination_history = []
    
    if metric in ['aic', 'bic', 'ssr']:
        minimize = True
    else:
        minimize = False
    
    # Get initial score
    initial_scores = fit_and_score_model(X[current_features], y, metric)
    best_score = initial_scores[metric]
    
    if verbose:
        print(f"Initial model with all features - {metric.upper()}: {best_score:.4f}")
    
    while len(current_features) > 1:
        scores = []
        
        for feature in current_features:
            remaining_features = [f for f in current_features if f != feature]
            score_dict = fit_and_score_model(X[remaining_features], y, metric)
            scores.append((feature, score_dict[metric], remaining_features, score_dict))
        
        # Find best feature to remove
        if minimize:
            worst_feature, score, remaining, full_scores = min(scores, key=lambda x: x[1])
        else:
            worst_feature, score, remaining, full_scores = max(scores, key=lambda x: x[1])
        
        # Check if improvement
        if (minimize and score < best_score) or (not minimize and score > best_score):
            current_features = remaining
            best_score = score
            
            elimination_history.append({
                'step': len(elimination_history) + 1,
                'removed': worst_feature,
                'remaining_features': current_features.copy(),
                **full_scores
            })
            
            if verbose:
                print(f"Step {len(elimination_history)}: Removed '{worst_feature}' - {metric.upper()}: {score:.4f}")
        else:
            if verbose:
                print(f"No improvement found. Stopping backward elimination.")
            break
    
    return current_features, pd.DataFrame(elimination_history)

## easy forward selection
from sklearn.feature_selection import SequentialFeatureSelector
from sklearn.linear_model import LinearRegression
# define Linear Regression Model in sklearn
linearmodel = LinearRegression()
# Sequential Feature Selection using sklearn
sfs = SequentialFeatureSelector(linearmodel, n_features_to_select=3,direction='forward')
sfs.fit(x_full, y)
chosen_predictors = x_full.columns[sfs.support_].values
# Print Chosen variables
print(chosen_predictors)
```


## Seasonal decomposition 
```python
from statsmodels.tsa.seasonal import seasonal_decompose
decomp= seasonal_decompose(ausBeer['megalitres'], model='additive', period=4)
fig = decomp.plot()
fig.set_size_inches(12,6)
plt.show()


from statsmodels.tsa.seasonal import STL
decomp = STL(ausBeer['megalitres'], seasonal=5).fit()
fig = decomp.plot()
```


## zeros of characteristic polynomial

$$\phi(x) = 1 - 1.21x + 0.5x^2 + 0.13x^3 - 0.24x^4$$
```python
import numpy as np
roots = np.roots([-0.24, 0.13, 0.5, -1.21, 1])
print(np.round(roots, 3))

#process is stationary if the absolute values of the zeros of the characteristic polynomial are all larger than 1
print(np.round(abs(roots), 3))
```

## Logistic regression helpers

```python
def classification_metrics(y_true, y_pred, y_prob=None):
    """
    Calculate comprehensive classification metrics
    
    Parameters:
    - y_true: true binary labels
    - y_pred: predicted binary labels  
    - y_prob: predicted probabilities (optional)
    
    Returns:
    - Dictionary of metrics
    """
    # Basic counts
    tp = np.sum((y_pred == 1) & (y_true == 1))
    tn = np.sum((y_pred == 0) & (y_true == 0))
    fp = np.sum((y_pred == 1) & (y_true == 0))
    fn = np.sum((y_pred == 0) & (y_true == 1))
    
    # Calculate metrics
    accuracy = (tp + tn) / (tp + tn + fp + fn)
    precision = tp / (tp + fp) if (tp + fp) > 0 else 0
    recall = tp / (tp + fn) if (tp + fn) > 0 else 0
    f1 = 2 * (precision * recall) / (precision + recall) if (precision + recall) > 0 else 0
    specificity = tn / (tn + fp) if (tn + fp) > 0 else 0
    
    metrics = {
        'accuracy': accuracy,
        'precision': precision,
        'recall': recall,
        'f1_score': f1,
        'specificity': specificity,
        'true_positives': tp,
        'true_negatives': tn,
        'false_positives': fp,
        'false_negatives': fn
    }
    
    return metrics

def plot_roc_curve(y_true, y_prob, ax=None):
    """
    Plot ROC curve for binary classification
    
    Parameters:
    - y_true: true binary labels
    - y_prob: predicted probabilities
    - ax: matplotlib axes (optional)
    
    Returns:
    - AUC score
    """
    if ax is None:
        fig, ax = plt.subplots(figsize=(8, 6))
    
    # Calculate ROC curve points
    thresholds = np.linspace(0, 1, 100)
    tpr_values = []
    fpr_values = []
    
    for threshold in thresholds:
        y_pred = (y_prob >= threshold).astype(int)
        metrics = classification_metrics(y_true, y_pred)
        tpr_values.append(metrics['recall'])  # True Positive Rate
        fpr_values.append(1 - metrics['specificity'])  # False Positive Rate
    
    # Calculate AUC using trapezoidal rule
    auc = np.trapz(tpr_values, fpr_values)
    
    # Plot ROC curve
    ax.plot(fpr_values, tpr_values, 'b-', lw=2, label=f'ROC Curve (AUC = {auc:.3f})')
    ax.plot([0, 1], [0, 1], 'r--', lw=2, label='Random Classifier')
    ax.set_xlabel('False Positive Rate')
    ax.set_ylabel('True Positive Rate')
    ax.set_title('ROC Curve')
    ax.legend()
    ax.grid(True, alpha=0.3)
    
    return abs(auc)  # Ensure positive AUC

```

## Data transformation functions

```python
def box_cox_transform(x, lambda_val):
    """
    Apply Box-Cox transformation
    
    Parameters:
    - x: input data (must be positive)
    - lambda_val: transformation parameter
    
    Returns:
    - Transformed data
    """
    if lambda_val == 0:
        return np.log(x)
    else:
        return (x**lambda_val - 1) / lambda_val

def find_optimal_box_cox_lambda(x, y, lambda_range=(-2, 2, 0.1)):
    """
    Find optimal Box-Cox lambda by minimizing residual sum of squares
    
    Parameters:
    - x: predictor variable
    - y: response variable (must be positive)
    - lambda_range: (min, max, step) for lambda search
    
    Returns:
    - Optimal lambda value
    - Dictionary of lambda vs RSS
    """
    lambdas = np.arange(lambda_range[0], lambda_range[1] + lambda_range[2], lambda_range[2])
    rss_values = []
    
    for lam in lambdas:
        y_transformed = box_cox_transform(y, lam)
        X_const = sm.add_constant(x)
        model = sm.OLS(y_transformed, X_const).fit()
        rss_values.append(model.ssr)
    
    optimal_idx = np.argmin(rss_values)
    optimal_lambda = lambdas[optimal_idx]
    
    lambda_rss_dict = dict(zip(lambdas, rss_values))
    
    return optimal_lambda, lambda_rss_dict

```


## Data loading and exploration
```python
def load_and_explore_data(filepath, target_col=None):
    """
    Load dataset and perform initial exploration
    
    Parameters:
    - filepath: path to CSV file
    - target_col: name of target/response variable (optional)
    
    Returns:
    - DataFrame
    """
    try:
        df = pd.read_csv(filepath)
        print(f"✅ Successfully loaded data: {df.shape[0]} rows, {df.shape[1]} columns")
        
        print("\n📊 Data Overview:")
        print("=" * 50)
        print(f"Shape: {df.shape}")
        print(f"\nColumn Types:\n{df.dtypes}")
        print(f"\nMissing Values:\n{df.isnull().sum()}")
        
        print(f"\n📈 Statistical Summary:")
        display(df.describe())
        
        print(f"\n👀 First 5 rows:")
        display(df.head())
        
        if target_col and target_col in df.columns:
            print(f"\n🎯 Target Variable '{target_col}' Distribution:")
            if df[target_col].dtype in ['int64', 'float64']:
                plt.figure(figsize=(10, 4))
                plt.subplot(1, 2, 1)
                plt.hist(df[target_col], bins=30, alpha=0.7, edgecolor='black')
                plt.title(f'Distribution of {target_col}')
                plt.xlabel(target_col)
                plt.ylabel('Frequency')
                
                plt.subplot(1, 2, 2)
                plt.boxplot(df[target_col])
                plt.title(f'Boxplot of {target_col}')
                plt.ylabel(target_col)
                plt.tight_layout()
                plt.show()
            else:
                print(df[target_col].value_counts())
        
        return df
        
    except FileNotFoundError:
        print(f"❌ File not found: {filepath}")
        return None
    except Exception as e:
        print(f"❌ Error loading data: {str(e)}")
        return None
```


## Simple linear regression
```python
def simple_linear_regression_analysis(X, y, X_name="X", y_name="y"):
    """
    Comprehensive simple linear regression analysis
    
    Parameters:
    - X: predictor variable (Series or array)
    - y: response variable (Series or array)
    - X_name, y_name: variable names for labels
    
    Returns:
    - Fitted model, results dictionary
    """
    # Prepare data
    X_const = sm.add_constant(X)
    
    # Fit model
    model = sm.OLS(y, X_const).fit()
    
    # Extract key results
    results = {
        'intercept': model.params[0],
        'slope': model.params[1],
        'r_squared': model.rsquared,
        'adj_r_squared': model.rsquared_adj,
        'f_statistic': model.fvalue,
        'f_pvalue': model.f_pvalue,
        'slope_pvalue': model.pvalues[1],
        'aic': model.aic,
        'bic': model.bic
    }
    
    # Print interpretation
    print("🔍 Simple Linear Regression Results")
    print("=" * 40)
    print(f"Equation: {y_name} = {results['intercept']:.4f} + {results['slope']:.4f} × {X_name}")
    print(f"R-squared: {results['r_squared']:.4f} ({results['r_squared']*100:.2f}% of variance explained)")
    print(f"Adjusted R-squared: {results['adj_r_squared']:.4f}")
    print(f"F-statistic: {results['f_statistic']:.4f} (p-value: {results['f_pvalue']:.6f})")
    print(f"Slope p-value: {results['slope_pvalue']:.6f}")
    
    # Interpretation
    if results['f_pvalue'] < 0.05:
        print("✅ Model is statistically significant (F-test p < 0.05)")
    else:
        print("❌ Model is not statistically significant (F-test p ≥ 0.05)")
    
    if results['slope_pvalue'] < 0.05:
        print(f"✅ Slope is statistically significant (p < 0.05)")
        if results['slope'] > 0:
            print(f"📈 Positive relationship: As {X_name} increases, {y_name} tends to increase")
        else:
            print(f"📉 Negative relationship: As {X_name} increases, {y_name} tends to decrease")
    else:
        print(f"❌ Slope is not statistically significant (p ≥ 0.05)")
    
    # Visualization
    fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(15, 6))
    
    # Scatter plot with regression line
    ax1.scatter(X, y, alpha=0.6, s=50)
    X_range = np.linspace(X.min(), X.max(), 100)
    X_range_const = sm.add_constant(X_range)
    y_pred_range = model.predict(X_range_const)
    ax1.plot(X_range, y_pred_range, 'r-', lw=2, 
             label=f'y = {results["intercept"]:.3f} + {results["slope"]:.3f}x')
    ax1.set_xlabel(X_name)
    ax1.set_ylabel(y_name)
    ax1.set_title(f'Simple Linear Regression\nR² = {results["r_squared"]:.4f}')
    ax1.legend()
    ax1.grid(True, alpha=0.3)
    
    # Residuals vs fitted
    y_pred = model.fittedvalues
    residuals = model.resid
    ax2.scatter(y_pred, residuals, alpha=0.6, s=50)
    ax2.axhline(y=0, color='r', linestyle='--', alpha=0.8)
    ax2.set_xlabel('Fitted Values')
    ax2.set_ylabel('Residuals')
    ax2.set_title('Residuals vs Fitted Values')
    ax2.grid(True, alpha=0.3)
    
    plt.tight_layout()
    plt.show()
    
    # Full model summary
    print(f"\n📋 Complete Model Summary:")
    print(model.summary())
    
    return model, results

# Example usage:
# Assuming you have data loaded
# model, results = simple_linear_regression_analysis(df['age'], df['price'], 'Age', 'Price')
```

**How to interpret the results:** 
- **R-squared**: Proportion of variance in Y explained by X (higher is better, but context matters) 
- **F-statistic p-value**: Tests if the model is better than just using the mean of Y 
- **Slope p-value**: Tests if the slope is significantly different from zero 
- **Residual plot**: Should show random scatter around zero (no patterns)


## Multiple linear regression
```python
def multiple_linear_regression_analysis(X, y, feature_names=None, target_name="y"):
    """
    Comprehensive multiple linear regression analysis
    
    Parameters:
    - X: predictor variables (DataFrame or array)
    - y: response variable (Series or array)
    - feature_names: list of feature names (optional)
    - target_name: target variable name
    
    Returns:
    - Fitted model, results dictionary
    """
    # Prepare data
    if isinstance(X, pd.DataFrame):
        feature_names = X.columns.tolist()
    elif feature_names is None:
        feature_names = [f'X{i+1}' for i in range(X.shape[1])]
    
    X_const = sm.add_constant(X)
    
    # Fit model
    model = sm.OLS(y, X_const).fit()
    
    # Extract results
    results = {
        'coefficients': dict(zip(['intercept'] + feature_names, model.params)),
        'p_values': dict(zip(['intercept'] + feature_names, model.pvalues)),
        'r_squared': model.rsquared,
        'adj_r_squared': model.rsquared_adj,
        'f_statistic': model.fvalue,
        'f_pvalue': model.f_pvalue,
        'aic': model.aic,
        'bic': model.bic,
        'n_observations': model.nobs,
        'df_residuals': model.df_resid
    }
    
    # Print results
    print("🔍 Multiple Linear Regression Results")
    print("=" * 50)
    
    # Model equation
    equation_parts = [f"{results['coefficients']['intercept']:.4f}"]
    for feat in feature_names:
        coef = results['coefficients'][feat]
        sign = "+" if coef >= 0 else ""
        equation_parts.append(f"{sign}{coef:.4f}*{feat}")
    
    equation = f"{target_name} = " + " ".join(equation_parts)
    print(f"Equation: {equation}")
    
    print(f"\nModel Statistics:")
    print(f"R-squared: {results['r_squared']:.4f} ({results['r_squared']*100:.2f}% of variance explained)")
    print(f"Adjusted R-squared: {results['adj_r_squared']:.4f}")
    print(f"F-statistic: {results['f_statistic']:.4f} (p-value: {results['f_pvalue']:.6f})")
    print(f"AIC: {results['aic']:.2f}")
    print(f"BIC: {results['bic']:.2f}")
    
    # Coefficient interpretation
    print(f"\n📊 Coefficient Analysis:")
    print("-" * 40)
    for feat in feature_names:
        coef = results['coefficients'][feat]
        p_val = results['p_values'][feat]
        significance = "***" if p_val < 0.001 else "**" if p_val < 0.01 else "*" if p_val < 0.05 else ""
        print(f"{feat}: {coef:.4f} (p={p_val:.4f}) {significance}")
    
    # Overall model significance
    if results['f_pvalue'] < 0.05:
        print("\n✅ Overall model is statistically significant (F-test p < 0.05)")
    else:
        print("\n❌ Overall model is not statistically significant (F-test p ≥ 0.05)")
    
    # Generate diagnostic plots
    print("\n📈 Generating diagnostic plots...")
    comprehensive_model_diagnostics(model)
    
    # Check for multicollinearity
    if isinstance(X, pd.DataFrame):
        print("\n🔍 Multicollinearity Analysis:")
        vif_df = calculate_vif(X)
        interpret_vif(vif_df)
    
    # Full model summary
    print(f"\n📋 Complete Model Summary:")
    print(model.summary())
    
    return model, results
```


## Logistic regression
```python
def logistic_regression_analysis(X, y, feature_names=None, target_name="y"):
    """
    Comprehensive logistic regression analysis
    
    Parameters:
    - X: predictor variables (DataFrame or array)
    - y: binary response variable (Series or array)
    - feature_names: list of feature names (optional)
    - target_name: target variable name
    
    Returns:
    - Fitted model, results dictionary, predictions
    """
    # Prepare data
    if isinstance(X, pd.DataFrame):
        feature_names = X.columns.tolist()
    elif feature_names is None:
        feature_names = [f'X{i+1}' for i in range(X.shape[1])]
    
    X_const = sm.add_constant(X)
    
    # Fit model
    model = sm.Logit(y, X_const).fit()
    
    # Get predictions
    y_prob = model.predict(X_const)
    y_pred = (y_prob > 0.5).astype(int)
    
    # Calculate metrics
    metrics = classification_metrics(y, y_pred, y_prob)
    
    # Extract results
    results = {
        'coefficients': dict(zip(['intercept'] + feature_names, model.params)),
        'p_values': dict(zip(['intercept'] + feature_names, model.pvalues)),
        'odds_ratios': dict(zip(['intercept'] + feature_names, np.exp(model.params))),
        'pseudo_r_squared': model.prsquared,
        'log_likelihood': model.llf,
        'aic': model.aic,
        'bic': model.bic,
        'n_observations': model.nobs,
        **metrics
    }
    
    # Print results
    print("🔍 Logistic Regression Results")
    print("=" * 50)
    
    print(f"Model Statistics:")
    print(f"Pseudo R-squared: {results['pseudo_r_squared']:.4f}")
    print(f"Log-likelihood: {results['log_likelihood']:.2f}")
    print(f"AIC: {results['aic']:.2f}")
    print(f"BIC: {results['bic']:.2f}")
    
    print(f"\n📊 Classification Metrics:")
    print(f"Accuracy: {results['accuracy']:.4f}")
    print(f"Precision: {results['precision']:.4f}")
    print(f"Recall (Sensitivity): {results['recall']:.4f}")
    print(f"Specificity: {results['specificity']:.4f}")
    print(f"F1-Score: {results['f1_score']:.4f}")
    
    # Coefficient and odds ratio interpretation
    print(f"\n📈 Coefficient Analysis:")
    print("-" * 60)
    print(f"{'Variable':<15} {'Coefficient':<12} {'Odds Ratio':<12} {'P-value':<10} {'Significance'}")
    print("-" * 60)
    
    for feat in ['intercept'] + feature_names:
        coef = results['coefficients'][feat]
        odds_ratio = results['odds_ratios'][feat]
        p_val = results['p_values'][feat]
        significance = "***" if p_val < 0.001 else "**" if p_val < 0.01 else "*" if p_val < 0.05 else ""
        print(f"{feat:<15} {coef:<12.4f} {odds_ratio:<12.4f} {p_val:<10.4f} {significance}")
    
    # Interpretation of odds ratios
    print(f"\n🎯 Odds Ratio Interpretation:")
    for feat in feature_names:
        odds_ratio = results['odds_ratios'][feat]
        if odds_ratio > 1:
            print(f"• {feat}: {((odds_ratio-1)*100):.1f}% increase in odds per unit increase")
        elif odds_ratio < 1:
            print(f"• {feat}: {((1-odds_ratio)*100):.1f}% decrease in odds per unit increase")
        else:
            print(f"• {feat}: No effect on odds")
    
    # Visualizations
    fig, ((ax1, ax2), (ax3, ax4)) = plt.subplots(2, 2, figsize=(15, 12))
    
    # ROC Curve
    auc_score = plot_roc_curve(y, y_prob, ax1)
    
    # Predicted probabilities distribution
    ax2.hist(y_prob[y == 0], bins=30, alpha=0.7, label='Class 0', color='red')
    ax2.hist(y_prob[y == 1], bins=30, alpha=0.7, label='Class 1', color='blue')
    ax2.set_xlabel('Predicted Probability')
    ax2.set_ylabel('Frequency')
    ax2.set_title('Distribution of Predicted Probabilities')
    ax2.legend()
    ax2.grid(True, alpha=0.3)
    
    # Confusion Matrix
    cm = np.array([[results['true_negatives'], results['false_positives']],
                   [results['false_negatives'], results['true_positives']]])
    sns.heatmap(cm, annot=True, fmt='d', cmap='Blues', ax=ax3,
                xticklabels=['Predicted 0', 'Predicted 1'],
                yticklabels=['Actual 0', 'Actual 1'])
    ax3.set_title('Confusion Matrix')
    
    # Residuals vs fitted (Pearson residuals)
    pearson_residuals = (y - y_prob) / np.sqrt(y_prob * (1 - y_prob))
    ax4.scatter(y_prob, pearson_residuals, alpha=0.6)
    ax4.axhline(y=0, color='r', linestyle='--')
    ax4.set_xlabel('Fitted Probabilities')
    ax4.set_ylabel('Pearson Residuals')
    ax4.set_title('Residuals vs Fitted')
    ax4.grid(True, alpha=0.3)
    
    plt.tight_layout()
    plt.show()
    
    # Full model summary
    print(f"\n📋 Complete Model Summary:")
    print(model.summary())
    
    return model, results, {'y_prob': y_prob, 'y_pred': y_pred}
```


## Time series analysis
```python
def time_series_decomposition(ts, model='additive', period=None, figsize=(15, 10)):
    """
    Perform seasonal decomposition of time series
    
    Parameters:
    - ts: time series data (pandas Series with datetime index)
    - model: 'additive' or 'multiplicative'
    - period: seasonal period (auto-detected if None)
    - figsize: figure size
    
    Returns:
    - Decomposition object
    """
    print("🕒 Time Series Decomposition Analysis")
    print("=" * 40)
    
    # Perform decomposition
    decomposition = seasonal_decompose(ts, model=model, period=period)
    
    # Plot decomposition
    fig, axes = plt.subplots(4, 1, figsize=figsize)
    
    decomposition.observed.plot(ax=axes[0], title='Original Time Series')
    decomposition.trend.plot(ax=axes[1], title='Trend Component')
    decomposition.seasonal.plot(ax=axes[2], title='Seasonal Component')
    decomposition.resid.plot(ax=axes[3], title='Residual Component')
    
    for ax in axes:
        ax.grid(True, alpha=0.3)
    
    plt.tight_layout()
    plt.show()
    
    # Summary statistics
    print(f"\nDecomposition Summary ({model} model):")
    print(f"• Original series variance: {ts.var():.4f}")
    print(f"• Trend variance: {decomposition.trend.var():.4f}")
    print(f"• Seasonal variance: {decomposition.seasonal.var():.4f}")
    print(f"• Residual variance: {decomposition.resid.var():.4f}")
    
    # Seasonal strength
    if model == 'additive':
        seasonal_strength = 1 - (decomposition.resid.var() / (decomposition.seasonal + decomposition.resid).var())
    else:
        seasonal_strength = 1 - (decomposition.resid.var() / (decomposition.seasonal * decomposition.resid).var())
    
    print(f"• Seasonal strength: {seasonal_strength:.4f}")
    
    return decomposition

def arima_analysis(ts, max_p=3, max_d=2, max_q=3, seasonal=False, m=12):
    """
    Automated ARIMA model selection and fitting
    
    Parameters:
    - ts: time series data
    - max_p, max_d, max_q: maximum orders to test
    - seasonal: whether to include seasonal terms
    - m: seasonal period
    
    Returns:
    - Best fitted model, order selection results
    """
    print("📈 ARIMA Model Selection and Analysis")
    print("=" * 40)
    
    # Grid search for best parameters
    best_aic = np.inf
    best_order = None
    best_seasonal_order = None
    results = []
    
    # Generate parameter combinations
    p_values = range(0, max_p + 1)
    d_values = range(0, max_d + 1)
    q_values = range(0, max_q + 1)
    
    if seasonal:
        seasonal_combinations = [(0,0,0,0), (1,0,0,m), (0,1,0,m), (0,0,1,m), (1,1,0,m), (1,0,1,m), (0,1,1,m), (1,1,1,m)]
    else:
        seasonal_combinations = [(0,0,0,0)]
    
    print("🔍 Testing parameter combinations...")
    
    for p, d, q in product(p_values, d_values, q_values):
        for seasonal_order in seasonal_combinations:
            try:
                model = ARIMA(ts, order=(p, d, q), seasonal_order=seasonal_order)
                fitted_model = model.fit()
                
                results.append({
                    'order': (p, d, q),
                    'seasonal_order': seasonal_order,
                    'aic': fitted_model.aic,
                    'bic': fitted_model.bic,
                    'log_likelihood': fitted_model.llf
                })
                
                if fitted_model.aic < best_aic:
                    best_aic = fitted_model.aic
                    best_order = (p, d, q)
                    best_seasonal_order = seasonal_order
                    best_model = fitted_model
                    
            except:
                continue
    
    # Results summary
    results_df = pd.DataFrame(results).sort_values('aic')
    
    print(f"\n🏆 Best Model: ARIMA{best_order}")
    if seasonal and best_seasonal_order != (0,0,0,0):
        print(f"   Seasonal: {best_seasonal_order}")
    print(f"   AIC: {best_aic:.2f}")
    
    print(f"\n📊 Top 5 Models:")
    print(results_df.head())
    
    # Diagnostic plots for best model
    fig, axes = plt.subplots(2, 2, figsize=(15, 10))
    
    # Residuals
    residuals = best_model.resid
    axes[0,0].plot(residuals)
    axes[0,0].set_title('Residuals')
    axes[0,0].grid(True, alpha=0.3)
    
    # Q-Q plot
    from scipy import stats
    stats.probplot(residuals, dist="norm", plot=axes[0,1])
    axes[0,1].set_title('Q-Q Plot of Residuals')
    
    # ACF of residuals
    from statsmodels.graphics.tsaplots import plot_acf, plot_pacf
    plot_acf(residuals, ax=axes[1,0], lags=20)
    axes[1,0].set_title('ACF of Residuals')
    
    # PACF of residuals  
    plot_pacf(residuals, ax=axes[1,1], lags=20)
    axes[1,1].set_title('PACF of Residuals')
    
    plt.tight_layout()
    plt.show()
    
    # Model summary
    print(f"\n📋 Model Summary:")
    print(best_model.summary())
    
    return best_model, results_df

```


## Autocorrelation linear process 
calculates $p(h)$ autocorrelation at any lag $h$ assuming white noise variance $variance = 1$


for example $$X_k = W_k + \frac{1}{2} W_{k-1} - \frac{2}{3} W_{k-2}$$
autocorrelation_linear_process([1, 1/2, -2/3], 1)

```python 
def autocorrelation_linear_process(coeffs, lag):
    """
    Compute the autocorrelation at given lag for the process
    X_k = sum_{i=0}^p a_i * W_{k-i},
    where W_k is white noise with variance 1.

    Args:
        coeffs (list or array): coefficients [a0, a1, ..., ap]
        lag (int): lag h for autocorrelation

    Returns:
        float: autocorrelation rho(h)
    """
    p = len(coeffs) - 1

    # Variance Var(X_k) = sum of squares of coefficients
    var_X = sum(a ** 2 for a in coeffs)

    # Covariance Cov(X_k, X_{k-lag})
    # Only terms where indices overlap: sum over i of a_i * a_{i+lag}
    cov = 0.0
    for i in range(p + 1):
        j = i + lag
        if j <= p:
            cov += coeffs[i] * coeffs[j]

    # Autocorrelation = covariance / variance
    rho = cov / var_X
    return rho


# Example: your original problem coefficients
coefficients = [1, 0.5, -2/3]

lag_1_autocorr = autocorrelation_linear_process(coefficients, 1)
print(f"Autocorrelation at lag 1: {lag_1_autocorr:.6f}")

```



![[Pasted image 20250624201246.png]]
A: Moving Average
B: Auto regressive
C: Random Walk
D: White noise


![[Pasted image 20250624202651.png]]
**(A) MA, (B) AR, (C) RW, (D) WN**

![[Pasted image 20250624203000.png]]

Based on the analysis:

1. **Is the process autoregressive?** Yes, because the ACF tails off exponentially and the PACF cuts off.
    
2. **Estimate the model order p:** The PACF cuts off after lag 2. This means that the partial autocorrelations are significant at lags 1 and 2, and then become non-significant from lag 3 onwards.
    

Therefore, the process appears to be an **Autoregressive (AR) process of order 2**, or **AR(2)**.

