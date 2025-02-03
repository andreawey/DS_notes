- MLP's can be used for regression tasks 
- If we want to predict a single value -> you just need a single output neuron: its output is the predicted value
- For multivariate regression -> you need one output neuron per output dimension 


<h2>Output layer</h2>
- When building an MLP for regression ==non activation function is needed for the output neurons==, so they are free to output any range of values
- However ReLU can be used in the output layer if we want to guarantee a posive output 
- Logistic function or hyperbolic tangent can be used in the output layer if we want that the predictions will fall within a given range of values 
	- we scale the labels to the appropriate range [0,1] for the logistic function, or [-1, 1] for the hyperbolic tangent


<h2>Loss function</h2>
- Mean Squared Error: typical loss function (to use during training)
- Mean absolute error might be used if we have a lot of outliers in the training set
- Huber loss is an alternative it is a combination of both

