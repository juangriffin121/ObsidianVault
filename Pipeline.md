The Pipeline constructor takes a list of name/estimator pairs (2-tuples) defining a sequence of steps. The names can be anything you like, as long as they are unique and don’t contain double underscores. They will be useful later, when we discuss hyperparameter tuning. The estimators must all be transformers (i.e., they must have a fit_transform() method), except for the last one, which can be anything: a transformer, a predictor, or any other type of estimator.
```python
from sklearn.pipeline import Pipeline
my_pipeline = Pipeline([
	("first_transformer", Transformer1()), #the Transformer object can be instanciated inside or not
	("second_transformer", Transformer2()), ...
	("final_estimator",Estimator())#can be just estimator, transformer or predictor
])
```
If you don’t want to name the transformers, you can use the make_pipeline() function instead; it takes transformers as positional arguments and creates a Pipeline using the names of the transformers’ classes, in lowercase and without underscores (e.g., "simpleimputer"):
```python
from sklearn.pipeline import make_pipeline
my_pipeline = make_pipeline(Transformer1(), Transformer2())
```
The pipeline exposes the same methods as the final estimator. In this example the last estimator is a StandardScaler, which is a transformer, so the pipeline also acts like a transformer. If you call the pipeline’s transform() method, it will sequentially apply all the transformations to the data. If the last estimator were a predictor instead of a transformer, then the pipeline would have a predict() method rather than a transform() method. Calling it would sequentially apply all the transformations to the data and pass the result to the predictor’s predict() method.

Pipelines support indexing; for example, pipeline[1] returns the second estimator in the pipeline, and pipeline[:-1] returns a Pipeline object containing all but the last estimator. You can also access the estimators via the steps attribute, which is a list of name/estimator pairs, or via the named_steps dictionary attribute, which maps the names to the estimators. For example, num_pipeline["simpleimputer"] returns the estimator named simpleimputer