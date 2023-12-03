An important theoretical result of statistics and machine learning is the
fact that a model’s generalization error can be expressed as the sum of
three very different errors:
Bias
This part of the generalization error is due to wrong assumptions,
such as assuming that the data is linear when it is actually quadratic.
A high-bias model is most likely to underfit the training data.
Variance
This part is due to the model’s excessive sensitivity to small
variations in the training data. A model with many degrees of
freedom (such as a high-degree polynomial model) is likely to have
high variance and thus overfit the training data.
Irreducible error
This part is due to the noisiness of the data itself. The only way to
6
reduce this part of the error is to clean up the data (e.g., fix the data
sources, such as broken sensors, or detect and remove outliers).
Increasing a model’s complexity will typically increase its variance and
reduce its bias. Conversely, reducing a model’s complexity increases its
bias and reduces its variance. This is why it is called a trade-off