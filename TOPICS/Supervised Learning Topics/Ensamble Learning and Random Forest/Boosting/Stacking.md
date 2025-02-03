[[Boosting]] technique

Stacked generalization instead of using trivial functions to aggregate the predictions of all predictors in an ensemble, it trains a model to perform this aggregation
![[Stacking.png]]


To train the blender a common approach is to use a hold-out set
- first the training set is split in two subsets. The first subset is used to train the predictors in the first layer
- The first layer predictors are used to make predictions on the second set

![[Blender.png]]

For each instance in the hold-out set there will be three predicted values
-> a new training set is created using these predicted values as input features and keeping the target values

The blender is trained on this new training set so it learns to predict the target value given the first layer's prediction




It is actually possible to train several different blenders this way
-> we get a whole layer of blenders

We split the training set into three subsets
- the first one is used to train the first layer
- the second one is used to create the training set used to train the second layer (using predictions made by the predictors of the first layer)
- the third one is used to create the training set to train the third layer (using the predictions made by the predictors of the second layer)
Prediction for new instances is done by going through each layer sequentially
