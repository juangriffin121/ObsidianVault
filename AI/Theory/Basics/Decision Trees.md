Makes decisions checking each feature making a threshold for features, the main feature is at the node and is the first decision you have to make, then further nodes are separated with other features 

```python
from sklearn.datasets import load_iris
from sklearn.tree import DecisionTreeClassifier
iris = load_iris(as_frame=True)
X_iris = iris.data[["petal length (cm)", "petal width (cm)"]].values
y_iris = iris.target
tree_clf = DecisionTreeClassifier(max_depth=2, random_state=42)
tree_clf.fit(X_iris, y_iris)
```

You can visualize the trained decision tree by first using the export_graphviz() function to output a graph definition file called iris_tree.dot:
```python
from sklearn.tree import export_graphviz
export_graphviz(
 tree_clf,
 out_file="iris_tree.dot",
 feature_names=["petal length (cm)", "petal width (cm)"],
 class_names=iris.target_names,
 rounded=True,
 filled=True
 )
```
to show it in jupyter
```python
from graphviz import Source
Source.from_file("iris_tree.dot")

```

a node’s gini attribute measures its Gini impurity: a node is “pure” (gini=0) if all training instances it applies to belong to the same class.
gini impurity of the ith node:
$$G_i = 1 - \sum_k (p_{i,k})^2$$
pi,k is the ratio of class k instances among the training instances in the i th node.

>[!info]
>Scikit-Learn uses the CART algorithm, which produces only binary trees, meaning trees where split nodes always have exactly two children (i.e., questions only have yes/no answers). However, other algorithms, such as ID3, can produce decision trees with nodes that have more than two children.

A decision tree can also estimate the probability that an instance belongs to a partic‐ ular class k. First it traverses the tree to find the leaf node for this instance, and then it returns the ratio of training instances of class k in this node.

#### CART
Scikit-Learn uses the Classification and Regression Tree (CART) algorithm to train decision trees (also called “growing” trees). The algorithm works by first splitting the training set into two subsets using a single feature k and a threshold tk (e.g., “petal length ≤ 2.45 cm”). How does it choose k and tk ? It searches for the pair (k, tk ) that produces the purest subsets, weighted by their size. 
The cost function that the algorithm tries to minimize:

$$E = \frac {m_{left}} {m}G_{left} + \frac {m_{right}} {m}G_{right}$$
G is the Gini index of each subset and m is the length of each subset or the entire set to be divided.

Once the CART algorithm has successfully split the training set in two, it splits the subsets using the same logic, then the sub-subsets, and so on, recursively. It stops recursing once it reaches the maximum depth or if it cannot find a split that will reduce impurity. A few other hyperparameters (described in a moment) control additional stopping conditions: min_samples_split, min_samples_leaf, min_weight_fraction_leaf, and max_leaf_nodes.
>[!info]
>As you can see, the CART algorithm is a greedy algorithm: it greed‐ ily searches for an optimum split at the top level, then repeats the process at each subsequent level. It does not check whether or not the split will lead to the lowest possible impurity several levels down. A greedy algorithm often produces a solution that’s reasonably good but not guaranteed to be optimal. Unfortunately, finding the optimal tree is known to be an NPcomplete problem.1 It requires O(exp(m)) time, making the prob‐ lem intractable even for small training sets. This is why we must settle for a “reasonably good” solution when training decision trees.

Instead of Gini impurity we can use [[Information Entropy]] as a measure, they usually yield similar results, when they differ, Gini impurity tends to isolate the most frequent class in its own branch of the tree, while entropy tends to produce slightly more balanced trees.

#### Regularization
Decision trees make very few assumptions about the training data (as opposed to lin‐ ear models, which assume that the data is linear, for example). If left unconstrained, the tree structure will adapt itself to the training data, fitting it very closely—indeed, most likely overfitting it.

To avoid overfitting the training data, you need to restrict the decision tree’s freedom during training. As you know by now, this is called regularization.