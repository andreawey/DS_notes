## Simple feedforward neural language models
- Standard feedforward neural network & Markov's assumption
- Input: sequence of preceding words; output: probability distribution over vocabulary
Important improvement of original idea
- Represent preceding words with their embeddings
- Better results than with words and better generalization to new data based on word similarity
Advantages over n-gram LMs
- No sparsity and storage problems, longer sequences/histories handled
- higher predictive accuracy than n-gram LMs (for the same size of training data)
Disadvantage:
- Much slower to train and more difficult to interpret

![[FFNN LM.png]]


## RNNs
- Strong limitation of feed-forward networks: fixed-size windows
	- Not long enough to model all dependencies
	- Size or W increases linearly with window size
- Idea:
	- Present one word at a time, memorize entire past through a hidden layer, which influences itself at the next step
- Similarities to feedforward networks
	- Multiply input $x_t$ by weight matrix, pass it through an activation function, obtain activations of hidden layer, use them to calculate output $y_t$
- Key difference: "recurrent" link
	- Augments input to the hidden layer with the value of the same layer from the preceding point in time
	- new set of weights connect the hidden layer from the previous time-step to the current one -> indicate how to use "the past" to compute the current output
	- Is a form of memory or context that encodes all preceding information, i.e. allows the past to go back to the beginning of the series
	- Avoids usage of fixed size window


## Alternative formulations
- Dimension of input, hidden, output layers of dimension: $d_{in}, d_h, d_{out}$
- Dimensions of matrices: W is $d_h x d_{in}$, U is $d_h, x d_h$, V is $d_{out}xd_h$
  $$h_t = g(U h_{t-1}+Wx_t)$$
$$y_t = f(Vh_t)$$
- For classification, obtain a probability distribution over output classes as 
  $$y_t=softmax(V h_t)$$
- Incremental inference algorithm: from the start of the sequence to its end
- Graphical "unfolding" or "unrolling" of network in time
![[Graphical unfolding of network in time.png]]

- Training
	- Recurrent connections weights are trained as the other weights in the network
	- Matrices, W, U, V shared across time steps
	- Method: "backpropagation through time" (BPTT)

## RNN adaptation for LMs
- Input to LM RNN are word embeddings
	- Computed via weight matrix $E\in R ^{d_hx|vocab|}$
- The rest is similar to a feedforward NN LM 
  $$e_t = E xt$$
  $$h_t= g(Uh_{t-1}+W_{e_t})$$
  $$y_t = f(V h_t)$$
- The probability of that the next word is $w_i$ from the vocabulary is component i of 
  $$y^i_t = P(w_{t+1}=w_i|w_{1:t})$$
- The probability of a given sequence of words is simply the product of the probabilities of each word in the sequence
![[RNN adaptation for LMs.png]]

- Advantages 
	- Constant model size for any size inputs
	- same weights applied for each timestep integrating long-range time information
- Disadvantages
	- slow training and difficulty to acess to "remote past" in practice


## Self-supervised training of RNN LMs
- Backpropagation in time
	- Derivative of the loss with respect to the repeated (multiplied) weight matrix W
- Objective function: minimize CE-loss
	- Difference between predicted $\hat{y}$ and ground truth y probability distribution 
	  $$L_{CE}(y^t, \hat{y}^t)= - \sum_{w\in V}y^t_w* log(\hat{y}^t_w)$$
	- Since the correct distribution equals 1 for the actual next word only, this simplifies to $$L_{CE}(y^t, \hat{y}^t)=-log(\hat{y}^t_w)$$
- Teacher forcing
	- Use ground truth of previous steps, instead of the model's (imperfect) outputs
- Stochastic Gradient Descent
	- Computing loss & gradients on entire corpus is too expensive -> use batches of sentences
- Weights tying
	- The input embedding matrix E and the final layer matrix V are of similar "shape" i.e. size of vocabulary times $d_h$
	- Forcing $E^T = V$ improves perplexity & reduces the number of parameters
![[Self supervised training of RNN LMs.png]]

- BPTT is computationally expensive: if input sequences have hundreds of timesteps, then hundreds of derivative computations are required for a single weight update
  -> this would cause learning to be slow and weights to vanish or explode
- Since training across entire corpus too expensive, sentences or documents are considered
![[Self-supervised training.png]]

## Training issues: vanishing & exploding gradients
- Vanishing gradients
	- Gradients for remote steps in the past are much smaller than recent ones
	- Model weights capture well close-range, not wide-range effects
	- RNN-LMs are better at learning from "sequential recency than from syntactic recency"
- Exploding gradients
	- Gradients (close & far) may become too big
	- Then the SGD update step becomes too big, shooting far away from the optimal region
	- May even result in numeric overflows or underflows in the parameters
	- Apply gradient clipping .. or lower learning rate

## LSTM: a solution to vanishing gradients
- hidden states and cell states are vectors of length $d_h$
- Cells store long-term information, which can be read/written/erased from t to t+1
- gates vectors of length $d_h$ select which information is read/written/erased
Advantages
- Information is better preserved over time
  -> easier to learn long-distance dependencies
Note: LSTMs introduce a considerable number of additional parameters and do not fully resolve the vanishing or exploding gradients issue

