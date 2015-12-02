# Errors and Exceptions #

## 1. Handle Exceptions ##
* `try ... except`

```python
while True:
	try:
		x = int(raw_input("Please enter a number: "))
		break
	except ValueError:
		print "Oops! That was no valid number. Try again..."
```

* `try` may have more than one `except`
* `except` clause may have multiple exceptions as a tuple 
	```python
	try:
		# something
	except (RuntimeError, TypeError, NameError): 
		pass
	```

* last except clause may omit exception name, wildcard. Use to print error message, re-raise the exception

	```python
	import sys

	try:
		f = open('myfile.txt')
		s = f.readline()
		i = int(s.strip())
	except IOError as e:
		print "I/O error({0}): {1}".format(e.errno, e.strerror)
	except ValueError:
		print "Could not convert data to an integer."
	except:
		print "Unexpected error:", sys.exc_info()[0]
		raise
	```
* optional `else`
	* must follow all except clauses
	* useful for code that must be executed if the try clause does not raise an exception
	
```python
for arg in sys.argv[1:]:
	try:
		f = open(arg, 'r')
	except IOError:
		print 'cannot open', arg
	else:
		print arg, 'has', len(f.readlines()), 'lines'
		f.close
```

* Exceptions's *argument*: an associated value with an exception
	* Not mandatory
	* presence and type of the argument depend on the exception type

* A except clause may specify a variable after the exception name (or tuple), and the variable is bound to an exception instance with the arguments stored in `instance.args`

	```python
	try:
		raise Exception('spam', 'eggs')
	except Exception as inst:
		print type(inst)
		print inst.args
		print inst
		x, y = inst.args
		print 'x =', x
		print 'y =', y
	```

## 2. Raising Exceptions ##
* `raise` allows programmer to force a specified exception to occur

	```python
	raise NameError('HiThere')
	```

## 3. User-defined Exceptions ##
* Derived from `Exception` class, directly or indirectly

	```python
	class MyError(Exception):
		def __init__(self, value):
			self.value = value
		def __str__(self):
			return repr(self.value)

	try:
		raise MyError(2*2)
	except MyError as e:
		print 'My exception occurred, value:', e.value
	```
* A module raises serveral distinct errors
	* create a base class for exceptions
	* subclass the base to create specific exception classes

* Most exceptions' names end with "Error"

## 4. Clean-up Actions ##
* `finally`, always executed before leaving try statement
* useful for releasing external resources

	```python
	def divide(x, y):
		try:
			result = x / y
		except ZeroDivisionError:
			print "division by zero!"
		else:
			print "result is", result
		finally:
			print "executing finally clause"

	divide(2, 1)
	divide(2, 0)
	divide("2", "1")
	```

## 5. Predefined Clean-up Actions ##

```python
with open("myfile.txt") as f:
	for line in f:
		print line,

# f is always closed after the statement
```
