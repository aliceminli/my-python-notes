# Classes #
* class members (including data members) are *public*
* all member functions are *virtual*
* method function,
	* declared with an **explicit** first argument representing the object
	* the object is provided **implicitly** by the function call

* classes are objects
* Built-in types can be used as base classes
* Operator overloading supported

## 1. Scopes and Namespaces ##

* *namespace*, mapping from names to objects
	* created at different moments, have different lifetimes
	* A module's global namespace, created when the module is read in
	* function's local namespace, created when the function is called, deleted when the function returns or raises exceptions

* *scope*, textual region where a namespace is directly accessible
	* statically determined, dynamically used: during execution, 3 nested scopes whose namespace are directly accessible:
		1. the innermost scope, searched first, contains local names
		2. the scope of any enclosing functions, starting with the nearest enclosing scope, contains non-local, but also non-global names
		3. the next-to-last scope contains the current module's global names
		4. the outermost scope (searched last) in the namespace containing built-in names

## 3. Introduction to Classes ##

#### 3.1 Class Definition ####

```python
class ClassName:
	<statement-1>
	.
	.
	.
	<statement-N>
```
#### 3.3 Class Object ####
* Support two operations: *attribute reference*, *instantiation*
	1. *attribute reference*, `obj.name`; Valid attribute names are all the names in the class's namespace.
		```python
		class MyClass:
			i = 12345
			def f(self):
				return 'hello world'
		```
		* `MyClass.i`, return an integer
		* `MyClass.f`, return a function object

	2. *instantiation*, uses function notation `x = MyClass()`
		* `__init__()`, create objects customized to specific initial state
		
			```python
			def __init__(self):
				self.data = []
			```

		* `__init__()`, may have arguments
		
			```python
			class Complex:
				def __init__(self, real, img):
					self.r = real
					self.i = img

			x = Complex(3.0, -4.5)
			x.r, x.i
			```

#### 3.3 Instance Object ####
* *attribute reference*, only operation
* Two kingds of valid attribute names, *data attribute*, *method*
	1. *data attribute*, data members. Need not be declared
		```python
		x = MyClass()

		x.counter = 1
		while x.counter < 10:
			x.counter = x.counter * 2
		print x.counter
		del x.counter
		```

	2. *method*, a class, all function objects define correponding methods of its instances. 
	   * `x.f` method reference, since `MyClass.f` is a function
	   * `x.i` is not, since `MyClass.i` is not

#### 3.4 Method Object ####
* `x.f`, method object, can be stored away and called later
	```python
	xf = x.f
	while True:
		print xf()
	```
* object passed as the first argument of the function, `x.f()` equivalent to `MyClass.f(x)`

1. an instance attribute is referenced that isn't a data attribute, its class is searched.
2. the name denotes a valid function object, a method object is created
3. a method object is created by pointers to the instance object and the function object found together in an abstract object


## 4. Remarks ##
* Data attribute override method attributes
	* Use some conventions to minimizes the chances of conflicts
		1. capitalizing method names
		2. prefixing data attribute names with a small unique string (underscore)
		3. using verbs of methods and nouns for data attributes

* No data hiding, data attributes can be referenced by anyone
* `self`, also a convention, no special meaning to Python. So function definition is not necessarily textually enclosed in class definition

	```python
	def f1(self, x, y):
		return min(x, x+y)

	class C:
		f = f1
		def g(self):
			return 'hello world'
		h = g
	```
	* `f`, `g`, `h` are all attributes of `C`

## 5. Inheritance ##
* `class DerivedClass(modename.BaseClass):` 
* Derived classs, may override methods of their base classes
	* extend the base class method, `BaseClass.methodname(self, arguments)`

* `isinstance()`, check an instance's type, `isinstance(obj, int)`
* `issubclass()`, check class inheritance, `issubclass(bool, int)`

#### 5.1 Multiple Inheritance ####
* `class DerivedClass(Base1, Base2, Base3):`

## 6. Private Variables and Class-local References ##
* Convention: a name prefixed with a underscore `_spam` should be treated as private

* *name mangling*, `__spam` (at least tow leading underscores, at most one trailing underscore), textually replaced with `_classname__spam`
	* helpful letting subclass override methods without breaking intraclass method calls

	```python
	class Mapping:
		def __init__(self, iterable):
			self.items_list = []
			self.__update(iterable)

		def update(self, iterable):
			for item in iterable:
				self.item_list.append(item)

		__update = update # private copy of original update() method

	class MappingSubclass(Mapping):

		def update(self, key, values):
			# provides new signature for update()
			# but does not break __init__()
			for item in zip(keys, values):
				self.items_list.append(item)
	```

## 7. Iterators ##
* `for` statement calls `iter()` on the container object
* `iter()` returns an iterator object; defines `next()`, accessing elements one at a time

	```python
	class Reverse:
		"""Iterator for looping over a seq backwards."""
		def __init__(self, data):
			self.data = data
			self.index = len(data)
		def __iter__(self):
			return self
		def next(self):
			if self.index == 0:
				raise StopIteration
			self.index = self.index - 1
			return self.data[self.index]

	rev = Reverse('spam')
	iter(rev)
	```

## 8. Generator ##
* simple powerful tool for creating iterators
* written like regular functions, but use the `yield` whenever return data
* each time `next()` called, the generator resumes where it lef-off

	```python
	def reverse(data):
		for index in range(len(data)-1, -1, -1):
			yield data[index]

	for char in reverse('golf'):
		print char
	```

## 9. Generator Expressions ##
* Simple generators, coded succinctly as expression using a syntax similar to list comprehensions but with parentheses
* more compact, less versatile

```python
sum(i*i for i in range(10))

xv = [10, 20, 30]
yv = [7, 5, 3]
sum(x*y for x,y in zip(xv, yv))

from math import pi, sin
sine_table = dict((x, sin(x*pi/180)) for x in range(0, 91))

unique_words = set(word for line in page for word in line.split())

valedictorian = max((student.gpa, student.name) for student in graduates)

data = 'golf'
list(data[i] for i in range(len(data)-1, -1, -1))
```

