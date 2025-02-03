The step function contains only flat segments -> there is no gradient to work with (gradient descent cannot move on a flat surface)

An alternative to the step function is the logistic function 

$\sigma(z)=\frac{1}{1+e^{-z}}$

The logistic function has a well-defined nonzero derivative everywhere allowing gradient descent to make some progress at every step [[Sigmoid Function]]

![[Sigmoid function.png]]


<h1>Activation Functions</h1>
The backpropagation algorithm works well with many other activation function apart from logistic regression
- the hyperbolic tangent function
- the rectified linear unit function

<h2>Hyperbolic tangent function</h2>
$tanh(z) = 2\sigma(2z) -1$

- it has an S-shaped continuous and differentiable function
- Its output values range is [-1, 1] which tends to make each layer's output more or less centered around 0 at the beginning of training -> this often helps speed up convergence.
![[Tanh function.png]]

<h2>Rectified Linear Unit function (ReLU)</h2>

$ReLU(z) = max(0,z)$

- it is continuous but not differentiable at z=0 (which can make GD bounce around)
- Its derivative is 0 for z < 0
- it works very well and has the advantage of being fast to compute 
- the fact that it does not have a maximum output value also heps reduce some issues during gradient descent
![[ReLU function.png]]

<h2>Softplus function</h2>
$softplus(z) = ln(1+e^z)$
- it is continuous and differentiable 
![[Softplus function.png]]


Why do we need activation functions ?
-> if you chain several linear transformations, all you get is a linear transformation
If you don't have some non-linearity between layers then even a deep stack of layers is equivalent to a single layer
-> you cannot solve very complex problems with that