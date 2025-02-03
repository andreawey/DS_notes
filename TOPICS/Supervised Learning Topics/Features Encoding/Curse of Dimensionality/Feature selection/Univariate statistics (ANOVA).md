A tool that can be used for [[Feature selection]] is ANOVA:

We compute whether there is a statistically significant relationship between each feature and the target. Then the features that are related with the highest confidence are selected.


<h2>Analysis of variance(ANOVA)</h2>
It consists in a univariate test: each feature is considered individually.
-> A feature is discarded if it is only informative when combined with another feature

Univariate tests are often very fast to compute and don't require building a model

They are completely independend of the model that we want to apply after the feature selection

Univariate feature selection if very helpful if:

- There is a too large number of features that building a model on them is infeasible
- If we suspect that many features are completely uninformative
