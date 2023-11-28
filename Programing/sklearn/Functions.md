train_test_split(): random choosing the [[Train and Test sets]] with a seed we can choose for replicability.
```python
train_set, test_set = train_test_split(dataset, test_size=0.2, random_state=42)
```
StratifiedShuffleSplit(): Used to get many splits representative of the whole dataset by containing roughly equal percentages of datapoints in every bin (strata) created from the splitting column. Returns a splitter object which has a split() method that takes in the dataset and 
```python
splitter = StratifiedShuffleSplit(n_splits=5, test_size=0.2, random_state=69)
for train_index, test_index in splitter.split(Dataset,Dataset['splitting column']):
    sub_train = train.iloc[train_index]
    sub_test = train.iloc[test_index]
```
SimpleImputer: replases missing values from numerical data to their mean  ^cfd672
```python
imputer = SimpleImputer(strategy="median")
numerical_dataset = Dataset.select_dtypes(include=[np.number])
imputer.fit(numerical_dataset)
X = imputer.transform(numerical_dataset)
transformed_dataset = pd.DataFrame(X, columns=numerical_dataset.columns, index=numerical_dataset.index)
```

OneHotEncoder transformer that turns the categorical dataset into one hot encoding SciPy sparse matrix
```python
from sklearn.preprocessing import OneHotEncoder 
cat_encoder = OneHotEncoder()
Dataset_1hot = cat_encoder.fit_transform(categorical_dataset)
```

MinMaxScaler: ^32b04d
```python
from sklearn.preprocessing import MinMaxScaler 
min_max_scaler = MinMaxScaler(feature_range=(-1, 1)) housing_num_min_max_scaled = min_max_scaler.fit_transform(housing_num)
```
StandardScaler: ^ce7dae
```python
from sklearn.preprocessing import StandardScaler 
std_scaler = StandardScaler() housing_num_std_scaled = std_scaler.fit_transform(housing_num)
```

MyTransformer.get_feature_names_out:
returns the names after the transformation

cross_val_score:
(see [[CrossValidation]])

Takes in a predictor, the input dataset and the labels and performs the training of the predictor on training sets randomly splitted from the dataset, and then tests the trained model on the test subsets. 
> [!info]
> Scikit-Learn’s cross-validation features expect a utility function (greater is better) rather than a cost function (lower is better), so the scoring function is actually the opposite of the RMSE. It’s a negative value, so you need to switch the sign of the output to get the RMSE scores.
```python
from sklearn.ensemble import RandomForestRegressor
from sklearn.model_selection import cross_val_score

forest_reg = make_pipeline(preprocessing,
						   RandomForestRegressor(random_state=42))
forest_rmses = -cross_val_score(forest_reg, housing, housing_labels,
								scoring="neg_root_mean_squared_error", cv=10)
```
