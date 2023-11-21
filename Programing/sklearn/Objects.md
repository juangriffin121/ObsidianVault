FunctionTransformer:
```python
from sklearn.preprocessing import FunctionTransformer
log_transformer = FunctionTransformer(np.log, inverse_func=np.exp)
log_pop = log_transformer.transform(housing[["population"]])
#or
ratio_transformer = FunctionTransformer(lambda X: X[:, [0]] / X[:, [1]]) ratio_transformer.transform(np.array([[1., 2.], [3., 4.]]))
```

Make a custom transformer:
This implementation is not 100% complete: all estimators should set feature_names_in_ in the fit() method when they are passed a DataFrame. Moreover, all transformers should provide a get_feature_names_out() method, as well as an inverse_transform() method when their transformation can be reversed. See the last exercise at the end of this chapter for more details.
```python
from sklearn.base import BaseEstimator, TransformerMixin 
from sklearn.utils.validation import check_array, check_is_fitted 
class MyTransformer(BaseEstimator, TransformerMixin):
	def __init__(self, hyperparams = Default): # no *args or **kwargs! 
		self.hyperparams = hyperparams 
	def fit(self, X, y=None): # y is required even if not used
		X = check_array(X) # checks that X is an array with finite float values
		self.estimated_params_ = estimate_params(X,self.hyperparams) #can be separate or here 
		self.n_features_in_ = X.shape[1] # every estimator stores this in fit() 
		return self # always return self! 
	def transform(self, X): 
		check_is_fitted(self) # looks for learned attributes (with trailing _) 
		X = check_array(X) 
		assert self.n_features_in_ == X.shape[1] 
		X = transform_dataset(X,self.hyperparams,self.params_) #can be separate or here 
		return X
```

Pipelines:
[[Pipeline]]