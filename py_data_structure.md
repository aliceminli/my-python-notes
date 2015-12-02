# Data Structures #

## 1. Lists ##
1. `list.append(x)`, `list[len(list):] = [x]`
2. `list.extend(L)`, `list[len(list):] = L`
3. `list.insert(i, x)`
4. `list.remove(x)`
5. `list.pop(i)`, `i` is optional, if `i` is not specified, removes last one
6. `list.index(x)`
7. `list.count(x)`
8. `list.sort()`
9. `list.reverse()`

#### 1.1 Lists as Stacks ####
* Add an item, use `append()`
* Retrieve an item, use `pop()`

#### 1.2 Lists as Queues ####
* Use `collections.deque`

	```python
	from collections import deque
	queue = deque(["Eric", "John", "Michael"])
	queue.append("Terry")
	queue.append("Graham")
	queue.popleft()
	```

#### 1.3 Functional Programming Tools ####
* `filter(function, sequence)`

	* returns a sequence whose items from the sequence for which `function(item)` is true
	* *sequence* can be a string or tuple, otherwise list

* `map(function, seq)`

	* calls `function(item)` for each of the `seq` items
	* returns a list of the return values
	* more than one sequence may be passed; the function must then have as many arguments as there are sequences

* `reduce(function, sequence)`

	* returns a single value
	* constructed by calling binary function on the first two items of seq
	* then on the result  and next item, and so on. 

	```python
	# filter
	filter(lambda x: x % 2 != 0 and x % 3 != 0, range(2, 25))

	# map
	map(lambda x: x*x*x, range(1, 11))

	# map with two sequences
	seq = range(8)
	def add(x, y): return x+y

	map(add, seq, seq)

	# reduce
	reduce(add, range(1, 11))
	```

#### 1.4 List Comprehensions ####

* Make new lists

	* each element results from some operations applied to each member of another sequence
	* create a subsequence of those elements meet a certain condition
	* consists of brackets containing an expression followed by `for`, then zero or more `for` or `if` clauses

	```python
	squares = [x**2 for x in range(10)]

	# listcomp combines the elements of two lists if they are not equal
	[(x, y) for x in [1,2,3] for y in [3,1,4] if x != y]

	# filter the list
	vec = [-4, -2, 0, 2, 4]
	[x for x in vec if x >= 0]

	# flatten  a listing using listcomp
	vec = [[1,2,3], [4,5,6], [7,8,9]]
	[num for elem in vec for num in elem]

	# complex expressions and nested functions
	from math import pi
	[str(round(pi, i)) for i in range(1, 6)]
	```

###### 1.4.1 Nested List Comprehensions ######
* The initial expression in a listcomp can be any arbitrary expression, including another listcomp

	```python
	matrix = [
		[1, 2, 3, 4],
		[5, 6, 7, 8],
		[9, 10, 11, 12]
		]

	# the transpose
	[[row[i] for row in matrix] for i in range(4)]

	transposed = []
	for i in range(4):
		transposed_row = []
		for row in matrix:
			transposed_row.append(row[i])
		transposed.append(transposed_row)
	```

## 2. `del` statement ##
* Remove an item from a list given its index instead of its value

	```python
	a = [-1, 1, 66.25, 333, 333, 1234.5]
	del a[0]
	del a[2:4]
	del a[:]
	```
## 3. Tuples and Sequences ##
* Consists of a number of values separated by commas
* Can be defined with or without surrounding parentheses
* Tuples are *immutable*, so `t[0] = 8888` will throw an error

	```python
	t = 12345, 54321, 'hello!'
	t[0]

	# Nested tuples
	u = t, (1, 2, 3, 4, 5)

	# empty tuple
	empty = ()

	# singleton tuple
	singleton = 'hello',  # trailing comma is necessary
	```
* *Sequence unpacking*
	* reverse operation to sequence packing (`t = 12345, 2, 3`), `x, y, z = t`
	* number of variables on the left = length of tuple 
	

## 4. Sets ##
* Creation, `set()`
* Set comprehensions also supported

	```python
	basket = ['apple', 'orange', 'apple', 'pear', 'orange', 'banana']
	fruit = set(basket)

	# set comprehension
	a = {x for x in 'abracdfad' if x not in 'abc'}
	```

## 5. Dictionaries ##
* Indexed by *keys*, which can be any immutable type (string, number, or tuples containing no mutable type)
* Creation, `{}`, placing a comma-separated list of key:value pairs
* `del` delete a key:value pair
* If a key is already in use, the old value will be overwritten
* Error to extract a value using non-existent key
* `keys()` returns a list of all the keys in dict, in arbitrary order
* `in` check whether a single key is in dict

	```python
	tel = {'jack': 4098, 'sape': 4139}

	# add a key:value pair
	tel['guido'] = 4127

	# retrieve a value by key
	tel['jack']

	# delete a KV pair
	del tel['sap']

	tel.keys()
	'guido' in tel
	```

* `dict()` constructor, construct dictionaries from sequences of key-value pairs
* Keys are simple strings, specify pairs using keywords arugments
	```python
	dict([('sape', 4139), ('guido', 4127), ('jack', 4098)])

	dict(sape=4139, guido=4127, jack=4098)
	```

* Dict comprehensions

	```python
	{x: x**2 for x in (2, 4, 6)}
	```

## 6. Looping Techniques ##
* `enumerate()`, position index and corresponding value can be retrieved at the same time

	```python
	for i, v in enumerate(['tic', 'tac', 'toe']):
		print i, v
	```

* `zip()`, loop over two or more seqs at the same time, pairing entries

	```python
	questions = ['name', 'quest', 'favorite color']
	answers = ['lancelot', 'the holy grail', 'blue']
	for q, a in zip(questions, answer):
		print 'What is your {0}? It is {1}.'.format(q, a)
	```

* `reversed()`, loop over a sequence in reserse

	```python
	for i in reversed(xrange(1,10,2)):
		print i
	```

* `sorted()`, returns a new sorted list, source unaltered

	```python
	basket = ['apple', 'orange', 'apple', 'pear', 'orange', 'banana']
	for f in sorted(set(basket)):
		print f
	```
* `iteritems()`, looping through dictionaries, retrieve key and corresponding value at the same time

	```python
	knights = {'gallahad': 'the pure', 'robin': 'the brave'}
	for k, v in knights.iteritems():
		print k, v
	```
* slice notation `words[:]` implicitly make a copying

	```python
	words = ['cat', 'window', 'defenestrate']
	for w in words[:]: # Loop over a slice copy of the entire list.
		if len(w) > 6:
			words.insert(0, w)
	```

## 7. More on Conditions ##
* `in`, `not in` checks whether a value (not) occurs in a sequence
* `is`, `is not` compare whether two objs are the same
* Chained comparison, `a < b == c`, whether `a` is less than `b` and moreover `b` equals `c`
* Comparisons, combined by `and`, `or`; Negated with `not`



