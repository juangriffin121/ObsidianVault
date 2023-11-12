Defines a family of algorithms, and makes them interchangeable

```python
class BaseStrategy:
	def algorithm():
		pass

class Strategy1(BaseStrategy):
	def algorithm():
		return f"done with algorithm  1"

class Strategy2(BaseStrategy):
	def algorithm():
		return f"done with algorithm  2"
```
Examples layers in [[Wkiki]], payment methods(paid with paypal paid with crefdit card have the same pay method)