S-shaped function that outputs a number between 0 and 1

$\sigma(t)= \dfrac{1}{1+e^{-t}}$


![[Sigmoid function.png]]

Once the Logistic Regression model has estimated the probability $p = h_{\theta}(x)$ that an instance x belongs to the positive class, it can make its prediction $\hat{y}$ easily

$$ \hat{p} = \begin{cases} 0 & \text{if } \hat{p} < 0.5 \\ 1 & \text{if } \hat{p} \geq 0.5 \end{cases} $$
