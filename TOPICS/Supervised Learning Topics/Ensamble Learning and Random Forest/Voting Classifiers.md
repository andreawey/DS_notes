<h1>Hard Voting Classifiers</h1>
- Suppose you have trained some classifiers, each one achieving about 80% accuracy
- A very simple way to create an even better classifier is to aggregate the predictions of each classifier and predict the class that gets the most votes
- This majority vote classifier is called a hard voting classifier

- Often achieves a higher accuracy than the best classifier in the ensemble
- Even if each classifier is a weak learner, the ensemble can still be a stronger learner
- Ensamble learning works if all classifiers are perfectly independent making uncorrelated errors, which is clearly not the case when they are trained on the same data
- Ensemble methods work best when the predictors are independent from one another as possible. One way to get diverse classifiers is to train them using very different algorithms. This increases the chance that they will make very different types of errors improving the ensemble's accuracy.


<h2>Voting types</h2>
- Hard voting: majority vote prediction
- Soft voting: predict the class with the highest class probability, averaged over all the individual classifiers
- Soft voting often achieves higher performance than hard voting because it gives more weight to highly confident votes.


<h2>Diverse set of classifiers</h2>
- Ways to get a diverse set of classifiers
	- Use very different training algorithms
	- Use the same training algorithm for every predictor, but train them on different random subsets of the training set
		- Bagging: when sampling is performed with replacement
		- Pasting: when sampling is performed without replacement


<h2>Bagging and Pasting</h2>
- Both bagging and pasting allows training instances to be sampled several times across multiple predictors
- Only bagging allows training instances to be sampled several times for the same predictor
- Once all predictors are trained the ensemble makes prediction by aggregating the predictions of all predictors 
- The aggregation function is typically the statistical mode for classification or the average for regression
- Each individual predictor has a higher bias than if it were trained on the original training set but aggregation reduces both bias and variance
- In general the ensemble has a similar bias but a lower variance than a single predictor trained on the original training set
- Both training and prediction can be performed in parallel


With bagging some instances may be sampled several times for any given predictor while others may not be sampled at all
The training instances are not sampled are called out-of-bag instances
A predictor never sees the out of bag instances during training ->it can be evaluated on these instances without the need for a separate validation set
We can evaluate the ensemble itself by averaging out of the bag evaluations of each predictor


- Random Patches method: sampling both training instances and features
- random Subspaces method: keeps all training instances but sampling features
- Sampling features result in even more predictor diversity, trading a bit more bias for a lower variance