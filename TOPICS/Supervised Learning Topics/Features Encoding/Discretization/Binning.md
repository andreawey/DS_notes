A common [[Discretization]] method is binning

The best way to represent data depends not only on the semantics of the data but also on the kind of model used
- #linear models can only model linear relationships
- Decision #tree can build more complex models
- One way to make linear models more powerful on continuous data is to use binning (aka discretization) of the features
Motivation:
- make the model more robust
- prevent overfitting
- it has a cost on the performance (making data more regularized)

![[Discretization.png]]

- We create a partition of the feature range generating a fixed number of bins
- A data point will be represented by which bin it falls into