[[Classification anomaly detection]]

Optimization and Classification with Fixed Feature Transformation

1. With a fixed feature transformation $\Phi$, optimize:
2. This optimization can be done efficiently using only the kernel corresponding to the feature $\Phi$.
3. For classifying a sample $x$ as normal, check whether $||\Phi(x) \cdot c||^2_{R^2}$ (which can also be formulated using kernels).
