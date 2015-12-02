# Input and Output #

## 1. Output Formatting ##
* `str.format()`: control formatting of output
* `str()`: return representations of values, which are human-readable
* `repr()`: genertate representations, readable by the interpreter
* Many values have the same representation using either function. However, strings and float point numbers have distinct representations

	```python
	s = 'Hello, world.'
	str(s)
	repr(s)

	str(1.0/7.0)
	repr(1.0/7.0)

	# repr() of a string adds string quotes and backslashes
	hellos = repr('hello, world\n')
	print hellos
	```

* `str.rjust()`: right justifies a string, in a field of given width, by padding it with spaces on the left.
	* Similar methods: `str.ljust()`, `str.center()`
	* If the input is too long, they don't truncate, return unchanged
	
	```python
	# creating a table of squares and cubes using rjust
	for x in range(1, 11):
		print repr(x).rjust(2), repr(x*x).rjust(3),
		print repr(x*x*x).rjust(4)

	```

* `str.zfill()`: pads a numeric string on the left with zeros

	```python
	'12'.zfill(5)
	'-3.14'.zfill(7)
	'3.1415926'.zfill(5)
	```

* `str.format()` usage: `print 'We are the {} who say "{}!"'.format('knights', 'Ni')`
	* *format field*, brackets and characters within them, replaced with objects passed into `str.format()`
		1. positional and keyword arguments are supported
		2. `!s` apply `str()`, `!r` apply `repr()`, convert the value before formatted
		3. `:` and format specifier (optional), can follow field name
			* `[]` reference the variables by name instead of by position
			* also could be done by passing table as keyword args `**`
	
	```python
	# positional arguments
	print '{0} and {1}'.format('spam', 'eggs'}
	print '{1} and {0}'.format('spam', 'eggs'}

	# keyword arguments
	print 'This {food} is {adjective}.'.format(food='spam', adjective='absolutely horrible')

	# positional args and keyword args can be combined
	print 'The story of {0}, {1}, and {other}.'.format('Bill', 'Manfred', other='Georg')

	# creating a table of squares and cubes using format()
	for x in range(1, 11):
		print '{0:2d} {1:3d} {2:4d}'.format(x, x*x, x*x*x)

	import math
	print 'The value of PI is approximately {}.'.format(math.pi)
	print 'The value of PI is approximately {!r}.'.format(math.pi)


	table = {'Sjoerd': 4127, 'Jack': 4098, 'Dcab': 7678}
	for name, phone in table.items():
		print '{0:10} ==> {1:10d}'.format(name, phone)

	# reference by name 
	print ('Jack: {0[Jack]:d}; Sjoerd: {0[Sjoerd]:d}; Dcab: {0[Dcab]:d}'.format(table))

	# pass dict as keyword args
	print 'Jack: {Jack:d}; Sjoerd: {Sjoerd:d}; Dcab: {Dcab:d}'.format(**table)
	```

## 2. File IO ##
* `open(filename, mode)` returns a file object
	1. *mode* `'r'`, read only 
	1. *mode* `'w'`, write, erase existing one 
	1. *mode* `'a'`, append 
	1. *mode* `'r+'`, read and write 

#### 2.1 File Objects Methods ####
* `f.read(size)`, reads some quantity of data and returns it as a string.
* `f.readline()`, reads a single line from the file
* `list(f)`, `f.readlines()`, read all the lines in a file

```python
# read lines from a file
for line in f:
	print line,
```

* `f.write('This is a test\n')`, writes to the file
* Write something other than a string, convert to a string first

	```python
	value = ('the answer', 42)
	s = str(value)
	f.write(s)
	```

* `f.tell()`, returns an integer giving the file object's current position
* `f.seek(offset, from_what)`, position computed from adding *offset* to a reference point
	* *from_what*, *0*: from beginning; *1*: from current position; *2*: end of the file
* `f.close()`, close the file and free up system resource

* **Good Practice**, use `with` keyword to deal with file objects
	* file is properly closed after its suite finishes
	* shorter than writing `try-finally`
	
	```python
	with open('workfile', 'r') as f:
		read_data = f.read()

	f.closed
	```

#### 2.2 JSON ####
* *Serialize*, take Python data hierarchies, convert them to string representations.
* *Deserialize*, reconstruct data from string representation

```python
json.dumps([1, 'simple', 'list])

# f is a file object, x is an object
json.dump(x, f)

# decode the object
obj = json.load(f)
```

