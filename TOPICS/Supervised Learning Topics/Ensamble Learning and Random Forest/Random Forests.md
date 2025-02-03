- A random forest is an ensemble of decision trees generally trained via the bagging method, typically on a sample set whose dimension is equal to the size of the training set 
- The random forest algorithm introduces extra randomness when growing trees, instead of searching for the very best feature when splitting a node it searches for the best feature among a random subset of features
- This results in a greater tree diversity which trades a higher bias for a lower variance generally yielding an overall better model


<h2>Extra-trees</h2>
- When you are growing a tree in a Random Forest at each node only a random subset of the features is considered for splitting 
- It is possible to make trees even more random by also using random thresholds for each feature rather than searching for the best possible thresholds
- A forest of such extremely random trees is simply called an Extremely Randomized Tree ensemble (or Extra-Trees)
- Once again this trades more bias for a lower variance
- It makes Extra-Trees much faster to train than regular Random Forests since finding the best possible threshold for each feature at every node is one of the most time-consuming tasks of growing a tree


<h2>Feature Importance</h2>
- Random Forests make it easy to measure the relative importance of each feature
- A way to measure feature importance is by looking at how much tree nodes that use that feature reduce impurity on average


