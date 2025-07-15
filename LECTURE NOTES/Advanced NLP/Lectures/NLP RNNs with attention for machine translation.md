RNNs in sequence-to-sequence processing
-  As LMs, RNNs can generate text auto-regressively 
	- Of the same genre as the training data
- RNNs can also represent input text 
	- Using final state, or average of all states
- What about combining an encoder using its final state with a decoder by conditioning it on the encoder's final state ?

## Seq2Seq RNN for machine translation
- MT = given an input sentence in one language generate an output sentence in another language
- Encoder-Decoder RNN
	- A conditional language model -> the decoder outputs a probability distribution for the next word of the output sentence, given the previous words and the encoder's representation of the entire input sentence
	  $$P(T|S)=P(t_1|S)*P(t_2|t_1, S)* ... * P(t_n|t_1, ..., t_{n-1}, S)$$
	- Input sentence (source S) output sentence (target T), with words $s_i$ (i=1, ..m) and $t_j$ (j = 1, ...n)
- For MT model training 
	- Use a parallel corpus
	- apply teacher forcing
	- end-to-end backpropagation in time

- Seq2Seq RNNs are useful for many other NLP tasks (eg summarization or dialogue)
	- single neural network to be optimized end-to-end (no individual subcomponents optimization)
	- no feature engineering ( no linguistic domain knowledge) same method for all language pairs
- Crucial limitation of seq2seq RNNs:
	- All source sentence content squeezed into encoder RNN' final state (bottleneck between encoder & decoder)
		- Translation quality decreases significantly with the length of the source sentence


## RNNs with attention
- Idea: on each step of the decoder, focus on a particular part of the source sequence
- Enable the decoder to access directly, with a combination of weights, each of the hidden states of the encoder
	- Weights = "attention" on the respective states
	- "attention" is a vector of size of the text span
	- bidirectional encoder | left-to-right decoder
- Generation is conditioned on a weighted average of all the encoder's hidden states . and the weights depend on the word to generate
![[Pasted image 20250701144307.png]]

![[Attention 1.png]]![[Attention 2.png]]![[Attention 3.png]]![[Attention 4.png]]

## Equations
- there are N hidden states of the encoder $h_1, ..., h_N \in R^H$
	- H is the dimension of the hidden layer
	- N is the length of the input sequence
- To generate token t of the output consider state $s_t \in R^H$ of the hidden layer of the decoder
	- Called "query" especially in attention-only models
- Attention scores for each hidden state of the encoder
  $$e_t = [s_t^T* h_1, ... s_t^T*h_N]\in R^N$$
- Attention distribution: probabilities derived from scores in $e_t$ using component-wise softmax $$\alpha_t=softmax(e_t)\in R^N$$
- Attention output ("context vector") at step t of the decoder: weighted sum of all encoder's hidden states
  $$a_t = \sum_{i=1}^N\alpha_{t,i} * h_i \in R^H$$
- The context vector is concatenated with the hidden state to form a new hidden state $[a_t, s_t] \in R^{2H}$ to be used instead of a regular RNNs $s_t$

![[Attention example.png]]

>[!FAQ] Attention
>- Computes a weighted sum of the values dependent on the query
>- results in a fixed-size representation of a set of representations (the values) dependent on another representation (the query)
>- Summarizes information contained in the values, with a focus selected by the query
>- The output mostly contains information from hidden states that received high attention
>
>- Improves NMT performance (the decoder focuses on parts of the source)
>- Solves the bottleneck problem and helps with the vanishing gradient problem (the decoder looks directly at the entire source and provides a shortcut to far away states)
>- Provides some interpretability (attention distribution reveals the decoder's focus)
>- Offers alignment for free w/o explicit training
>  
>  - RNNs with attention are limited w.r.t. parallelization
>  - Attention is applicable in many modalities, architectures and tasks


