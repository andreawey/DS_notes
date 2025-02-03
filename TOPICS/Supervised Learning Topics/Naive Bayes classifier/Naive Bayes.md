Family of classifiers based on Bayes theorem

- they tend to be faster in training with respect to linear models
	- they learn parameters by looking at each feature individually
	- they collect simple pre-class statistics from each feature
- Easy to build and particularly useful for very large dataset
- Might outperform even highly sophisticated classification methods
- often provide generalization performance slightly worse than linear classifiers



- All classifiers of this family assume independence among predictors
	- the presence of a particular feature in a class is unrelated to the presence of any other feature
	- Even if the features depend on each other all of these properties independently contribute to the probability that a sample belongs to a specific class c


There exist different types of Naive Bayes classifiers

<h2>Bernoulli</h2>
- it assumes binary data
- counts how often every feature is not zero for each class

<h2>Multinominal</h2>
- it assumes count data
- takes into account the average value of each feature for each class

<h2>Gaussian</h2>
- can be applied to any continuous data
- it assumes that features follow a normal distribution
- stores the average value as well as the standard deviation of each feature for each class


Prediction
- a data point is compared to the statistics for each of the classes 
- the best matching class is predicted

- Gaussian is mostly used on very high dimensional data
- Multinominal and Bernoulli are widely used for sparse count data
- Multinominal usually performs better than Bernoulli on datasets with a relatively large number of nonzero features


<h2>Parameters</h2>
- Multinominal and Bernoulli have a single parameter $\alpha$ which controls model complexity
- The way $\alpha$ works is that the algorithm adds to the data $\alpha$ many virtual points that have positive values for all the features
- The algo's performance is relatively robust to the setting of alpha, meaning that setting alpha is not critical for good performance
- tuning $\alpha$ usually improves accuracy somewhat

<h2>Strengths</h2>
- NB models are very fast to train and to predict
- the training procedure is easy to understand
- The models work very well with high-dimensional sparse data
- The models are relatively robust to the parameters
- NB models are great baseline models and are often used on very large datasets when training even a linear model might take too long
- When assumption of independence holds, a NB classifier performs a better compare to other models like logistic regression and you need less training data
- It performs well in case of categorical input variables compared to numerical variables 

<h2>Weaknesses</h2>
- if a categorical variable has a category which was not observed in the data set, then the model will assign a 0 probability and will be unable to make a prediction 
- NB is also known as a bad estimator so the probability outputs are not to be taken too seriously 
- Another limitation of NB is the assumption of independent predictors: in real life it is almost impossible that we get a set of predictors which are completely independent