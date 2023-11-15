```python
from sklearn.svm import SVC
svm_clf = SVC(random_state=42) 
svm_clf.fit(X_train[:2000], y_train[:2000])
```
For [[Multiclass]] classification problems you can use the SVM from sklearn and sklearn detects when you try to use a binary classifier for multiclass and chooses OvA or OvO accordingly.
