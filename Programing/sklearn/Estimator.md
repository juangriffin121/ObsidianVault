Any object that can estimate some parameters based on a dataset is called an estimator (e.g., a SimpleImputer is an estimator). The estimation itself is performed by the fit() method, and it takes a dataset as a parameter, or two for supervised learning algorithms—the second dataset contains the labels. Any other parameter needed to guide the estimation process is considered a hyperparameter (such as a SimpleImputer’s strategy), and it must be set as an instance variable (generally via a constructor parameter).

```python
class MyEstimator(BaseEstimator):
	def __init__(self,hyperparams = default):
		self.hyperparams = hyperparams
		pass

	def fit(self,X,y=None):
		params = estimate_params()
		self.params_ = params
		return self
```