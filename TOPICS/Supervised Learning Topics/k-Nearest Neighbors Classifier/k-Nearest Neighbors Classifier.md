- It is the simplest ML algorithm 
- building the model consists of storing the training dataset
- making a prediction for a new data point consists in finding the closest data points in the training dataset
- Multiclass classification we count how many neighbors belong to each class and predict the most common class

<h1>one-nearest-neighbor</h1>
The simplest version of the k-NN algorithm considers exactly one nearest neighbor 
The prediction is simply the known output for this training point

<h1>K-Nearest Neighbors</h1>
with k neighbor voting is used to assign a label between the k nearest neighbors
it assigns the class that is more frequent: the majority class among the neighbors


A single neighbor -> decision boundary follows the training data closely
Considering more neighbors leads to a smoother decision boundary -> simpler model
Considering few neighbors corresponds to high model complexity
Considering many neighbors corresponds to low model complexity



[[R2 score]] is the coefficient of determination
it is a measure of goodness of a prediction for a regression model
yields a score between 0 and 1 
a value of 1 corresponds to a perfect prediction
a value of 0 corresponds to a constant model that just predicts the mean of the training set responses

<h1>Parameters</h1>
- Number of neighbors: this parameter needs to be adjusted, however very common values are 3 or 5 neighbors
- distance metric: euclidean distance is frequently used, which works well in many settings. However, it is possible to choose a different metric

<h1>Strenghts and weaknesses</h1>
- the model is very easy to understand 
- often provides reasonable performance without a lot of adjustments
- Using this algorithm is a good baseline method to try before considering more advanced techniques
- Building the nearest neighbors model is usually very fast but when your training set is very large prediction can be slow
- When using the k-NN algorighm it's important to preprocess your data
- This approach often does not perform well on datasets with many features, and it does particularly badly with datasets where most features are 0 most of the time