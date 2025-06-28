Computing word embeddings with neural nets
- Training a neural network language model, was observed to also yield learned word representations, which can be used for other potentially unrelated tasks
- Thus novel model architectures for efficiently computing continuous vector representations of words from - at that time- very large data sets were developed

# Word2Vec
## Computing word embeddings with word2vec
- The superficial classification task of predicting the most likely upcoming words is used to resolve the underlying objective
  i.e. to learn dense word embeddings that reflect semantic relationships
- The architecture used is a  [[Multi-Layer perceptron (MLP)]]  with three layers (input, hidden and output layer) the hidden layer uses a linear activation function!
- The dimensionality of word embeddings is defined by the size of the hidden layer and typically between 200 and 500
- Dense word embeddings are extracted from connections between input and hidden layer, connections between hidden and output layer represent context embeddings
- word2vec trains the neural network (much) more efficiently than Truncated SVD
![[Word2Vec.png]]

- the probability calculation is based on embedding similarity of center $v_c$ and context word $u_o$ expressed as inner product $v_c^T*u_o$ 
- The dot product "similarity" is translated into probabilities by applying the softmax function over all output words in the output layer $$P(o|c) = \frac{e^{v_c^T*u_o}}{\sum_{w\in V} e^{v_c^T*u_w}}$$

- Both word2vec models i.e. CBOW and Skip-Gram learn two embeddings for each word via W and W'. Word "i" is represented by the $i^th$ row of (a) W or (b) W + W'
- CBOW (Continuous Bag-of-words) predicts one center word
	- Continuous refers to the non-discrete representation used by the model 
	- context words on the input side are embedded into the hidden layer
	- embedded context words are averaged before projecting them to the output layer
- Skip-gram (SG) predicts several context words
	- Word pairs in training are not all proper bi-grams, but often involve "skipping over" some words 
	- Each context word on the output is treated equally in probability computations, the model only has one output layer
	- Skip-gram takes much longer to train! it produces many more training samples than CBOW

## Training Skip-gram word2vec model
- The skip-gram model maximizes the probability with the context set of $w, C(v)$  
  $$argmax_\theta \prod_{w \in Text}[\prod_{c \in C(w)}p(c|w, \theta)]$$
- Theoretical solution is found via gradient descent
	- Start from random weight matrices and consider the difference between desired and actual outputs over all training pairs
	- Compute the gradient for each parameter (weight) and back-propagate it to lower layers
	- Adjust coefficients of $W'$ then $W$ such that difference desired vs actual predictions is reduced
- The basic Skip-gram formulation defines $p(c|w, \theta)$ using the softmax function, which is computationally expensive, due to summation in the denominator
- The theoretical solution has further limitations:
	- It is impossible to perform gradient descent for all data at once
	- Training converges towards a degenerate solution
- We resolve this issue by adding for each positive sample, negative sample, s.t small fraction of model weights only are updated
	- Draw J random words w from the data, suppress influence of frequent words via $freq(w)^{3/4}$
	- Perform gradient descent for the data with positive and negative samples

## Skip-gram model with negative sampling (SGNS)
- Given positive and negative instances and initial embedding vectors, the training goal is to adjust word vectors with Maximum Likelihood Estimation
	- Maximize similarity drawn from the positive data for the target word, context word pairs $w, c_{pos}$ 
	- Minimize similarity drawn from the negative data for the target word, context word pairs $w, c_{neg}$
- In other words, maximize dot product of the word with the actual context word and minimize dot product of the word with the k negative sampled non-neighbor words

## Word2Vec - Design & Tuning considerations
- Dynamic window scaling 
	- Given a maximum context window of size C, a random window size in the range \[1, C] is selected, each time the context window is moved to the next word in a sentence -> more weight given to context closer to target word
- Subsampling frequent and irrelevant words
	- Frequent words are only kept in the training vocabulary with a certain probability; words which represent less than 0.26% of the total words will never be subsampled
	- Note: do not mix subsampling of the vocabulary with negative sampling adjustment based on $freq(w)^{3/4}$


# FastText
FastText extends word2vec to learn from less data and represent rare words and typos, without major architectural changes
- FastText enhances vocabulary by adding a collection of character n-grams of which a word is composed by default, n takes on values 3, ... 6
- At inference rare words not present in the training data are represented by the average of underlying n-grams
- FastText needs to learn substantially more token vectors, hence more time and memory ... and vectors for common words are not better necessarily 

>[!FAQ] Conclusions
>- The presented methods using co-occurrence statistics (LSA & GloVe) or neural prediction models (word2vec & fastText) all use large corpora's word distributions, taking into account local (LSA) or global context windows and considering distance-dependence (GloVe)
>- All of these methods yield static or non-contextual word embeddings 
>  i.e. a single vector for ambiguous words like "bank", "bat" or apple
>- Word embeddings revolutionize NLP applications like information retrieval IR, similarity analysis, word clustering, sentiment analysis, topic classification
>- Word embeddings can be used to contruct sentence & document embeddings

