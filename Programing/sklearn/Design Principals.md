Consistency:
	All objects share a consistent interface:
	[[Estimator]]:
	[[Transformer(sklearn)]]
	[[Predictors]]
Inspection:
	All the estimator’s hyperparameters are accessible directly via public instance variables (e.g., imputer.strategy), and all the estimator’s learned parameters are accessible via public instance variables with an underscore suffix (e.g., imputer.statistics_).
Nonproliferation of classes:
	Datasets are represented as NumPy arrays or SciPy sparse matrices, instead of homemade classes. Hyperparameters are just regular Python strings or numbers.
Composition ([[Composite pattern]]):
	Existing building blocks are reused as much as possible. For example, it is easy to create a [[Pipeline]] estimator from an arbitrary sequence of transformers followed by a final estimator, as you will see.
Sensible defaults:
	Scikit-Learn provides reasonable default values for most parameters, making it easy to quickly create a baseline working system
