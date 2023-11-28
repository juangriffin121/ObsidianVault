Estimator that takes in a predictor and when fit is called it optimizes the hyperparameters, it does it in different splits ([[CrossValidation]] CV).
```python
from sklearn.model_selection import GridSearchCV
full_pipeline = Pipeline([
	("preprocessing", preprocessing),
	("random_forest", RandomForestRegressor(random_state=42)),
])
param_grid = [
	{'preprocessing__geo__n_clusters': [5, 8, 10],
	'random_forest__max_features': [4, 6, 8]},
	{'preprocessing__geo__n_clusters': [10, 15],
	'random_forest__max_features': [6, 8, 10]},
]
grid_search = GridSearchCV(full_pipeline, param_grid, cv=3,
						   scoring='neg_root_mean_squared_error')
grid_search.fit(housing, housing_labels)

```
>[!info]
>Notice that you can refer to any hyperparameter of any estimator in a pipeline, even if this estimator is nested deep inside several pipelines and column transformers. For example, when Scikit-Learn sees "preprocessing\__geo__n_clusters", it splits this string at the double underscores, then it looks for an estimator named "preprocessing" in the pipeline and finds the preprocessing ColumnTransformer. Next, it looks for a transformer named "geo" inside this ColumnTransformer and finds the ClusterSimilarity transformer we used on the latitude and longitude attributes. Then it finds this transformer’s n_clusters hyperparameter.
