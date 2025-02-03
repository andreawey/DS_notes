another type of [[Feature selection]] is model-based feature selection:

- A supervised ML model is used to evaluate the importance of each feature 
- This feature selection model does not need to be the same used for the final supervised modeling
- This feature selection model provides a measure of importance for each feature so that they can be ranked by this measure

- Decision tree based models: provide a feature importance score
- Linear models: the absolute value of the coefficients can be used to capture feature importance
	- Ex: linear models with L1 penalty learn sparse coefficients which only use a small subset of features


Model based feature selection considers all features at once, it can capture interactions
