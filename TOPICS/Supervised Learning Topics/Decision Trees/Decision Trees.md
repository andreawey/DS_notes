Widely used models for classification and regression tasks
They learn a hierarchy of if/else questions, leading to a decision

- each node in the tree either represents a question or a terminal node (also called leaf) that contains the answer
- The edges connect the answers to a question with the next question you would ask

<h1>Node's samples</h1>
counts how many training instances it applies to 

<h1>Node's value</h1>
tells how many training instances of each class this node applies to 

<h1>Node's Gini</h1>
measures its impurity
- a node is pure (gini = 0) if all training instances it applies to belong to the same class



> [!NOTE] Remark
> Decision trees require very little data preparation
> -> they do not require feature scaling or centering at all
>The decision tree CART algorithm (Training Algorithm), produces binary trees
>-> non-leaf nodes always have two children 
>Other algorithms can produce Decision Trees with nodes that have more than two children


- Decision trees are fairly intuitive and their decisions are easy to interpret 
- Such models are often called white box models
- n contrast, Random Forests or neural networks are generally considered black box models
	- they make great predictions and you can easily check the calculations that they performed to make these predictions, nevertheless it is usually hard to explain in simple terms why the predictions were made

<h1>Classification and Regression algorithm - CART</h1>
- this is one of the algorithms used to train Decision Trees, aka growing trees
- The algorithm first splits the training set in two subsets using a single feature k and the threshold $t_k$
- the pair $(k, t_k)$ is chosen in order to produce the purest subsets 
- Once it has successfully split the training set in two it splits the subsets using the same logic
- It recursively splits the sub-subsets until it reaches a stop condition
- Different stop conditions might be defined
	- maximum depth reached
	- the algorithm cannot find a split that will reduce impurity
	- a node reached the minimum number of samples a node must have before it can split, or the minimum number of samples a leaf node must have
	- the model reached the maximum number of leaf nodes



> [!NOTE] CART algorithm
> - CART algorithm is a greedy algorithm: it greedily searches foe an optimum split at the top level, then repeats the process at each level 
> - It does not check whether or not the split will lead to the lowest possible impurity several levels down
> - A greedy algorithm often produces a reasonably good solution, bu it is not guaranteed to be the optimal solution


<h1>Computational complexity</h1>
Making predictions requires traversing the decision tree from the root to a leaf
The training algorithm instead compares all features on all samples at each node

<h1>Gini impurity vs Entropy</h1>
- Both Gini or Entropy might be used as a measure of impurity
- Choosing entropy or gini does not make a big difference most of the time they lead to similar trees
- Gini impurity is slightly faster to compute
- Gini impurity tends to isolate the most frequent class in its own branch of the tree
- Entropy tends to produce slightly more balanced trees

<h1>Regularization Hyperparameters</h1>
- Decision trees make very few assumptions about the training data
- If left unconstrained the tree structure will adapt itself to the training data fitting it very closely and most likely overfitting it
- Such model is often called a nonparametric model: the number of parameters is not determined prior to training so the model structure is free to stick closely to the data
- It is opposed to the parametric model, which has a predetermined number of parameters so its degree of freedom is limited, reducing the risk of overfitting

- To avoid overfitting the data we need to restrict the decision Tree's freedom during training: regularization
- There are different regularization hyperparameters for decision trees: 
	- we can restrict the maximum depth of the decision tree: reducing the depth will regularize the model and thus reduce the risk of overfitting 
	- We can increase the minimum number of samples a node must have before it can be split 
	- We can increase the minimum number of samples a leaf node must have 
	- we can restrict the maximum number of leaf nodes a tree might have

<h2>Pruning</h2>
- Another methodology performed to regularize a tree foresees first the training of the decision tree without restrictions, then pruning unnecessary nodes
- A node whose children are all leaf nodes is considered unnecessary if the purity improvement it provides is not statistically significant
- Standard statistical test are used to estimate the prob. that the improvement is purely the result of chance
- If the probability is higher than a given threshold then the node is considered unnecessary and its children are deleted
- The pruning continues until all unnecessary nodes have been pruned

<h1>Decision trees for regression tasks</h2>
- Decision trees are also capable of performing regression tasks
- Instead of predicting a class in each node, it predicts a value
- The prediction is simply the average target value of the training instances associated to the reached leaf node
- Mean Squared Error (MSE) per leaf is also calculated for the related prediction value



<h1>Pros and Cons</h1>
- Decision Trees are simple to understand and interpret, easy to use, versatile and powerful
- Decision trees have orthogonal decision boundaries (all splits are perpendicular to an axis) which makes them sensitive to training set rotation
- Decision trees are very sensitive to small variations in the training data


