Linear regression regularized to keep small weights
$$ loss = MSE + \alpha\sum \limits_i w_i^2$$
>[!info] 
>It is important to scale the data (e.g., using a StandardScaler) before performing ridge regression, as it is sensitive to the scale of the input features. This is true of most regularized models

This type of regression has a close form solution or can be solved with SGD
