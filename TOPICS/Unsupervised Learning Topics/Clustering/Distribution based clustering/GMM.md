[[Distribution based clustering]]
Idea: Each Cluster Defined by a Multivariate Gaussian Distribution

- The assignment probability is given by 

$$
\gamma_{n,k} = \frac{P_{N_k}(x_n)}{\sum_{k=1}^K N_k(x_n)}
$$

As opposed to [[K-Means]], this is a soft assignment: the closer the datapoint is to the mean of the cluster’s distribution, the more likely it is assigned to that cluster.

- The updating objective is maximizing the likelihood function 

$$
L = \prod_{n=1}^{N} P_{\gamma_{n,k}} N(x_n | \mu_k, \Sigma_k)
$$

Where $\mu_k$ and $\Sigma_k$ are the mean and covariance for cluster $k$. Like for [[K-Means]], there is no closed solution, and the same trick of fixing one step and deriving the other can be used, in what is called the [[EM algorithm]].
