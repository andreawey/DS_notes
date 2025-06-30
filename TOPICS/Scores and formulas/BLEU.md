The BLEU metric ranks target sentence by function of the number of n-gram overlaps with the human translation and is based on precision
- BLEU is evaluated on a corpus and ranges from 0 to 1, built by comparing reference text r and candidate text c
	1. Compute n-gram matches sentence by sentence
	2. clip n-gram counts for all the candidate n-grams in the corpus
	3. Divide by number of candidate n-grams in the corpus
	4. compute geometric mean across all n-grams
	5. multiply by brevity penalty BP
It is quick to calculate, easy to understand, language independent, but considers only exact word matches w/o semantic meaning or word importance

$$BLEU= BP* exp(\sum_{n=1}^{N}w_n*log(prec_n))$$
$$= min(1,e^{1-\frac{r}{c}})* \prod_{n=1}^Nprec_n^{w_n}$$
$$prec_n = \frac{\sum_{cand}\sum_{n=1}^{N}count_{match,clipped}(n-gram)}{\sum_{cand}\sum_{n=1}^{N}count(n-gram)}$$
r/c : # words in reference / candidate translation


>[!INFO] BLEU examples
>- Reference: "The guard arrived late because it was raining"
>- Candidate: "The guard arrived late because of the rain"
>- n-gram precision = # n-gram matches / # total candidate n-grams
>$$\begin{array}{c|cccc}
> n & 1 & 2 & 3 & 4 \\
> \hline
> \text{n-gram precision} & \frac{5}{8} & \frac{4}{7} & \frac{3}{6} & \frac{2}{5} \\
> \end{array}$$
> - Geometric mean = $exp(\sum_{n-1}^{4}w_n*log(p_n)) = (5/8)^¼*(4/7)^¼*(3/6)^¼*(2/5)^¼$
> - Brevity penalty = $min(1, e^{1-\frac{r}{c}})= min(1, e^{1-\frac{8}{8}}) = 1$

