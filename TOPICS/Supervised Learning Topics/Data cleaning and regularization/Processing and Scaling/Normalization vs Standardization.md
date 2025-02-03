
Normalization:
- Good option when you know that your data does not follow a Gaussian distribution
- Good option in algorithms that do not assume any distribution of the data (ex: [[k-Nearest Neighbors Classifier]] and [[TODO/Neural Networks]])
Standardization:
- might be a good option when the data follows a Gaussian distribution
- it does not have a bounding range: outliers will not be affected by standardization


> It is important to apply exactly the same transformation to the training set and the test set of the supervised model
> ![[ImproperlyScaledData.png]]