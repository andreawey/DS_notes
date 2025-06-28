- A SVM is a supervised machine learning algorithm used for:
	- Classification: Binary or multiclass
	- Regression: predicting continuous values
- SVM aim to find the optimal hyperplane that separates data points belonging to different classes
- The decision boundary is chosen to maximize the margin between the classes

![[SVM.png]]

- The hyperplane separates the two classes
- The support vectors are the closest points to the hyperplane lying on the margin boundaries (dashed lines)
- The margin size is determined by the perpendicular distance between the hyperplane and these margin boundaries

## SVM Hyperplane
- A hyperplane in SVM is defined as: $$w*x+b=0$$
- Here:
	- $w:$ The normal vector (perpendicular to the hyperplane)
	- $x:$ The feature vector
	- $b:$ The bias term
- The hyperplane separates the data into two classes $$w*x+b>= \text{ (Class 1) and } w*x+b <0 \text{ (Class 2)} $$
## SVM Distance
- The perpendicular distance from any point $x_i$ to the hyperplane is given by: $$Distance = \frac{|w*x_i+b|}{||w||}$$
- The denominator $||w||$ normalizes the distance relative to the orientation of the hyperplane
- Taking the absolute value ensures we only measure distance magnitude, regardless of side
- This formula gives the **absolute distance** from $x_i$ to the hyperplane
- The signed version $\frac{w*x_i+b}{||w||}$ determines on which side of the hyperplane the point lies

## SVM Margin
- In SVM the margin is defined as the distance between the two margin boundaries:
	- The upper margin boundary $(f(x) = +1)$
	- The lower margin boundary $(f(x) =-1)$
- The distance from the hyperplane to one margin boundary is: $$\frac{1}{||w||}$$
- The total margin is: $$\text{Margin Size}= \frac{1}{||w||} + \frac{1}{||w||} =  \frac{2}{||w||} $$
- SVM maximizes the margin by solving the following optimization problem: $$\min_{w, b} \frac{1}{2} \|w\|^2$$
- subject to $y_i(w*x_i +b) \geq 1$
	- where $y_i$ class label of the i-th data point (+1 or -1)

- This ensures all points are correctly classified ant he margin is maximized

>[!FAQ] SVM
>In SVM, we find an optimal hyperplane such that:
>- Support vectors lie exactly on the margin boundaries.
>- The margin boundaries are parallel to the optimal hyperplane.
>	- Positive hyperplane, where the support vectors of the positive class lie
>	- Negative hyperplane, where the support vectors of the negative class lie
>- The margin (distance between the boundaries) is maximized.

