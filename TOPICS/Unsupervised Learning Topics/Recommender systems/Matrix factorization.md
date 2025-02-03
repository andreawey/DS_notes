[[Recommender Systems]]

We factor the Interactions matrix into a product of two matrices with small inner dimension, thus introducing latent factors which describe the interaction. The objective is to do so minimizing the reconstruction error. The classical algorithm for matrix decomposition is Single Value Decomposition, but SVD works badly with sparse matrices. 

Modern approach Funk MF, matrix factors approximated by [[Gradient Descent]], can be considered an embedding. This opens the door for many [[Neural Networks]] uses in recommender systems.
