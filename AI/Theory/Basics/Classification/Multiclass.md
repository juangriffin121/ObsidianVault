You can use [[binary]] classifiers one for each class and then use all of them (one vs rest/all OvR/ OvA), or use natively multiclass classifiers.
You can also use binary classifiers the distinguishes pairs of classes, in that case you need N*(N-1)/2 classifiers but each classifier is trained on a fraction of the dataset (one vs one OvO) and classify the input with the class that wins the most duels.
"Some algorithms (such as support vector machine classifiers) scale poorly with the size of the training set. For these algorithms OvO is preferred because it is faster to train many classifiers on small training sets than to train few classifiers on large training sets. For most binary classification algorithms, however, OvR is preferred."
[[SciKitLearn]]

