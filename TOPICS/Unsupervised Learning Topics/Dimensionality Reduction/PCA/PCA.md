# Principal Component Analysis (PCA)

[[Unsupervised Learning]]

Principal Component Analysis (PCA) is the linear method for dimensionality reduction, lossy data compression, feature extraction, and also data visualization. Two common definitions:

- Project data (orthogonally) onto a lower-dimensional subspace so that the total variance of the projected data is maximized.
- Project data (orthogonally) onto a lower-dimensional subspace so that the projection cost is minimized.

Both definitions are equivalent. It's assumed the data is centered (mean is 0); otherwise, the projection subspace is just displaced by a fixed vector.

# Notes

Principal components are new variables that are constructed as linear combinations or mixtures of the initial variables. These combinations are done in such a way that the new variables (i.e., principal components) are uncorrelated, and most of the information within the initial variables is squeezed or compressed into the first components.

An important thing to realize is that the principal components are less interpretable and don’t have any real meaning since they are constructed as linear combinations of the initial variables.

- More variance → Larger dispersion of data points along the line → More information.

PCA extracts the most meaningful basis for the given data. It can be used to remove noise because the noise is inherently high-dimensional.

# PCA Considerations

The total variance of the data in the projection subspace will be $\lambda_1 + \cdots + \lambda_M$, and the projection cost will be $\lambda_{M+1} + \cdots + \lambda_D$, where $M$ is the cutoff dimension selected.

PCA is also useful as a data exploration tool: compute the first few PCA dimensions and see what happens. For example, the **Eigenfaces** example.
