As we have discussed, a random forest is an ensemble of decision trees, generally
trained via the bagging method (or sometimes pasting), typically with max_samples
set to the size of the training set. Instead of building a BaggingClassifier and
passing it a DecisionTreeClassifier, you can use the RandomForestClassifier
class, which is more convenient and optimized for decision trees.

```python
from sklearn.ensemble import RandomForestClassifier
rnd_clf = RandomForestClassifier(n_estimators=500, max_leaf_nodes=16,
 n_jobs=-1, random_state=42)
rnd_clf.fit(X_train, y_train)
y_pred_rf = rnd_clf.predict(X_test)

```

### Feature importance

Yet another great quality of random forests is that they make it easy to measure the
relative importance of each feature. Scikit-Learn measures a feature’s importance by
looking at how much the tree nodes that use that feature reduce impurity on average,
across all trees in the forest. More precisely, it is a weighted average, where each
node’s weight is equal to the number of training samples that are associated with it
