When you have [[Missing Data|Missing Data]] you can:

Estimate missing values: reserves the data size
- Constant value imputation: replace with 
	- Mean (sensitive to noise)
	- Median (for numerical features)
	- Mode
	- Constant value (for categorical)
Ex: missing number of accidents -> replace with 0

- Numerical imputation replace with all possible values (weighted by their probabilities)
- Categorical imputation maximum occurred value or a default value

- Do nothing 
	- Algos can learn the best imputation values for the missing data based on the training loss reduction (ex: [[XGBoost]]) 
	- Some algos do just ignore the missing data (ex: [[LightGBM]])
	- Some algos do not accept missing values (ex: [[Linear regression]]) 

- Imputation using [[k-Nearest Neighbors Classifier]]: it finds the K's closest neighbors to the observation with missing data and then imputes them based on the non-missing values in the neighborhood (ex: weighted average on the distance). 

- Regression-based imputation: predict the missing values by regressing it from other relate variables 

- Cluster-based Imputation 
