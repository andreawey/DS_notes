<- Some regression algorithms can be used for classification as well
- Logistic regression is commonly used to estimate the probability that an instance belongs to a particular class
	- it is used as a binary classifier when prob >0.5 then label 1 else label 0

Logistic regression computes a weighted sum of the input features 

[[Sigmoid Function]]


<h1>Softmax regression</h1>

[[Softmax Regression]]


<h2>Cross Entropy</h2>
During training the objective is to have a model that estimates a high probability for the target class
Minimizing the cost function called the cross entropy should lead to this objective because it penalizes the model when it estimates a low probability for a target class
