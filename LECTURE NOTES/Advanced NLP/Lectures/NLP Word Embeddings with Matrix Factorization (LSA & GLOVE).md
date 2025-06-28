# Modeling relatedness of word meanings 
- Distributional semantics teaches us that words occurring often in similar contexts are likely to be similar
- Relatedness of word meanings may be partially obtained via dictionaries, as far as finding synonyms is concerned... but dictionaries are only available for a few languages and may be incomplete
- The more general topic of word similarity if preferably learnt from data covering similarity of types or topics
Idea: transform high-dimensional space of words, where all words are perpendicular see TF-IDF VSM, into a low-dimensional, dense embedding space, where topically-similar words get closer

# LSA Latent Space Approximation
## Intuition behind dimensionality reduction
- Instead of many unrelated vectors, find a cluster of vectors that point in the same direction
	- Change the axis of the space to this direction
	- Represent each vector through its coordinate on this direction
	- Proceed orthogonally to the next direction, etc
	- Stop at a much smaller number of new vectors than the original space -> implies an approximation
- Results: in the reduced space, similar original vectors are all reduced to the same principal component

>[!INFO] Singular Value Decomposition (SVD)
>A matrix $M$ of size $mxn$ can be decomposed by 3 geometrical transformations $U * \Sigma* V^T$
>- Rotation $U$ is orthongonal matrix of size $mxm$ with $U^T * U = 𝕀_m$
>- Coordinate by coordinate scaling $\Sigma$ of size $mxn$ is a matrix with null coefficients, except for non-negative diagonal elements $\sigma_i$ sorted in decreasing order.
>	- Diagonal elements $\sigma_i$ are the singular values of $M$ i.e. the square roots of the eigenvalues of $M^T * M$
>- Rotation $V^T$ of size $nxn$ is the transpose of an orthogonal matrix (i.e. $V^T * V = 𝕀_n$)
>![[SVD.png]]
>

>[!FAQ] SVD toy example
>![[SVD Example.png]]

## SVD versions 

- Full SVD
- Thin SVD: remove columns of $U$ not corresponding to rows of $V^T$ 
- Compact SVD: remove zero singular values and the corresponding columns/rows in $U$ and $V^T$ 
- Truncated SVD: approximate M by keeping only the largest t singular values $\sigma_1, ...\sigma_t$ and corresponding columns/rows in $U \& V^T$
  It yields the closest approximation of M with rank r by another matrix M' of lower rank t (t<r)

## SVD applied to the word-document matrix

- A word-document matrix can be re-written as M = W * S * D
- W: each row corresponds to a word in the vocabulary, and the m columns are dimensions of a "latent topic" space
- S: diagonal matrix with singular values, giving the importance of each dimension
- D: each column corresponds to a document in the database, the n rows are the dimension of a "latent topic" space

## Text embeddings from word-doc matrix with LSA
- Decompose the word-document matrix M with truncated SVD using dimensionality t
- use $W_{trunc}$ of size |vocabulary| x t to identify low-dimensional embeddings
  i.e. extract the $i^{th}$ row of $W_{trunc}$ to represent a t-dimensional vector for the $i^{th}$ word of the word document matrix M
	- Alternatives used: $W_{trunc}*\sqrt{S_{trunc}}$ or $W_{trunc}*S_{trunc}$
- The dimensionality t is selected to keep 80%-90% of the total "energy" of the singular values
  i.e. $\sum^{t}_{t=1}\sigma_i= \alpha * \sum^{N}_{i=1}$ with $\alpha \in [0.8, 0.9]$
- Heuristics from past: 50 < t < 1000, frequently t approx 300

## Applications and limitations of LSA
- Additional applications include:
	- Predict query-document topic similarity judgments
	- Simulate human choices on subject-matter multiple choice tests
	- Predict text coherence and resulting comprehension, subjective ratings of essays
- LSA enables the use of word co-occurrences to capture latent semantic associations of terms and thus addresses the first problem by detecting synonyms 
- However, 
	- Dimensions of the LSA embedding space do not always have good interpretations
	- LSA has a significant computational cost for large corpora (due to SVD) which can be reduced by building the LSA embeddings from a document subset
	- LSA cannot express negations nor enforce Boolean conditions

## Extracting meaning from word co-occurrences
- Observations show that ratios of word-word co-occurrence probabilities have the potential to encode semantic meaning 
- using vectors for words $(w_i, w_j)$ and context $(w_k)$ the ratio can be expressed as 
  $$\frac{P_{ik}}{P_{jk}} = \frac{X_{ik}}{X_{i}}*\frac{X_j}{X_{jk}} = F(w_i, w_j, w_k)$$


# Global Vectors (GloVe) for word representations
- GloVe is a matrix-based distributional semantic model trained in an unsupervised manner leveraging ratios of word co-occurrence probabilities
- The training uses
	- A log bi-linear regression objective function i.e. word vectors align with the logarithm of the words co-occurrence via dot product
	- 5 large corpora extracting the 400k most frequent words, the co-occurrence matrix is constructed with a weighting factor 1/d reducing influence for word pairs d words apart in the total counts
$$J= \sum^{V}_{i,j=1}f(X_{ij})(w_i^T\tilde{w}_j +b_i + \tilde{b}_j - logX_{ij})^2$$
$X_{ij}$ co-occurrence of word i and word j
$w_i$ embedding for word i
$b_i, \tilde{b}_j$ bias terms i
$F(X_{ij})$ weighting function that assigns lower weights to rare and frequent co-occurrences

## GloVe - identifying word analogies & similarities
- Euclidean distance or cosine similarity between two word vectors provides an effective method for measuring the linguistic or semantic similarity of the corresponding words
- Word analogies answer the question "a is to b as c is to ?" by finding the word d whose embedding $w_d$ is closest to $w_b - w_a + w_c$ according to the cosine similarity

>[!FAQ] Conclusion
> - The contextual understanding of LSA and GloVe builds upon of co-occurrence count statistics and leverages matrix factorization resulting in dense word embedding of dimensionality of a few 100's 
> - LSA efficiently leverages statistical information from the word-document matrix, but does relatively poorly on the word analogy task
> - GloVe trained on global word-word co-occurrence counts makes efficient use of statistics and excels in word similarity and semantic and syntactic word analogy tasks
> - While both LSA and GloVe use the Bag-of-Word approach, GloVe accounts for word pair distance when constructing the word co-occurrence matrix
> - The unsupervised learning paradigm used word representations from large-scale corpora without requiring labeled data making it scalable and applicable to a wide range of languages and domains


> 