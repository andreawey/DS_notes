This is another way to assess the generalization performance of a model

Plots of the model's performance on the training set and the validation set as a function of the training set size (or the training iteration)

To generate the plots, simply train the model several times on different sized subsets of the training set

<h2>Performance on the training data</h2>
- When there are just one or two instances in the training set, the model can fit them perfectly, which is why the curve starts at zero
- As new instances are added to the training set it becomes impossible for the model to fit the training data perfectly, both because the data is noisy and because it is not linear at all
- The error on the training goes up until it reaches a plateau -> at this point adding new instances to the training set does not make the average error much better or worse
<h2>Performance on the validation data</h2>
- When the model is trained on very few training instances, it is incapable of generalizing properly, which is why the validation error is initially quite big.
- Then as the model is shown more training examples, it learns and this the validation error slowly goes down
- A straight line cannot do a good job modeling the data so the error ends up at a plateau very close to the other curve


> [!NOTE] Overfitting
>- The error on the training data is much lower than with the linear regression model
>- There is a gap between the curves this means that the model performs significantly better on the training data than on the validation data -> Overfitting
>- One way to improve an overfitting model it to feed it more training data until the validation error reaches the training error


