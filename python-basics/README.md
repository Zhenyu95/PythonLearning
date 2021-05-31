# Python Basics

## `lambda`

```python
lambda argument_list : expression
```

`lambda` takes `argument_list` as input and returns `expression`

eg.

```python
#inputs are x & y, output is x*y
lambda x, y: x*y
#no input and output is None
lambda:None
#inputs are *args and output is the sum of inputs
lambda *args: sum(args)
```

## `map`

```python
map(function, iterable)
```

**`map()`**function returns a **map object**\(which is an iterator\) of the results after applying the given function to each item of a given iterable \(list, tuple etc.\)

## `*args` & `**kwargs`

_`*args`_ in function definitions is to pass a number of variables to a function.

_`**kwargs`_ in function definitions is to pass a keyworded, variable-length argument list.

```python
# *args with first extra argument 
def myFun(arg1, *argv): 
	print ("First argument :", arg1) 
	for arg in argv: 
		print("Next argument through *argv :", arg) 

myFun('Hello', 'Welcome', 'to', 'GeeksforGeeks') 


# **kargs for variable number of keyword arguments
def myFun(**kwargs): 
	for key, value in kwargs.items(): 
		print ("%s == %s" %(key, value)) 

# Driver code 
myFun(first ='Geeks', mid ='for', last='Geeks')	 

#output
#last == Geeks
#mid == for
#first == Geeks
```

## `assert`

```python
x = "hello"

#if condition returns True, then nothing happens:
assert x == "hello"

#if condition returns False, AssertionError is raised:
assert x == "goodbye"
```

## Set entries of a matrix to 0 and 1 based on a threshold

判断列表中的每一个元素，如果大于阈值则1 否则为0

```python
list = np.random.randn(5,5)
result = (list > threshold)
```

```python
list = np.random.randn(5,5)
result = (list > threshold).astype(int)
```

## Pass by reference

{% hint style="info" %}
Python dictionaries and lists are "pass by reference", which means that if you pass a dictionary into a function and modify the dictionary within the function, this changes that same dictionary \(it's not a copy of the dictionary\).
{% endhint %}

## `vars()`

Python classes have an attribute called `__dict__`. `__dict__` is a Python dictionary that contains the object's instance variables and values as key-value pairs. 

`vars()` will call the object's `__dict__` attribute.

```python
# Define a small class MyClass
class MyClass:
    def __init__(self):
        # One class variable 'a' is set to 1
        self.var1 = 1

# Create an object of type MyClass()
my_obj = MyClass()

my_obj.__dict__
>>> {'var1': 1}
vars(my_obj)
>>> {'var1': 1}

# Add a new instance variable and give it a value
my_obj.var2 = 2
# Calls vars() again to see the object's instance variables
vars(my_obj)
>>> {'var1': 1, 'var2': 2}

# Call vars, passing in the object.  
# Then access the __dict__ dictionary using square brackets
vars(my_obj)['var3'] = 3
# Call vars() to see the object's instance variables
vars(my_obj)
>>> {'var1': 1, 'var2': 2, 'var3': 3}
```



