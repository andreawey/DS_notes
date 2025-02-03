[[Anomaly, outlier and novelty detection]]

Defining Normality and Abnormality

We need to define what normality and abnormality are. Unless a law of normality is known (e.g., in some physics applications), it must be estimated from the data.

Let $X \subset \mathbb{R}^D$ be the data space. Assume that $P_+(x)$ is the distribution of “normal” samples, with probability density function $p_+$. Then it is straightforward to define the set of anomalies as

$$ A = \{ x \in X \mid p_+(x) \leq \tau \} $$

i.e., the set of data points whose probability of being part of the normal samples falls below a certain threshold.
