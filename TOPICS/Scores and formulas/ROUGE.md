ROUGE is evaluated between a candidate and a set of reference texts via matching n-grams and ranges from 0 to 1
1. Compute N-gram matches between candidate c & all references $r_i$
2. Add up number of matches for all reference sentences $r_i$
3. Divide by the sum of reference N-grams

$$ROUGE -N = \frac{\sum_{S \in Refs}\sum_{N-gram \in s}{count}_{match}(N-gram)}{\sum_{S\in Refs}\sum_{N-gram\in s}count(N-gram)}$$

Rouge-N defines N and only considers N-grams (unlike BLEU)
Inner sum over N-grams in a given sentence, outer sum over all reference sentences

NB: ROUGE-1 focuses on keywords, ROUGE-4 on syntactic & linguistic structure, $ROUGE-N_{multi refs}$ considers max pairwise summary level ROUGE-N, ROUGE-L the longest common subsequence

ROUGE is quick to calculate, easy to understand, language independent, but considers only exact word matches w/o semantic meaning or word importance