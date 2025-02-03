[[Recommender Systems]]

It is a common technique to use [[Neural Networks]] to create Embeddings.

1. Compute trainable embeddings of user vectors and item vectors.
2. Train a neural network-based regressor (could also be a simple dot product) on these embeddings. Training is done by [[Gradient Descent]].

This approach could also be used to incorporate additional information (e.g., content information).

Embeddings are a way to regularize the [[Recommender Systems]].

Basic systems can tendentially scale badly and are prone to overfitting to only the most popular items. More complex systems use an underlying model to alleviate this problem, and to obtain better performance.
