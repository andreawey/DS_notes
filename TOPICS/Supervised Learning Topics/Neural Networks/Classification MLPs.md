- MLPs can also be used for classification tasks
- For a binary classification problem a single output neuron is needed using the logistic activation function 
	- the output will be a number in range [0,1] which we can interpret as the estimated probability of the positive class 
	- The estimated probability of the negative class is equal to (1-positive)
- MLPs can also easily handle multilabel binary classification tasks
	- we need multiple output neurons all using a logistic activation function
	- one output neuron for each positive class
	- note that the output probabilities do not necessarily add up to one -> the model might output any combination of labels


<h2>Multiclass classification </h2>
- If each instance can belong to only a single class out of 3 or more possible classes we need to have one output neuron per class and we should use the softmax activation function for the whole output layer
- The softmax function will ensure that all the estimated probabilities are in range [0, 1] and that they add up to one

![[Classification MLP example.png]]

<h2>Loss function</h2>
Since we are predicting probability distributions the cross-entropy (log loss) is generally a good choice
