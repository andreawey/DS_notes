
- Boosting refers to any Ensemble method that can combine several weak learners into a strong learner
- The general idea of most boosting methods is to train predictors sequentially, each trying to correct its predecessor
 ![[boosting.png]]
- There exist several boosting methodologies but the most popular are:
  - [[AdaBoost]]
  - [[Gradient Boost]]


<h2>Learning Rate</h2>
The learning rate hyperparameter scales the contribution of each tree
- if low -> we will need more trees in the ensemble to fit the training set, but the predictions will usually generalize better
- This is a regularization technique called shrinkage


<h2>Early Stopping</h2>
- In order to find the optimal number of trees, we can use early stopping
- It can be performed in two different ways
	- Training a large number of trees first and then looking back to find the optimal number
	- Stopping training early when the validation error does not improve for a defined number of consecutive times


<h1>Stochastic Gradient Boosting</h1>
[[Stochastic Gradient Boosting]]

<h1>Stacking</h1>
 [[Stacking]]
