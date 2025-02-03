
<h1>Multi-Layer Perceptron (MLP)</h1>
- ANN built stacking multiple perceptrons
- It eliminates some of the limitations of perceptrons (incapability of solving some trivial problems like XOR classification problem)

A multi layer perceptron is composed of 
- one (passthrough) input layer
- one or more layers of TLUs called hidden layers
- one final layer of TLUs called the output layer
![[multi layer perceptron.png]]

The layers close to the input layer are usually called the lower layers 
the ones close to the outputs are usually called the upper layers 
every layer except the output layer includes a bias neuron and is fully connected to the next layer


<h1>Artificial Neural Networks</h1>
- The signal flows only in one direction ( from the inputs to the outputs ) so this architecture is an example of a feedforward neural network (FNN)
- when an ANN contains a deep stack of hidden layers, it is called a deep neural network (DNN)
	- the field of Deep Learning studies DNNs and more generally models containing deep stacks of computations
	- However many people talk about deep learning whenever neural networks are involved (even shallow ones)

<h2>Backpropagation</h2>
Training a multi layer perceptron backpropagation training algorithm
- it consists in a gradient descent using an efficient technique for computing the gradients automatically: called automatic differentiation or autodiff
	- there are various autodiff techniques
	- The one used by backpropagation is called reverse-mode autodiff. It is fast and precise and is well suited when the functions to differentiate has many variables and few outputs
- In just two passes through the network (one forward and one backward) the backpropagation algorithm is able to compute gradient of the network's error with regards to every single model parameter.
- in other words it can find out how each connection weight and each bias term should be tweaked in order to reduce the error
- Once it has the gradients it just performs a regular gradient descent step and the whole process is repeated until the network converges to the solution

The algorithm :
- It handles one mini-batch at a time and it goes through the full training set multiple times 
  -> each pass is an **EPOCH**
- Each mini batch is passed to the network's input layer which just sends it to the first hidden layer. The algo then computes the output of all the neurons in this layer (for every instance in the mini-batch) The result is passed on to the next layer, its output is computed and passed to the next layer and so on until we get the output layer
  -> this is the **Forward pass** it is exactly like making predictions, except all intermediate results are preserved since they are needed for the backward pass
- Next the algo measures the network's output error 
- Then it computes how much each output connection contributed to the error
  -> This is done analytically by simply applying the chain rule which makes this step fast and precise
- The algorithm then measures how much of these error contributions came from each connection in the layer below, again using the chain rule, and so on until the algorithm reaches the input layer
- Finally the algorithm performs a gradient descent step to tweak all the connection weights in the network using the error gradient it just computed

> [!HINT] Summary
> - First makes a prediction (forward pass)
> - measures the error
> - then goes through each layer in reverse to measure the error contribution from each connection (reverse pass)
> - finally slightly tweaks the connection weights to reduce the error (Gradient Descent Step)



> [!CAUTION] Remarks
> - It is important to initialize all the hidden layers connection weights randomly or else training will fail
> - If all weights and biases are initialized to zero -> all neurons in a given layer will be perfectly identical and thus backpropagation will affect them in exactly the same way so they will remain identical 
> 	- that is despite having hundreds of neurons per layer, your model will act as if it had only one neuron per layer: it won't be too smart
> - If the weights are randomly initialized -> we break the symmetry and allow backpropagation to train diverse team of neurons






