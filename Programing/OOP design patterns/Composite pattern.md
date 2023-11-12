Treats an object and a composition of objects uniformly. work with complex structures as if they were individual objects.

```python
class IndividualObject:
	def init(self,name):
		self.name = name

class Composite:
	def init(self, name):
		self.name = name
		self.contents = []
	def add(self, entry):
		self.contents.append(entry)
```

Example sklearn transformers