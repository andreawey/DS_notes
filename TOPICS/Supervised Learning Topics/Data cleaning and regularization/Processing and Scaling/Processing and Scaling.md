
In most cases the numerical features of the dataset do not have the same values range

- Ex: age and income do not have the same scale

Some algorithms (ex: [[TODO/Neural Networks]] , [[Support Vector Machines]] or [[Distance Based Algorithms]] algos like [[k-Nearest Neighbors Classifier]] or k-Means) are very sensitive to the scaling of the data 
- from a ml point of view columns with different values range might be difficult to compare
- a common practice is to adjust the features so that the data representation is more suitable for these algorithms
- often a simple pre-feature rescaling and shift of the data is enough

Scaling solves this problem: All the continuous features are rescaled in order to have the same values range.

! Features values must be represented int he same way as in the training set and the test set


![[UnscaledData.png]]



Steps to take:

- [[Normalization]]
- [[Normalization vs Standardization]]
- [[Standardization]]
- [[Robust Standardization]]

