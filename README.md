# pythonLearning
how to use __new__
when need to change the input at the very beginning
Example:

```python
class CapStr(str):
	def __new__(cls,string):
		string=string.upper()
		return str.__new__(cls,string)
````
