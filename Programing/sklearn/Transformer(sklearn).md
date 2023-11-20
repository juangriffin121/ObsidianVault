Some estimators (such as a SimpleImputer [[Preprocessing functions#^cfd672]]) can also transform a dataset; these are called transformers. Once again, the API is simple: the transformation is performed by the transform() method with the dataset to transform as a parameter. It returns the transformed dataset. This transformation generally relies on the learned parameters, as is the case for a SimpleImputer. All transformers also have a convenience method called fit_transform(), which is equivalent to calling fit() and then transform() (but sometimes fit_transform() is optimized and runs much faster).

```python
class MyTransformer(BaseEstimator, TransformerMixin):
	def __init__(self, hyperparams):
		self.hyperparams = hyperparams
	def fit(self,X,y=None):
		params = estimate_params()
		self.params_ = params
		return self
	def transform(self,X):
		# transform X
		return X
```

my examples:
```python
class Honorifics(BaseEstimator,TransformerMixin):
    def __init__(self):
        pass
    
    def fit(self,X,y=None):
        return self
    
    def transform(self,X):
        honorifics = X["Name"].str.extract(r'.*, (\w*)\.')
        honorifics[~honorifics.isin(("Mr","Miss","Master","Mrs"))] = "Other"
        honorifics.columns = ["honorific"]
        return honorifics
    
    def get_feature_names_out(self,name = None):
        return ["honorific"]
```
in this case we want to use a categorical column to impute the values of columns with empty values with the mean for each category.
This case is a good example of estimator and transformer since the fit method calculates the parameters it needs (the means) and the transformer changes the dataset using those values.
```python
class GroupImputer(BaseEstimator,TransformerMixin):
    
    def __init__(self, gcol_name,):
	    # init always just sets the hyperparams as instance variables 
		# here we save the name of the column we will use to impute others as gcol_name
        self.gcol_name = gcol_name
    
    def fit(self,X,y = None):
	    #this method always calculates the parameters needed to perform further operations
		# here we calculate the means of each numerical column of the dataset other than groupcol for each category in groupcol
		# we save the information in a dataframe means which is means[cat][col] is the mean of the cases in column col where their category in gcol is cat
        self.names = X.columns
        groupcol = X.loc[:,self.gcol_name]
        self.groupcol = groupcol
        self.cats = groupcol.unique()
        means = {}
        self.remaining = X.loc[:,X.columns != self.gcol_name]
        for cat in self.cats:
            means[cat] = {}
            for col in self.remaining:
                means[cat][col] = X[X.loc[:,self.gcol_name] == cat].loc[:,col].mean()
        self.means = pd.DataFrame(means).transpose()
        return self
    
    def transform(self,X):
		#Replace the nan columns in the numerical dataset with the mean for that column in the category the nan entry has in gcol
        for col in self.remaining:
            nancol_df = X.loc[X.loc[:,col].isna(), :]
            mean_col = self.means.loc[nancol_df.loc[:,self.gcol_name]].reset_index()
            X.loc[X.loc[:,col].isna(), :] = mean_col.values
        return X
    
    def get_feature_names_out(self,Names = None):
        return [name for name in self.names]
```