## Language models (LMs): Definition & application
- Definition: given a word sequence, an LM predicts the most likely next word or serves to compute the probability of any sequence of words
- LMs are subcomponents of many NLP tasks
	- Spelling and grammar correction 
	- Speech recognition
	- Predictive typing (autocompletion)
	- Machine translation
	- OCR and handwriting recognition
	- authorship identification
	- summarization
	- chatbots
	- information retrieval

### Formal Definition
- LMs are models/tools that
	- either compute the joint probability of a sequence of words $P(w_1, w_2, w_3,..., w_n)$
	- or compute the conditional probability of a next word $P(w_n|w_1, w_2, w_3, ... w_{n-1})$
	- The two formulations are equivalent because $$P(w_n|w_1, w_2, w_3, ..., w_{n-1}) = \frac{P(w_1,w_2,w_3, ..., w_n)}{P(w_1,w_2,w_3, ..., w_{n-1})}$$
	- Therefore going from left to right (earlier to later words) via chain rule: $$P(w_1,w_2,...,w_n)=P(w_1)*P(w_2|w_1)*P(w_3|w_1,w_2)* ... * P(w_n|w_1, ..., w_{n-1}) = \prod_{k=1}^nP(w_k|w_1,...,w_{k-1})$$

### Evaluating language models
- Extrinsic evaluation: how much does a LM help a task?
	- Complex, computationally expensive, task-specific assessment
- Intrinsic evaluation: perplexity (train LM, evaluate LM on unseen test set)
	- Probability $P(w_1,w_2,w_3, ... w_N)$ -> the higher the better
	- Perplexity $PP(w_1, w_2, w_3, ..., w_N)$ -> the lower the better

$$PP(W)=P(w_1, w_2, w_3, ... w_N)^{-1/N}$$
$$=\sqrt[N]{\frac{1}{P(w_1,w_2,w_3, ..., w_N)}}$$
$$=\sqrt[N]{\prod^N_{k=1}\frac{1}{P(w_k|w_1,w_2,w_3, ...,w_{k-1})}}$$
$$=2^{H(W)}=2^{-\frac{1}{N}log_2(P(w_1,w_2,w_3, ...,w_N))}$$

|       | Unigram | Bigram | Trigram |
| ----- | ------- | ------ | ------- |
| PP(W) | 962     | 170    | 109     |

## Machine Translation (MT)
- Goal automatically translate a sentence or document from a source language to a target language, preserving the meaning, tone and fluency
- Application domains include real-time communication, medical report or legal documents translation or multilingual customer/service support
- Sentence complexity, idioms, and domain-specific vocabulary affect translation quality, target language pairs is a parameter of the task determining difficulty of task
- Methodologically there are 2 main approaches
	- Rule based / statistical translation "assembled" translations from pieces of text using rules or aligned phrases
	- Neural Machine Translation, translates by generating new text in the target language, like a human translator would; approach involves training on source-target parallel corpora and is enabled by the fast improvements of LMs conditioned on source text; seq2seq


## Evaluating LMs with BLEU for MT
![[BLEU]]
## Automatic summarization
- Goal create a summary of a given text or several texts preserving core meaning and key information
- Application domains include news aggregation, scientific article digests, legal or medical report summarization, chat and meeting summaries
- Text type, eg. structure, tone and density of the input text matter, summary length is a parameter of the task
- Methodologically there are 2 main approaches:
	- Extractive, selects the most representative sentences of the text without reformulation; coherence of abstract is problematic but often more faithful and appropriate for news
	- Abstractive, generates new shorter text by rephrasing and compression, just as humans do, approach involves training on text-summary pairs and is enabled by the fast improvements of LMs conditioned on source text; seq2seq

## Evaluating LMs with ROUGE for summarization
![[ROUGE]]


## Evaluating LMs for summarization
- other n-gram based metrics are also used, eg. BLEU or METEOR
	- METEOR computes the harmonic mean of unigram precision and recall and incorporates stemming and synonym matching for better alignment with human judgments 
- Embeddings help to overcome some of the limitations of n-gram based metrics
	- BERTScore computes cosine similarity between contextual embeddings, capturing semantic meaning beyond exact word matches
	- BLEURT fine-tunes BERT on human evaluation data, providing robust trained assessment of summarization quality
	- Value-add recognizing paraphrasing, reordering & meaning preservation with different wording
- Human evaluation remains important for several reasons
	- Grammatically, referential clarity, conciseness, fluency, coherence, coverage, faithfullness, ..

## Common denominator: conditional text generation

- We look for language models: $P(w_n|C, w_1, w_2, w_3, ..., w_{n-1})$ whereC is a context
	- Pure language model C=Ø
	- Machine translation: C=source language text
	- Summarization C= original article
	- Other options 
- Models thus predict the next token: $argmax_{w_n\in V}P(w_n|C, w_1, w_2, w_3, ..., w_{n-1})$
	- Applied several times in order to generate a larger text = "decoding"
- If C is a sequence of tokens: sequence-to-sequence model (seq2seq)
- 