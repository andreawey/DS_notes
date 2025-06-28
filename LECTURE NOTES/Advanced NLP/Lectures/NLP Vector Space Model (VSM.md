# Vector Space Model (VSM) 
## The VSM
- The concept of representing words and documents as vectors in a multi-dimensional space was first introduced for developing information retrieval (IR) systems
- Given a vocabulary of size T, i.e. a list of unique words appearing in the document collection of size D, we represent:
	- A document d is as a vector in $ℕ^{|𝑇|}$ with components counting occurrences of words
	- A word w or term t as a vector in $ℕ^{|𝐷|}$
- This leads to the term-document matrix of size $ℕ^{|𝑇|x|𝐷|}$
- Notes
	- preprocessing the document collection by with stop-word removal, case-folding, stemming or lemmatizing can be applied to improve the efficiency and relevance of text representation
	- Under the Bag of Word (BoW) approach word order, word distance or semantic relations and similarities between words are not considered

>[!FAQ] Example
>Database with nine very short documents (i.e., titles)
>– d1 : Human machine interface for ABC computer applications
>– d2 : A survey of user opinion of computer system response time
>– d3 : The EPS user interface management system
>– d4 : System and human system engineering testing of EPS
>– d5 : Relation of user perceived response time to error measurement
>– d6 : The generation of random, binary, ordered trees
>– d7 : The intersection graph of paths in trees
>– d8 : Graph minors IV: Widths of trees and well-quasi-ordering
>– d9 : Graph minors: A survey
>![[Vector space model.png]]
>Vocabulary T pre-processed by removing articles, prepos. & words <2 occurrences
>- {‘human’, ‘interface’, ‘computer’, ‘user’, ‘system’, ‘response’, ‘time’, ‘EPS’, ‘survey’, ‘trees’, ‘graph’, ‘minors’}


## Term-Frequency (TF) Adjustments
- Relevance in fact increases sub-linearity with number of occurrences
	- If a query word occurs 5 times in a document then this document should be preferred over a document where it occurs only once.. but if a query word occurs 50 times in a document it should not be preferred 50 times more over a document where it occurs only once
- Term-frequency $TF_{term, docu}$ requires adjustment to avoid dominance of single words and comes in different flavors
- Note $c_{t,d}$ is the number of occurrences of a term t document d
![[Term frequency adjustments.png]]

## Inverse document frequency (IDF)
- Relevance in fact depends also on the informative, e.g. domain specific words even if they are rare
	- If a word occurs frequently across many/all documents (eg. "the", "is", "for") TF needs to be adjusted to suppress spurious relevance
- Document frequency related adjustment of term frequency $TF_{term,docu}$ comes in different flavors 
Approaches:
- Naive: $IDF_{term}$ equals $1/DF_{term}$ i.e the inverse of the number of documents in which the number of documents in which the term occurs
- Sophisticated: dampening with logs, $|D|$ number of documents in the collection
note: $IDF_{term}$ can be pre-computed independent of queries
![[IDF variants.png]]

## Document length normalization (DLN)
- Relevance requires careful consideration of document length
	- If a document is long the chance to match any query increases and requires penalization... but a long document can objectively have more relevant content
- Document frequency related adjustment of term frequency $TF_{term, docu}$ uses document length normalizer applying the average document length as pivot
![[Document length normalization (DLN).png]]

## TF-IDF model
- Each cell of the term-document is transformed via TF-IDF with a multiplicator $$TF- IDF_{term, docu} = TF_{term, docu} x IDF_{term, collection}$$
- TF-IDF increases each cell's value with the number of occurrences of given term within a document, and the rarity of term in the collection
- The TF-IDF transforms the term-document matrix from $ℕ^{|𝑇|x|𝐷|}$ to $R^{|𝑇|x|𝐷|}$

>[!NOTE] In conclusion
>with document preprocessing and term-document matrix transformation, text can be analyzed algorithmically
>The coefficients of the term-document matrix can be fine-tuned in various ways with different flavors of TF, IDF and DLN
>
>However different approaches are required to properly represent
>- word order
>- word distance
>- semantic relations and similarities between words

