
It shifts and rescales the data such that all features are exactly between 0 and 1

$x_{new} = \frac{x-x_{min}}{x_{max}-x_{min}}$
- for the two-dimensional dataset this means all of the data is contained within the rectangle created by the x-axis between 0 and 1 and the y-axis between 0 and 1

![[MinMaxScaler.png]]