Gradient Descent is an optimization algorithm used to find optimal solutions to a wide range of problems

The general idea of Gradient Descent is to tweak parameters iteratively in order to minimize a cost function

To find the minimum quickly it looks for the steepest slope

![[Gradient Descent.png]]

Start by filling $\theta$ with random values
Improve it gradually, taking one small step at the time, each step attempting to decrease the cost function (ex: MSE)
-> Step size = learning rate * slope 
Stop when the algorithm converges to a minimum

<h2>Learning Rate</h2>
Size of the steps is an important parameter in Gradient Descent
The step size is determined by the learning rate hyperparameter $\eta$
If $\eta$ is too small -> the algorithm will have to go through many iterations to converge, which will take a long time

if $\eta$ is too large -> the algorithm might jump across the valley and end up on the other side, possibly even higher up than it was before 
This might make the algorithm diverge, with larger and larger values failing to find a good solution



[[Gradient Descent]] at each step computes gradients based:
- [[Batch Gradient Descent]] on the full training set
- [[Stochastic Gradient Descent]] on just one instance
- Mini-batch Gradient Descent on small random sets of instances called mini-batches


Not all cost functions look like nice regular bowls. There may be holes, ridges, plateaus, making convergence to the minimum very difficult.

Challenge for GD 
- if the random initialization starts the algorithm on the left, then it will converge to a local minimum 
- if it starts on the right, then it will take a very long time to cross the plateau
- if the algorithm stops too early, it will never reach the global minimum

![[Challenge for Gradient Descent.png]]

Fortunately the MSE cost function for a linear regression model is a convex function
-> if we pick any two points on the curve, the line segment joining them never crosses the curve
-> there are no local minima, just one global minimum

It is also a continuous function with a slope that never changes abruptly

GD is guaranteed to approach arbitrarily close to the global minimum 


> [!WARNING] Attention
> When using Gradient Descent, we should ensure that all features have a similar scale, otherwise it might take much longer to converge



| Algorithm       | Large m | Out-of-core support | Large n | Hyperparams | Scaling required | SkLearn         |
| --------------- | ------- | ------------------- | ------- | ----------- | ---------------- | --------------- |
| Normal Equation | Fast    | No                  | Slow    | 0           | No               | No              |
| SVD             | Fast    | No                  | Slow    | 0           | No               | LinearRegressor |
| Batch GD        | Slow    | No                  | Fast    | 2           | Yes              | SGDRegressor    |
| Stochastic GD   | Fast    | Yes                 | Fast    | >=2         | Yes              | SGDRegressor    |
| Mini-Batch GD   | Fast    | Yes                 | Fast    | >=2         | Yes              | SGDRegressor    |

m= number of training instances
n= number of features


There is almost no difference after training: all these algos end  up with very similar models and make prediction in exactly the same way.
While the normal equation can only perform linear regression the gradient descent algos can be used to train many other models.
