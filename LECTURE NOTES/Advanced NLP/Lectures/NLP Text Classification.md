

| Type                       | Definition                                                                                         |
| -------------------------- | -------------------------------------------------------------------------------------------------- |
| Sentiment analysis         | Classification based on the polarity or emotional tone of text                                     |
| News classification        | Categorize news articles based on content and subject matter                                       |
| Topic labeling             | Identify the main subject or topic in text                                                         |
| Question answering         | Provide accurate answers to user queries                                                           |
| Natural Language Inference | Determine the logical relationship between sentences                                               |
| Dialog act classification  | Identification of text intention such as requesting, questioning, or informing                     |
| Named entity recognition   | Categorize named entities such as people, orgs and locations                                       |
| Syntactic parsing          | Analyze the grammatical structure of text to understand the relationship between words and phrases |


# Classification types
## Binary Classification
- Assign one of two mutually exclusive categories to text
- Ex: spam detector

## Multiclass Classification
- Assigns one of three or more mutually exclusive categories to a text
- Ex: classify text into genres
- Useful for organizing data into distinct, non-overlapping groups

## Fine-Grained sentiment analysis
- Common example of a multiclass
- Captures more nuances
	- Positive vs negative vs neutral
	- Amazon's five star rating system
- Easy to go overboard

## Aspect-Based Sentiment Analysis
- Analyzes sentiments toward specific aspects of features of a product
- Ex: Evaluating a product's quality, price, or design separately

## Offensive language detection
- Identifies text containing abusive, offensive, or inappropriate content
- Common in moderating social media or public forums

## Multilabel Classification
- Assigns multiple overlapping labels to a single text

## Intent recognition
- Trains models on dialog to understand users intent
## Document classification
- Classify longer documents
- Traditionally done using standard non-neutral techniques
- Shifted toward word embeddings and contextual embeddings
- Toda, LLM-Based

## Fake news classification
- Identifies patterns to classify news as true or fake
- Useful for combating misinformation in media
- Incredibly hard to do 

## Cross-Lingual Classification
- Different from language detection, which is relatively simple
- Classification of multi-lingual content into language-independent categories

## Stance detection
- Determines the perspective toward a piece of text (in favor, against, neutral)
- Applicable to political debates, ...
- Conceptually similar to sentiment analysis but applied to text of different nature

## Emotion/Mental Health Detection
- Assesses the emotional or mental health status of a person from text
- Detection of stress, anxiety or depression in online interactions

## Multimodal Classification
- Combines multiple data types such as text, video audio


# Performance evaluation 
- Accuracy: fraction of test documents that have been correctly classified
- Precision for class: fraction of test documents classified as class x correctly
- Recall: fraction of test documents labeled as x that have been correctly classified
- F1 score for class c: harmonic mean of precision and recall 

$$F1= \frac{2P_cR_c}{P_c+ R_c}$$
$$P_c = \frac{TP}{TP+FP}$$
$$R_c = \frac{TP}{TP+FN}$$


>[!INFO] TF-IDF
>$TF_IDF(t,d)= TF(t,d)*IDF(t,D)$
>Components: 
>- Term Frequency (TF):
>	$TF(t,d) = \frac{\text{Number of times term t appears in document d}}{\text{Total number of terms in d}}$
>- Inverse Document Frequency (IDF):
>	$IDF(t,D)= log\frac{\text{Total number of documents}}{\text{Number of documents containing t}}$
>- Combined TF-IDF reflects the importance of a term in a document while down-weighting terms that are frequent across the corpus

>[!FAQ] TF-IDF Example
>Document corpus:
>- Document 1: "The cat sat on the mat"
>- Document 2: "The doc chased the cat"
>- Document 3: "The cat and the dog played"
>Steps: 
>- Compute **TF** for term t= "cat" in Document 1:
>	$TF("Cat", Doc1) = \frac{1}{6} = 0.167$
>- Compute **IDF** for term t = "cat":
>	$IDF("Cat") = log\frac{3}{3} = 0$
>- Compute **TF-IDF**: 
>	$TF-IDF("cat", Doc1) = 0.167 * 0 = 0$

# Legacy ML
- Many well-established, easy-to-use algorithms
- Near zero coding effort for anyone
- No need to know what's going on under the hood

Classifiers: 
	How to use them: 
	- Transform each training example into a vector 
	- Use the vectorized training set to train some ML model
	Which ML model? 
	- Naive Bayes
	- Logistic regression
	- Support vector machines
	- KNN
	- Decision Trees
	- Random Forests
	- Gradient boosting machines

### Logistic regression
- models the probability of a binary outcome: 
$$P(y=1|x)$$
- probabilities are constrained to \[0, 1] but a linear model is not suitable because it can output values outside this range

### Odds
Logistic regression uses the odds
$$Odds = \frac{P(y=1|x)}{1- P(y=1|x)}$$
- Why odds ?
	- Odds are always positive ($[0, \infty]$)
	- They represent the likelihood of success relative to failure

### Log-Odds
Logistic regression takes the logarithm of the odds: 
$$Log-Odds = ln(\frac{P(y=1|x)}{1- P(y=1|x)})$$
- Why Log-Odds ?
	- the log makes the odds unbounded ranging from $-\infty to +\infty$
	- This allows us to model the log-odds as a linear function:
	$$Log-Odds = z = w_1x_1 + w_2x_2 + ... + w_nx_n + b$$
	- Once the log-odds are modeled as a linear function, we convert them back into probabilities using the [[Sigmoid Function]]
		![[Sigmoid Function]]

>[!FAQ] Logistic regression in a nutshell
>predicts probabilities for binary classifiers
>1. Linear model: $z = w_1x_1 + w_2x_2 + ... + w_nx_n + b$
>2. Sigmoid function: $\sigma(t)= \dfrac{1}{1+e^{-t}}$
>   Converts t into a probability between 0 and 1
>3. Decision boundary: if $P(y=1|x) \geq =0.5$ predict the positive class, otherwise negative
>
>Binary classification: Logistic regression uses a single sigmoid function
>Multiclass classification: It uses softmax function to predict probabilities for each class
>$$Softmax: P(y=i|x) = \frac{e^{z_i}}{\sum_j{e^{z_j}}}$$


## Support Vector Machines
![[Support Vector Machines]]



# From legacy ML to Transformers
- Legacy ML methods require preprocessing
- RNNs learn representations dynamically,but operate sequentially and have problems with long-range dependencies
- Transformers learn representations dynamically and leverage the attention mechanism to move past the limitations of RNNs

## BERT
BERT (Bidirectional Encoder Representations from Transformers) is a pre-trained language model
It processes text by splitting into tokens and learning contextual representations for these tokens by processing the next bidirectionally (left-to-right and right-to-left)

In its original form BERT only outputs contextualized embeddings (vectors) for tokens, no built in mechanism for classification

### Fine-tuning BERT
We can adapt the pretrained BERT model for a specific downstream task by adding and training an additional layer on top of it

Input preparation:
- Input sentence: for classification the entire input sentence or text is tokenized and fed to BERT
- A special token \[CLS] is added to the beginning of the input to serve as a summary representation of the entire input text
Step 2: Pass through BERT
- The tokenized input passes through BERT's transformer encoder layers
- Each token gets converted into a contextualized embedding, these embeddings capture the meaning of tokens in the sentence, taking the context into account.
- The embedding of the \[CLS] token at the final layer can be used to represent the entire sentence for classification.
Step 3: Add a classification head
- this is a thin layer of neurons
- a fully connected feedforward neural network added on top of the \[CLS] embedding
- The \[CLS] token contains the aggregated info about the input text, but we need a way to map this representation to a specific output like "positive" or "negative"
>[!FAQ] For example if you're classifying text into 3 categories
>- The classification head consists of: 
>	- a fully connected layer with 3 neurons (one for each category)
>	- A softmax [[Activation Functions]] that converts the output into probabilities for each class

Step 4:  Fine-Tuning
- Only small changes to BERT: the model itself is fine-tuned but only slightly. Most of its pretrained knowledge is retained.
- Train the classification head: the classification head is trained from scratch
- Loss function: the standard classification is cross-entropy-loss, which compares the predicted class probabilities to the true labels
- The optimizer adjusts the weights in both BERT and the classification head to minimize the loss

>[!INFO] What happens during inference ?
>1. New text input is fed
>2. BERT encodes the text and the \[CLS] token's embedding is passed to the classification head
>3. The classification head outputs a probability distribution over the classes
>4. The class with the highest probability is selected as the prediction

>[!FAQ] Takeaway
>BERT is kind of like a very knowledgeable reader that has gained some meaningful level of understanding of human language through sheer exposure. 
>Fine-tuning teaches our reader to specialize in a specific task, like identifying sentiment or detecting spam, by adding a simple decision-making layer on top

