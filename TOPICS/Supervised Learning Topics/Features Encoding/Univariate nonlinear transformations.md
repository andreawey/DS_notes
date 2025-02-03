Application of mathematical functions like log, exp or sin to the input feature could bring some benefits.

- Linear models and neural networks are very tied to the scale and distribution of each feature and if there is a nonlinear relation between the feature and the target, that becomes hard to model, particularly in regression
- **Log** and Exp **adjust** the relative scales in the data so that they can be captured better by a **linear model** or **neural network**
- **Sin** and **Cos** functions are useful when dealing with data that encodes periodic patterns.
- Most models work best when each feature is loosely Gaussian distributed 
	- log and exp is a way to achieve this


These transformations are irrelevant for [[Tree-based models]] models, but might be essential for [[Linear models]].


> Summary: 
> - Linear models and naive Bayes models can have clear improvements of the model performances after the application of binning, polynomials and interactions
> - Tree based models are able to discover important interactions themselves and do not require transforming the data explicitly most of the time.
> - SVMs nearest neighbors and neural networks might sometimes benefit from using binning, interactions, or polynomials, but the implications there are less clear than in the case of linear models