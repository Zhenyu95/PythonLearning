# Python Basics

## lamda

```python
lamda argument_list : expression
```

`lamda` takes `argument_list` as input and returns `expression`

eg.

```python
#inputs are x & y, output is x*y
lambda x, y: x*y
#no input and output is None
lambda:None
#inputs are *args and output is the sum of inputs
lambda *args: sum(args)
```

## `*args` & `*kwargs`

_`*args`_ in function definitions is to pass a number of variables to a function.

_`**kwargs`_ in function definitions is to pass a keyworded, variable-length argument list.

```python
# *args with first extra argument 
def myFun(arg1, *argv): 
	print ("First argument :", arg1) 
	for arg in argv: 
		print("Next argument through *argv :", arg) 

myFun('Hello', 'Welcome', 'to', 'GeeksforGeeks') 


# *kargs for variable number of keyword arguments
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

