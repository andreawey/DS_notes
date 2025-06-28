you can't simply input words into neural networks
NN input = activate input unit = one-hot vector
Input layer cannot be extended excessively

>[!FAQ] OOV = Out of vocabulary

## Word- / Character- / subword-based tokenization

- Word-based: split text into individual words
	- simple and intuitive: use white-spaces, deal with punctuation
	- limitations: need to handle many exceptions -> OOV -> no relationship between words
- Character-based: split text into individual characters
	- very simple, no need for specific rules
	- can handle any word (no OOVs)
	- long sequences, much more difficult to learn, hard to capture semantic information
- Subword-based: vocabulary made of words, word parts, and characters learned from the training data


# Byte-Pair Encoding (BPE)

## Byte-Pair Encoding (BPE): preprocessing
- Byte-Pair Encoding requires input text to be preprocessed for training to build vocabulary and testing to tokenize new text
- Preprocessing includes:
	- Clean up newlines and multiple whitespaces, possibly lowercase
	- Tokenize into words and punctuations based on whitespaces and/or model-specific-rules
	- Mark word endings with a specific symbol (eg </w>) or word continuations (## or \_\_)
	- split text into individual characters, remove infrequent ones

## Byte-Pair Encoding (BPE): training
- Build the vocabulary through successive applications of "merge" operations
1. what is the most frequent character bigram? 
   -> add new symbol to vocabulary
   -> update vocabulary
Repeat for x steps

>[!EXAMPLE] Example of BPE
>Training corpus: low (5), lower (2), newest (6), widest (3)
>Initial vocabulary, after pre-processing: `l o w</w> e r n s t i d`
>
>1st step: most frequent bigram `e+s` 9 times
>-> add new symbol `es` to vocabulary
>-> update: `l o w </w> e r n s t i d es`
>
>2nd step: most frequent bigram `es+t` 9 times
>-> add new symbol `est` to vocabulary
>-> update: `l o w </w> e r n s t i d es est`

## Byte-Pair Encoding (BPE): results & testing
- Results of training consists of the vocabulary and a set of "merge rules"
- vocabulary contains:
	- all characters, many frequent words, frequent character n-grams (some meaningful some not)
- vocabulary size = number of initial characters + number of merges -X
	- often between 10k and 32k, X: some subwords are removed if they occur only as parts of larger subwords
- Testing mode: tokenize new text
	- Preprocess, including splitting into characters
	- apply the merge operations learned, in the same order
		- no new counts are needed


# WordPiece

## WordPiece: training
WordPiece builds the vocabulary similarly to BPE through successive applications of "merge" operations starting from a character based-vocabulary
- Start with a vocabulary of individual characters from the training corpus adding \[UNK] for unknown tokens and the \## prefix for subwords that don't start a word
- Split training corpus into words and create all possible subword combinations for each word
- Count all subword frequencies in the training data (track appearance at start/middle/end)
- Evaluate possible new word units, i.e candidate merges $x, y$ using scoring function $$\frac{P(xy)}{P(x)P(y)}$$
- Generate new word unit with the highest score by merging and add it with the \## prefix, then update frequencies of the added tokens
- stop when the desired vocabulary size is reached, or highest merge score falls below a threshold 

## WordPiece: results & testing
Results of training consists of the vocabulary
- Vocabulary contains
	- All characters, many frequent words, subwords and all UNK symbol for completely new words
- Vocabulary size
	- sizes vary between 30k and up to 110k 

Testing mode: tokenize new text
- Preprocess, including splitting into words
- For each word, match the longest subword from the vocabulary at the start of the word
- If a match is found, add it to the output and repeat from the next character, for matches after the first character, use the \## version of the subword
- If no match is found, add \[UNK] token and move to the next character


# Comparison of subword tokenization methods



|                     | BPE                                                                               | WordPiece                                                                                                                                                           | UnigramLM                                                                                                               |
| ------------------- | --------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------- |
| Training            | Starts from a small vocabulary<br>and learns rules to merge tokens                | Starts from a small vocabulary and<br>learns rules to merge tokens                                                                                                  | Starts from a large vocabulary and<br>learns rules to remove tokens                                                     |
| Training steps      | Merges the tokens corresponding<br>to the most frequent pair                      | Merges the tokens corresponding<br>to the pair with the best score<br>(frequency of the pair, privileging<br>pairs where each individual token is<br>less frequent) | Remove n% tokens corresponding to<br>the lowest reduction in likelihood<br>upon removal computed on the<br>whole corpus |
| result of training  | “Merge” rules and a vocabulary                                                    | Just a vocabulary                                                                                                                                                   | Vocabulary and score for each token                                                                                     |
| Encoding a new text | Splits words into characters and<br>applies the merges learned during<br>training | Finds the longest subword (starting<br>from the beginning) that is in the<br>vocabulary, then does the same for<br>the remainder of the word                        | Finds the most likely split into<br>tokens, using the scores learned<br>during training                                 |


# SentencePiece

## SentencePiece: training
directly operates on raw text without pre-tokenization/pre-splitting
- Text is considered as a sequence of Unicode characters
- Whitespace is replaced with "\_"
- T ext is normalized via Unicode NFKC normalization or custom normalization rules
Token vocabulary of predefined size is created by BPE or UnigramLM on raw text
- UnigramLM starts with a large set of candidate subwords and prunes them probabilistically using a likelihood model
Approach:
- Does not require language-specific pre-tokenization
- Is readily applicable for language w/o whitespace-separators (ex. Chinese, Japanese)
- Improves NMT accuracy and robustness via subword regularization using multiple subword segmentations, ex: "internationalization" -> \[Internation, alization] or \[International, ization]

## SentencePiece: results & testing
Results of training is a fully self-contained model with no external dependencies
- Pre-compiled finite state transducer for character normalization
- vocabulary and segmentation parameters contains
	- all characters, many frequent words, subwords, UNK for completely new words
- Vocabulary size is around 32k for T5 and Llama
Testing mode: tokenize new text
- Use SentencePiece for tokenization offline and integrate into the pipeline
- segmentation speed of SentencePiece is fast enough for on-the-fly execution



>[!INFO] Subword tokenization
>- Splits text into subword units and reduces vocabulary size compared to word-based tokenization
>- Effectively handles out-of-vocabulary words and captures certain semantic relationships between words and their variations
>- Is more complex to implement & produces longer sequences than word-based tokenization

