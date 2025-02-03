
It uses the median (50th percentile) and IQR Interquartile range (75th percentile - 25th percentile) instead of mean and variance

$x_{new}=\frac{x - median}{IQR}$
The resulting variable has a zero mean and median and a standard deviation of 1

- Ignore the data points that are very different from the rest, which are considered in the calculation of mean and variance
	- these odd data points are outliers and can lead to trouble for other scaling techniques
- Resulting data is not skewed by outliers
	- the outliers are still present with the same relative relationships to other values
![[RobustScaler.png]]
