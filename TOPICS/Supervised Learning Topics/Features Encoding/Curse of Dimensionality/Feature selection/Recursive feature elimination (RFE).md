Another type of [[Feature selection]] is Recursive feature elimination:

- RFE starts with all features, builds a model and discards the least important feature according to the model
- Then a new model is build using all but the discarded features
- This continues until a pre-specified number of features are left

A series of models are built -> RFE is more computationally expensive than other methods