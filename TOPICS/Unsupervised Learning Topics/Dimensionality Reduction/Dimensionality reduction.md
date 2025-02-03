[[Unsupervised Learning]]

Dimensionality reduction means to map samples into a lower-dimensional space. Relevant applications include analysis and visualization of data, and performing preprocessing (feature extraction) for further steps in data analysis and machine learning.

- In particular, you might want to perform dimensionality reduction to reduce overfitting — careful not to destroy useful information.
- Another standard application is **noise reduction** — sometimes, data errors and artifacts can be reduced by dimensionality reduction.
- In some cases, dimensionality reduction is also relevant for **efficiency** (nowadays, in particular on small or embedded devices).


# Introduction

The idea of dimensionality reduction is heavily based on the **manifold assumption**. This means that we have a high-dimensional sample space, but we assume that the relevant data is in a subspace of much smaller dimension. This could be a **linear subspace** or a “**manifold**”, which is a curved subspace that is (locally) “similar” to a linear subspace. 

As for other unsupervised methods, the objective function and the evaluation of dimensionality reduction methods heavily depend on the use case. If no better (e.g., data-specific) objective function can be found, a typical generic objective is to minimize the loss of information incurred by the dimensionality reduction. 

The mapping from high-dimensional source space to low-dimensional target space is parametrized/constrained in a way to make the result useful.




PCA
	- [[PCA]]
	- [[PCA considerations]]
Autoencoder
	- [[Autoencoder]]
	- [[The denoising Autoencoder]]