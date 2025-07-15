# The transformer
- Encoder-Decoder model for MT without recurrence
- Encoder: input sentence -> abstract representation 
	- N stacked blocks of multi-head attention layers plus fully connected FF NN layers
- Decoder: output sentence up to $w_{i-1}$ -> $P(w_i)$
	- N stacked blocks of masked multi-head attention layers with inputs from encoder and past decoder layers + fully-connected FF NN layers

Note: like the RNN the model does not really generate a translation but assigns probabilities on word $w_i$ given input and output $w_1, ... , w_{i-1}$
A decoder procedure for instance beam search is needed to build the translation 
![[Transformer.png]]

![[Transformer parts.png]]

# Self-attention
- Self-attention learns a vector representation of each word base don the representation of potentially distant words in the sentence
- The input vector of the word in position i is noted $x_i$ from which 3 vectors are derived
- Each word is represented as vector playing three roles
	- Query $q_i$ "hey there, do you want to have this information"
	- Key $k_i$ "Yes, give me a large weight"
	- Value $v_i$ "Here is the information"
- This logic is applied in every encoder sublayer. All input vectors can be packed into one matrix X



![[Self- attention detail.png]]

