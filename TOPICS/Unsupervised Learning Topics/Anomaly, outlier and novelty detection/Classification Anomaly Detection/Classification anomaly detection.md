[[Anomaly, outlier and novelty detection]]

Idea: instead of estimating an entire probability distribution, compute just one density set, challenge: regularization is needed to avoid all samples predicted as normal.

# Goal and Approach for One-Class Decision Boundary

**Goal**: Learn a one-class decision boundary which minimizes:
- False alarms for samples which are actually normal (type I error),
- Misses of anomalous samples which are detected as normal (type II error).

**Approach**: Specify a target false alarm rate $\alpha$ and minimize the miss rate based on this constraint.

# Advantages of Classification-Based Anomaly Detection

- Classification-based techniques can use well-established, powerful algorithms.
- They can incorporate information about several normal classes, and in particular, they can easily incorporate anomalous samples during training.
- Fast testing phase (depends on the model though).

# Disadvantages of Classification-Based Anomaly Detection

- There is no principled way to choose the regularization method.
- There is no direct way to obtain scores which reflect degrees of anomalousness.


Classification Anomaly Detection
	- [[Classification anomaly detection]]
	- [[Anomaly detection with support vector data algorithm]]