[[Unsupervised Learning]]

# Neural Network for Nonlinear Unsupervised Dimensionality Reduction

We aim at using a [[Neural Networks]] for nonlinear unsupervised [[Dimensionality reduction]]. Since neural network training always requires computing some loss with respect to a target, we need to define a surrogate loss which will be used for training by [[Gradient Descent]]. In this case, a **reconstruction loss**, which evaluates how well the network reconstructed the input data, is used. This means that input and target vectors are identical.

The NN setup is thus the one we know from other regression tasks (final linear layer, MSE loss, gradient descent training). We are usually interested in the input representation in the hidden layer(s), which are usually smaller than the input/output layers.

## Observations

- If all layers are linear: the **AE** computes a linear projection on a subspace whose dimensionality is given by the size of the smallest inner layer. This linear projection spans the same space as **PCA**, but the NN is not constrained to find an orthonormal basis.
- If the inner layer is greater than the input, the autoencoder might simply learn the identity function.
- In some cases, each input sample is reconstructed (almost) perfectly, but the autoencoder learns no interesting structure, as there may still be some overfitting. For that, [[The denoising Autoencoder]] comes in handy.


# Autoencoder Conclusions

- A major drawback is the un-interpretability of the hidden representation. In particular, one would like a representation that disentangles factors of variation.
- More advanced autoencoders exist, like the **variational autoencoder**.
- [[The denoising Autoencoder]] is nothing but a standard [[Neural Networks]] regressor.
- Autoencoders tend to remove small-scale details, as they have high-dimensional character and do not "fit" into the low-dimensional encoding.
- Small details are suppressed as they are high-dimensional and contribute little to the **MSE**.

The **Variational Autoencoder** and its variants aim at solving the latter problem. However, we do not cover these techniques here.

The **Autoencoder** framework is more general than the [[PCA]] framework. The **denoising autoencoder** is nothing but a standard neural network regressor.

### Why a standard autoencoder tends to remove small-scale details?

The small details have high-dimensional character and do not "fit" into the low-dimensional encoding. Small details are suppressed as they are high-dimensional and contribute little to the **MSE**.
