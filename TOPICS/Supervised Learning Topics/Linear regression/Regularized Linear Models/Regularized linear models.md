A good way to reduce overfitting is to regularize the model (by constraining it)
- the fewer degrees of freedom it has, the harder it will be for it to overfit the data
- a less complex model will have worse performance on the training set but better generalization

For linear models regularization is typically achieved by constraining the weights of the model using 
- [[Ridge Regression]]
- [[Lasso Regression]]

<h1> Lasso vs Ridge</h1>
- In practice Ridge regression is usually the first choice between these two models
- If you have a large amount of features and expect only a few of them to be important Lasso might be the better choice
- Similarly if you would like to have a model that is easy to interpret Lasso will provide a model that is easier to understand as it will select only a subset of the input features.

