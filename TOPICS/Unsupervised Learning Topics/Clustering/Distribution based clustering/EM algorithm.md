[[Distribution based clustering]]

## The algorithm

![[EM Algorithm.png]]


Comparison [[GMM]]-based clustering solves a number of problems of the more elementary [[K-Means]] algorithm:

• The spatial extent of clusters is modelled by the covariance matrix of each cluster.
• We have a natural way to perform soft assignment, using a probabilistic framework.
• Also the algorithm as a whole is firmly rooted in probability theory. This can be interesting
in certain use cases.
• - GMM-based clustering works badly if the clusters have shapes which are very non-Gaussian.