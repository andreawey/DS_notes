[[Anomaly, outlier and novelty detection]]

Very basic idea: fit a probability distribution $P_+(x)$, for example the Gaussian distribution, to the data. The key assumption of probabilistic AD is that the normal data follows the model probability distribution. Note: If the distribution $P_+(x)$ is Gaussian, the method is equivalent to thresholding the Mahalanobis distance ([[Distance measures]]) of a data point to the mean of the set of (normal) training data.

## Advantages of Probabilistic AD
- The probability of a possibly anomalous sample can be used as a rank of anomalousness (the less probable, the more anomalous).
- If the underlying model assumption holds, probabilistic AD gives mathematically justified results.
- Tendentially, probabilistic methods are good at smoothing outliers in the training data.

## Disadvantages of Probabilistic AD
- Often, it is difficult to find a good model for the data, especially in high-dimensional settings. A bad model greatly impacts the quality of the algorithm.
- The distribution estimation may fail or be inaccurate (again, because of the curse of dimensionality).
- In case there are known examples of anomalies, there is no way to use them in the model estimation.

[[Classic Density Estimation algorithm]]