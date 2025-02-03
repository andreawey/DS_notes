The perceptron is one of the simplest ANN architectures
It is based on an artificial neuron called a Threshold logic unit (TLU)
or sometimes a linear threshold unit (LTU)

![[perceptron.png]]
The inputs and outputs are numbers
Each input connection is associated with a weight

<h1>The Perceptron</h1>
The TLU computes a weighted sum of its inputs
then it applies a step function to that sum and outputs the results

<h1>Step functions</h1>
The most common step functions used in perceptrons are the heaviside step function and Sign function

$heaviside(z) \begin{cases} 0 & \text{if } z < 0 \\ 1 & \text{if } z \geq 0 \end{cases}$

$sgn(z)=\begin{cases} -1 & \text{if } z < 0 \\ 0 & \text{if } z = 0 \\ 1& \text{if } z \geq 0 \end{cases}$


- A single TLU can be used for simple linear binary classification
- It computes a linear combination of the inputs and if the result exceeds a threshold it outputs the positive class or else outputs the negative class (just like Logistic Regression Classifier)
- Training a TLU means finding the right values for the weights 

- A perceptron is simply composed of a single layer of TLUs with each TLU connected to all the inputs
- When all the neurons in a layer are connected to every neuron in the previous layer it is called a **fully connected layer or dense layer**
- To represent the fact that each input is sent to every TLU, it is common to draw special passthrough neurons called **input neurons** they just output whatever input they are fed
- All the input neurons form the **input layer**
- Moreover an extra bias feature is generally added $(x_0 = 1)$ it is typically represented using a special type of neuron called a **bias neuron** which just outputs 1 all the time

![[perceptron.png]]

It can classify instances simultaneously into three different binary classes, which makes it a multioutput classifier

The perceptron learning algorithm strongly resembles Stochastic Gradient Descent
-> Perceptron in fact is equivalent to a SGD with constant learning rate and without regularization

Contrary to logistic regression classifiers perceptrons do not output a class probability, rather they just make predictions based on a hard threshold.
This is one of the good reasons to prefer Logistic Regression over Perceptrons

Perceptrons are incapable of solving some trivial problems which is true also for other classification models


