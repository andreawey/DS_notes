Regularized version of [[Linear regression]]
The coefficients $\theta$ are chosen not only so that they predict well on the training data, but also to keep the magnitude of coefficients as small as possible

A regularization term is added to the cost function

The regularization term forces to keep the model weights close to zero
The regularization term should only be added to the cost function during training
-> once the model is trained, you want to evaluate the model's performance using the unregularized performance measure


The ridge regression model makes a trade-off between:
- the simplicity of the model 
- the performance on the training set
How much importance the model places on simplicity versus training set performance can be specified by the user, using the $\alpha$ parameter
The optimum setting of $\alpha$ depends on the particular dataset we are using


> [!NOTE] Note
> It is quite common for the cost function used during training to be different from the performance measure used for testing.
> - A good training cost function should have optimization-friendly derivatives.
> - The performance measure used for testing should be as close as possible to the final objective.

> [!WARNING] Attention
> It is important to scale the data before performing ridge regression as it is sensitive to the scale of the input features

