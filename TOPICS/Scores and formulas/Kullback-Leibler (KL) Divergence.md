- KL divergence measures how one probability distribution differs from another
- For two discrete distributions P and Q: $$D_{KL}(P||Q)= \sum_{x}P(x)log(\frac{P(x)}{Q(x)})$$
- Properties:
	- $D_{KL}(P||Q)\geq 0$
	- $D_{KL}(P||Q)=0$ iff P=Q
	- Not symmetric: $D_{KL} (P||Q) \neq D_{KL}(Q||P)$
- Commonly used in machine learning to measure how much a model distribution deviates from a target


>[!FAQ] Example
>- Suppose we observe travel times (in hours): 1,2,3
>- True distribution P: \[0.5, 0.3, 0.2]
>  Model distribution Q: \[0.4, 0.4, 0.2]
>- Compute $D_{KL}(P||Q)$ using base 2 (bits):
>  $$D_{KL}(P||Q) = 0.5log_2(\frac{0.5}{0.4}) + 0.3 log_2 (\frac{0.3}{0.4}) + 0.2 log_2 (\frac{0.2}{0.2})$$
>  $$≈0.5(0.3219)+0.3(-0.4150)+0$$
>  $$≈0.1609 - 0.1245=0.0364 bits$$
>- Interpretation: small divergence -> model is close to true distribution



