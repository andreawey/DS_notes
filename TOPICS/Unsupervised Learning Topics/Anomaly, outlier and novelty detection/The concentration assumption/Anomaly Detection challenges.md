[[Anomaly, outlier and novelty detection]]


Typically, no samples of the “anomalous” class are available, hence the structure of the “anomalous” class is also unknown: is it bounded or unbounded in the feature space, how many modes does it have, etc. This also means when we train the concept of normality, we can never be sure that we have no undetected anomalous samples in the dataset. The variability within the “normal” class can likewise be very large (for example, person-to-person variability in biomedical datasets), or can be non-stationary. 

Anomality is not a binary concept: there may be varying degrees of anomalousness. If anomalous examples are available, this can greatly improve the quality of the AD system! Finally, a system may also detect its own regions of uncertainty and ask the environment, or even a human supervisor, for additional normal or anomalous examples (active learning).
