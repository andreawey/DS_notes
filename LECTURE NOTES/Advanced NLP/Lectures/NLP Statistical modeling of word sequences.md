# PoS tagging
- PoS tagging used to be a basic step in any NLP pipeline
- Today's end-to-end approaches don't use it explicitly... but they infer it implicitly 
- Understanding PoS tagging at the basic level is very useful in many ways
	- Understanding the challenges of NLP more in depth
	- Making sense of what happens in complex models
	- Making sense of model errors

## PoS tagging naive solution
- Look up the part of speech in a dictionary
	- Noun vs Verb vs Adjective vs Adverb vs Pronoun vs Conjunction vs Preposition ...
	- It actually works for about 85% of dictionary words
	- Unfortunately there are many common words in the other 15%
	- That's because a lot of common words are ambiguous

# PoS tagging baseline solution
What's the simplest way to resolve PoS tagging ambiguities?
- For each ambiguous word token, choose the tag that is most frequent for that word token in the training corpus
In PoS tagging, this method gets us to roughly 90% accuracy on large English corpora!
- Compare that to the 97% accuracy of state-of-the-art PoS taggers
- Compare that to the 98% uppoer bound

## How to PoS tag?
The baseline shows us the best we can do with next to zero effort.

For a new language 
- Step 0: Define a POS tag set
- Step 1: Annotate a corpus with these tags
For a well-studied language
- Step 1: Obtain a PoS-tagged corpus
For any language
- Step 2: Choose a PoS tagging model
- Step 3: Train your model on your training corpus
- Step 4: Evaluate your model on your test corpus

## PoS as sequence classification
A sentence in a sentence of observations
What is the most probable sequence of tags corresponding to the sentence of observations ?

Probabilistic approach:
- Look at all the possible tag sequences
- Choose the most likely tag sequence
In practice:
- Classify each token independently 
- Use information about the surrounding tokens as input features
- The best kind of information are the PoS tags, but they're exactly what we're trying to find!
	- We can use the tags of the preceding tokens as we go forward ...
	- and the tags of the successive tokens if we go back from the end of the sentence


## Markov Chains
Models that tell us about the probabilities of a sequence of random variables
- A set of N state variables
- a transition probability matrix A where $a_{i.j}$ is the probability of going from state i to state j
- an initial probability distribution over the N states
	- This is what we know a priori before observing the system
	- $\pi_i$ is the probability that the chain will start in state i

- Key assumption: the next state only depends on the current state 
- For word sequences, this means we assume that the next word only depends on the current word (bigram language model)

>[!FAQ] Markov Chains example
>![[Markov Chain Example.png]]
>- N = 3 state variables: the words uniformly, are, charming
>- the arrow weights are the transition probabilities
>- the initial distribution over the states is not shown; it would tell us how likely we are to begin in each of the  3 states


## Hidden Markov Models
Can we do PoS tagging with Markov Chains ?
- Not quite, a MC works when you're interested in the probability of observed events
- In PoS tagging we want the probability of things we can't observe directly: the tags, which are hidden

HMMs enable us to model sequences of both observed events (words) and related hidden events (tags)
- A set of N states
- a NxN transition matrix A where $a_{i,j}$ is the probability of going from state i to state j
- A sequence O of T observations (drawn from a set of V possible observations) $O = o_1, o_2 , ... o_T$
- a NxV emission matrix B where $b_{i,t} = P(o_t|i)$ is the probability of state i generating observation $o_t$
- An initial probability distribution over the N states

>[!INFO] HMM as the fair bet casino
>![[HMM casino.png]]
>- The hidden states are Fair and Biased
>- the possible observations are Heads and Tails
>- When the system is in the Fair state, it is equally likely to generate Heads or Tails
>- when the system is in the Biased state, it is biased towards Heads


## HMM assumptions
- Markov assumption: the probability of a tag only depends on the previous tag
- Output independence: the probability of a word depends only on the tag that corresponds to it
- The markov assumption is why we use the transition matrix A
- The output independence assumption is why we use the emission matrix B
- The transition and emission probabilities are estimated as counts based on corpora
  $a_{0,1} = P_{VBP|PRP} \approx \frac{\text{number of PRP VBP}}{\text{number of PRP}}$
  $b_{1,2} = P_{old|VBP} \approx \frac{\text{number of old tagged as VBP}}{\text{number of VBP}}$


>[!FAQ] HMM in a nutshell
>Let W be an observed word sequence and T be a hidden tag sequence
>What we want is $$\theta{T} = argmax P(T|W) = argmaxP(W|T)P(T) =  arg max \prod_{i}P(w_i|t_i)\prod_{i}P(t_i|t_{i-1})$$
>over all possible tag sequences
>
>The process we use to discover the tag sequence that maximizes P(W|T)P(T) is called decoding and can be carried out using the Viterbi algorithm


## Viterbi Decoding
The HMM may be represented as a graph called a trellis with a row for each tag and a column for each word

![[Viterbi.png]]

- The i-th column corresponds to the i-th observation (word)
- The k-th row corresponds to the k-th hidden state (tag)
- We want to find the most probable path through the trellis 
- Which means the path with the highest end-to-end probability, computed as the products of the arrow weights along the path
- The weight of the arrow from circle (k,i) to circle (j,i+i) is the product of two probabilities: 
	- The probability of going from state k to state j: $a_{k,j} = P(j|k)$
	- The probability of state j generating word i + 1: $b_{j,i+1}=P(i+1|j)$
- The path (state sequence) through the trellis with the highest end-to-end probability is the most likely tag sequence associated to the word sequence


## Dependency Parsing
![[Dependency Parsing.png]]
- Dependencies are (labeled) asymmetric relationships between two lexical items
- Today they get implicitly figured out by machine learning systems
- Nevertheless, it's useful to understand such relationships and how they can be explicitly computed algorithmically

>[!FAQ] Dependency Parsing Example
>![[Dependency Parsing Example.png]]
>- If there is a main verb, it serves as the root of the sentence. Here the root is eats
>- If there is no main verb, the nominal predicate becomes the root. Example: I am a student -> student is the root

### Attachment ambiguity
Example: *I saw the man with a telescope*
- Saw is the root
- I and man depend on the root
- If I used a telescope to see the man, then telescope also depends on the root
- If the man was holding a telescope then telescope depends on man
Other example: *Scientists count whales from space*


## Arc-standard transition-based parser
- At any given time, the parser has a configuration:
	- A stack of words (top to the right)
	- A buffer of words (top to the left)
	- A (partial) dependency graph
- The parser begins in an initial configuration
	- stack: ROOT symbol
	- buffer: all words
	- dependency graph: empty
- The possible actions are:
1. SHIFT: move a word from the top of the buffer to the top of the stack 
2. $LA_r$ Left Arc
	- The neighbor of the head of the stack is dependent of the head of the stack
	- add a corresponding link to the dependency graph with label r
	- remove the neighbor from the stack 
3. $RA_r$: Right Arc
	- the head of the stack is a dependent of its neighbor
	- add a corresponding link to the dependency graph with label r
	- remove the head of the stack 

How do we decide which action to take at each step?
- Use a discriminative classifier (SVM, perceptron, maxent) over all legal moves
	- 3 untyped choices
	- 2 |R| + 1 if we have |R| possible dependency laels ($|R|\approx 40$)
- Features: word at top of the stack word and its PoS; word at the top of the buffer and its PoS
	- Common approach in the 2000s
	- Sparse feature vectors (up to $10^{17}$ dimensions)
	- Incomplete because you can't account for everything
	- Feature computation was extremely time-consuming
- Training: use treebanks
- Fast greedy approach: pick the best option at each step
- This is the basic idea in MaltParser

## Neural Dependency Parser
- Represent words as d-dimensional word embeddings with $d\approx 1000$
- Also represent PoS tags and the already known dependency labels as d-dimensional vectors
	- NNS (plural noun) should be close to NN (singular noun)
	- amod (adjective modifier) should be close to advmod (adverbial modifier)
- Extract a set of tokens based on stack/buffer positions
	- For example, head of stack, its neighbor, head of buffer, its neighbor
- Look up their embedding vectors and concatenate them with the PoS and label embedding vectors

>[!FAQ] Neural Dependency Parser
>- Input layer: concatenated embedding vectors
>- Feedforward neural network with ReLu non-linearity
>- Softmax gives a probability distribution
>- Use cross-entropy loss to measure the error
>![[Neural Dependendy Parser.png]]
>
>Hidden layer: 
>- the input vector is linearly transformed: $$h = ReLU(Wx+b_1)$$
>- Where :
>	- $W \in R^{mx(nd)}$ is a learnable weight matrix
>	- $b_1 \in R^m$ is a bias vector
>- ReLu introduces non-linearity, allowing the network to learn complex patterns
>
>Output layer:
>- The hidden layer output is further transformed: $$z = Uh+b_2$$
>- Where:
>	- $U \in R^{kxm}$ maps hidden features to k output classes
>	- $b_2 \in R^k$ is another bias term
>- The result is a vector of logits, which are unbounded raw scores
>
>Softmax probabilities: 
>- The logits are converted into probabilities using the softmax function: $$P(y_i) = \frac{e^{z_i}}{\sum_{j}e^{z_j}}$$
>- This ensures: 
>	- All outputs are positive
>	- Probabilities sum to 1
>
>Cross-Entropy Loss
>- Cross-entropy measures the difference between two probability distributions
>- It is used to compare:
>	- True class labels (one-hot encoded)
>	- Predicted probabilities (softmax outputs)
>- Formula for cross entropy loss for k classes:$$L = -\sum_{i=1}^{k}y_ilog(\hat{y}_i)$$
>- Where $y_i$ is the true label (1 for the correct class, 0 otherwise) and $\hat{y}_i$ is the predicted probability
>- For a single correct class c, cross-entropy simplifies to: $$L = -log(\hat{y}_x)$$
>- If the predicted probability $\hat{y}_c$ is:
>	- High (close to 1) -> loss is small
>	- Low (close to 0) -> loss is large
>- Encourages the model to output high confidence for the correct class






