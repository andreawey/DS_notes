[[Anomaly, outlier and novelty detection]]

Like we have done in the project, the autoencoder, based on either the **manifold assumption** (you can meaningfully reduce to a lower dimension the dataset), or the **prototype assumption**, there is a finite set of prototypical data which summarizes well the whole set. This suggests clustering as a valuable technique.

Reconstruction Anomaly Detection
	- [[Reconstruction Anomaly Detection]]
	- [[Regularization on encoder-decoder]]

## Advantages

The key challenge of reconstruction-based anomaly detection (AD) is to find the right latent dimension.

- Access to a wide range of models.
- Allows to obtain additional information from the hidden representation, might even allow to explain what is wrong with an anomalous example.

## Disadvantages

- In reconstruction with a small latent space, there is no principled way to determine the optimal dimension of the latent space.
- There is no way to incorporate anomalous examples in training.