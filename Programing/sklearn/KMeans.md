```python
from sklearn.cluster import KMeans
model = KMeans(n_clusters=i, random_state=69, n_init=10).fit(X_train)
model.inertia_

```