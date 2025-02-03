[[Autoencoder]]

During training, the input data is corrupted by noise (e.g. Gaussian noise, random masking, etc).
The target is the uncorrupted version of the input. In the test phase, the uncorrupted input data
is fed into the network to extract high-level representations.