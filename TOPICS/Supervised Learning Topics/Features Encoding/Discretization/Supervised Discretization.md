[[Discretization]] using class labels
- Entropy based approach
	- The entropy is calculated based on the class label: how consistently a potential partition will match up with a classifier decider output 
	- It finds the best split so that the bins are as pure as possible that is the majority of the values in a bin correspond to have the same class label
	- Lower entropy is better, and a 0 value for entropy is the best 
	- Same reasoning is at the base of methodologies to make meaningful split in a continuous variables (ex: decision trees)