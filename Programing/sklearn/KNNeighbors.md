Predictor: Finds the k nearest neighbors to the input in the dataset and uses their outputs to make a decision for the output of our case. If the output is categorical it chooses the most chosen category and if its numerical it uses an average or weighted average.
For classification
```python
class sklearn.neighbors.KNeighborsClassifier(n_neighbors=5, *, weights='uniform',
											 algorithm='auto', leaf_size=30, p=2,
											 metric='minkowski', metric_params=None,
											 n_jobs=None)
```
```python
from sklearn.neighbors import KNeighborsClassifier
	knn_clf = KNeighborsClassifier()
    knn_clf.fit(X_train, y_train)
    knn_clf.predict(X_test)
```
For regression
```python
class sklearn.neighbors.KNeighborsRegressor(n_neighbors=5, *, weights='uniform',
											algorithm='auto', leaf_size=30, p=2,
											metric='minkowski', metric_params=None,
											n_jobs=None)
```
