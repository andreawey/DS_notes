
It ensures that for each feature the mean is 0 and the standard deviation is 1
$x_{new} = \frac{x-\mu}{\sigma}$

Useful for optimization algorithms (ex: [[Gradient Descent]], exploited for regression and [[TODO/Neural Networks]]) and for distance based algos (ex: [[k-Nearest Neighbors Classifier]]) 

+ It brings all features to the same magnitude
+ It ensures statistical properties for each feature that guarantee that they are on the same scale
+ It does not ensure any particular minimum and maximum values for the features

![[StandardScaler.png]]
