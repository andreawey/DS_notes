   ## n-gram LM and Markov assumption
- Simplest LM is based on sequences of words, the n-gram
- Markov chain assumption: $P(w_n|w_1, ..., w_{n-1} \approx P(w_n|w_{n-m}, .., w_{n-1})$
- The order of the LM is defined by m often:
  $P(w_1, w_2, ..., w_n) = \prod_{k=1}^n P(w_k|w_1, ..., w_{k-1})$
	- m= 0 (unigrams)  -> $P(w_1, w_2, ..., w_n)  \approx \prod_{k=1}^n P(w_k)$
	- m= 1 (bigrams)     -> $P(w_1, w_2, ..., w_n)  \approx \prod_{k=1}^n P(w_k|w_{k-1})$
	- m= 2 (trigrams)     -> $P(w_1, w_2, ..., w_n)  \approx \prod_{k=1}^n P(w_k|w_{k-2}, w_{k-1})$
- Example with trigrams:
  $P(the, students, opened, their) \approx P(the|<s>, <s>)$     $P(students|<s>, the)*P(opened|the, students)$   $P(their|students, opened)$

## Learning a language model (LM)
- To compute the probability of a sequence, find the probabilities of its n-grams
	- Example with trigrams: what is out best estimate of $P(w_k|w_{k-2},w_{k-1})$ ?
- Use a large training corpus and apply maximum likelihood estimation (MLE) based on the idea that "what you see is in fact the most likely event"
	- Use relative frequencies i.e. count numbers of occurrences of trigrams and normalize counts so that they lie between 0 and 1 
	  $$P(w_k,|w_{k-2},w_{k-1}) = \frac{count(w_{k-2}w_{k-1}w_k)}{\sum_{∀w \in Vocab}count(w_{k-2}w_{k-1}w)}=\frac{count(w_{k-2}w_{k-1}w_k)}{count(w_{k-2}w_{k-1})}$$
	- The larger the training corpus, the better the estimations .. but the more n-grams must be stored in the model ... and increasing n also increases model size

## Applying a language model
- Compute the probability of the sentence "I want to eat Chinese food" using a bigram model i.e. $P(w_1, w_2, ..., w_n) \approx \prod_{k=1}^n P(w_k|w_{k-1})$
![[Bigram probability example.png]]



## A limitation of counts: unseen n-grams
- Approximating n-gram probabilities with counts works well for frequent n-grams, 
  $$P(nextword | students, opened, their)= \frac{count(\text{students opened their nextword})}{count(\text{studentts opened their})}$$ but ....
1. Numerator if "students opened their nextword" never occurred in the training corpus, then conditional probability equals zero
2. Denominator: if "students opened their" never occurred in the training corpus, then we can't even calculate probability for a next word at all
- Certain n-grams might not be present in the training corpus but appear during inference -> For new sentence containing them, we get zero probability!
- Increasing n, i.e. using higher order n-grams increases these two sparsity problems

### Solution: 
- Solution to sparsity problem #1: smoothing
	- Shave off a bit of probability mass from some more frequent n-grams and give it to the n-grams that were never seen
	- Add a fictitious count (a small number) to each probability, to avoid unseen n-grams having 0 probability
- Example for tri-grams.
  $$P(w_k,|w_{k-2},w_{k-1}) = \frac{count(w_{k-2}w_{k-1}w_k)}{\sum_{∀w \in Vocab}count(w_{k-2}w_{k-1}w)}\approx \frac{count(w_{k-2}w_{k-1}w_k)+1}{count(w_{k-2}w_{k-1})+N_{trigram}}$$
  -> laplace smoothing with normalization
  $$\approx \frac{count(w_{k-2}w_{k-1}w_k)+k*1}{count(w_{k-2}w_{k-1})+k*N_{trigram}}$$
  -> improved version 
  reduce correction and estimate n with held out data (compare with actual counts of unseen n-grams)

### Other solutions for unseen n-grams
- Solution to sparsity problem  #2: backoff & interpolation
	- If we have no examples of a particular trigram, using less context helps to generalize
	- Backoff: if a count is zero for an n-gram, then use lower-order n-grams
	- Interpolation: combine lower-order n-grams e.g. through linear interpolation
		- compute counts on training data for all lower-orders, combine language models of different orders with positive weights $\lambda_i, \sum_i \lambda_i =1$ constant or context-specific, tune weights on an additional held-out corpus
$$P(w_k|w_{k-2}, w_{k-1}) \approx \lambda_1*P(w_k) + \lambda_2*P(w_k|w_{k-1}) + \lambda_3*P(w_k|w_{k-2}, w_{k-1})$$
- Desired effect: a zero count for an unseen trigram does not lead to a zero interpolated probability