[[Gradient Descent]] computes the gradient of the cost function with respect to each model parameter 

Partial derivative: we calculate how much the cost function changes if we change $\theta_j$ just a little bit.
We compute the partial derivative of the cost function with respect to the parameter $\theta_j$ as follows


![[Gradient Descent Learning Rate.png]]

To find a good learning rate you can use grid search 
- limit the number of iterations so that the grid search can eliminate models that take too long to converge
- if it is too low you will still be far away from the optimal solution when the algorithm stops
- If it is too high you will waste time while the model parameters do not change anymore

Another version is [[Stochastic Gradient Descent]]
