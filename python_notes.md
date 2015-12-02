# Python Notes #

# Ch.1 Basics #

## 1. Executable Script ##

* Put the following line at the beginning of the script to give the file an executable mode.

	```python
	#! /usr/bin/env python
	```

## 2. Source code encoding ##

```python
# _*_ coding: utf-8 _*_
```

## 3. Comment ##
* Start with `#`

# Ch.2 Introduction #

## 1. Variables ##
* Variables must be defined before use
* Can assign several variables in one line
* Support Complex Numbers

	```python
	width = 20
	x = y = z = 0

	# Complex Number
	1j
	1J
	a = complex(0, 1)
	a.real
	a.img
	abs(a)
	```

## 2. String ##

#### Quoted in `'` or `"` ####

```python
'a string'
"a string"
hello = "hello\n\
world!"

print hello
```
* Surround in a pair of matching triple-quotes: `"""` or `'''`, `\n` do not need to be escaped

```python
print """
Hello
World!
"""
```
* "Raw" string, `\n` sequences are not converted to newlines. `\` and `\n` both included in the string as data.

```python
hello = r"Hello\n\
World!"

print hello
```

#### Concatenation `+` ####

```python
word1 = 'word1'
word2 = 'word2'
word1 + word2
```

#### Repetition `*` ####

```python
word = 'help'
word*5
```

#### Index ####

```python
word = 'hello'
word[4]
word[0:2] # 'he'
word[2:4] # 'll'
word[:2]  # 'he'
word[2:]  # 'llo'

word[-1]  # last one char
word[-2]  # last-but-one char
word[-2:] # last two chars
word[:-2] # everything except last two chars
```

* Python string cannot be changed. `word[0] = 'x'` will throw out an ERROR!
* Create a new one instead of change the content `'x' + word[1:]`

#### Length ####

```python
s = 'abcd'
len(s)
```

## 3. List ##

#### Surrounded by `[]`, elements separated by `,` ####

```python
a = ['spam', 'eggs', 100, 1234]
```
* Index, see String
* Elments is mutable

```python
a[2] = a[2] + 10
```
* Operations
  1. Replace `a[0:2] = [1, 12]`
  2. Remove `a[0:2] = []`
  3. Insert `a[1:1] = ['bletch', 'xyzzy']`
  4. clear `a[:] = []`
* `len()` function also applies 

# Ch.3 Control Flows #

## 1. `if`, `elif`, `else` ##

```python
x = int(raw_input("Please enter an integer: "))
if x < 0:
	x = 0
	print 'Negative changed to zero'
elif x == 0:
	print 'zero'
else:
	# do something else
```

## 2. `for` ##

```python
words = ['cat', 'window', 'defenestrate']
for w in words:
	print w, len(w)
```

* Iterate over a slice of a list

	```python
	for w in words[0:1]:
		print w, len(w)
	```

## 3. `range()` function ##
* Generates lists containing arithmetic progressions

	```python
	range(10) # [0, 1, 2, ..., 9]
	range(5, 10)  # [5, 6, ..., 9]
	range(0, 10, 3) # [0, 3, 6, 9]
	range(-10, -100, -30) # [-10, -40, -70]
	```

* Iterate over indices of a list

	```python
	a = ['Mary', 'had', 'a', 'little', 'cat']
	for i in range(len(a))
		print i, a[i]
	```


## 4. `break`, `continue`, and `else` clause ##

* `break`, `continue` the same as in C or Java
* Loop may have an `else` clause, and it is executed when the loop terminates through exhaustion of the list.
  
	```python
	for n in range(2, 10):
		for x in range(2, n):
			if n % x == 0:
				print n, 'equals', x, '*', n/x
				break
		else:
			# loop fell through without finding a factor
			print n, 'is a prime'
	```

## 5. `pass` ##
* Does nothing
* Commonly used for creating minimal classes
* Place-holder for a function or a conditional body, which allows user keep thinking at a more abstract level.

## 6. Define Functions ##

* `def` followed by function name, and parenthesized list of formal parameters. 
* *docstring*, optional, a string literal locate at the very beginning of the function body. 
* Function name, the value can be assigned to another name, which can also be used as a function.
* `return` without an expression, or falling off the end of a function, returns **None**

	```python
	# Define fibonacci series
	def fib(n):
		"""Print a Fibonacci series up to n."""  # docstring
		a, b = 0, 1
		while a < n:
			print a,
			a, b = b, a+b

	# Assign a function name value
	f = fib
	f(100)
	```

#### 6.1 Default Argument Values ####

```python
def ask_ok(prompt, retries=4, complaint='Yes or no, please!'): # reties = 4, complaint=... are default argument values
	while True:
		ok = raw_input(prompt)
		if ok in ('y', 'ye', 'yes'): in keyword, test whether a sequence contains a certian value
			return True
		if ok in ('n', 'no', 'nop', 'nope'):
			return False
		retries = retries - 1
		if retries < 0:
			raise IOError('refusenik user')
		print complaint
```
* *Default values* are evaluated at the point of function definition
* *Default values* only evaluated once
* If the default is a mutable object (list, dictionary ...), the function may behave differently

	```python
	def f(a, L=[]):
		L.append(a)
		return L

	# Note: the list L accumulates
	print f(1)
	print f(2)
	print f(3)
	```
* In the above example, the `L` accumulates (see the reasons above). If you don't want the default to be shared, use the following example Insted:

```python
def f(a, L=None):
	if L is None:
		L = []
	L.append(a)
	return L
```

#### 6.2 Keyword Arguments ####
* *keyword arguments*, `kwarg=value`
* *keyword arguments* must follow *positional arguments*
* *keyword arguments* passed must match one of the arguments accepted by the function

	```python
	def parrot(voltage, state='a stiff', action='voom', type='Norwegian Blue'):
		print "-- This parrot wouldn't", action,
		print "if you put", voltage,
		print "-- Lovely plumage, the", type
		print "-- It's", state

	# Accepted function calls
	parrot(100)
	parrot(voltage=1000)
	parrot(voltage=10000, action='vooom')
	parrot('a million', 'bereft of life', 'jump')
	parrot('a thousand', state='pushing up the dasisies')

	# INVALID FUNCTION CALLS
	parrot()                    # required argument missing
	parrot(voltage=5.0, 'dead') # non-keyword argument after a keyword argument
	parrot(110, voltage=220)    # duplicate value for the same argument
	parrot(actor='John Cleese') # unknown keyword argument
	```

#### 6.3 Dictionary Argument `**arg`, Tuple Argument `*arg` ####
* Dictionary Argument `**arg`
* Tuple Argument `*arg`

	```python
	def cheeseshop(kind, *arguments, **keywords):
		print "-- Do you have any", kind, "?"
		print "-- I'm sorry, we're all out of", kind
		for arg in arguments:
			print arg
		print "-" * 40
		keys = sorted(keywords.keys())
		for kw in keys:
			print kw, ":", keywords[kw]

	cheeseshop("Limburger",
		"It's very runny, sir."                # corresponds to *arguments
		"It's really very, VERY runny, sir.",  # corresponds to *arguments
		shopkeeper='Michael Palin',            # **keywords, dict
		client='John Cleese',                  # **keywords, dict
		sketch="Cheese Shop Sketch")           # **keywords, dict
	```

* Arbitrary Argument Lists, wrapped up in tuple.

#### 6.4 Unpacking Argument Lists ####
* Function call with `*` operator to unpack the arguments in a list or tuple

	```python
	args = [3, 6]
	range(*args)
	```
* Dictionaries can deliver keyword arguments with `**`

	```python
	def parrot(voltage, state='a stiff', action='voom'):
		# print whatever

	d = {"voltage": "four million", "state": "bleedin", "action": "VOOM"}
	parrot(**d)
	```

#### 6.5 Lambda Expression ####
* Small anonymous functions can be created with `lambda` keyword
* Creates a function object
* Syntactically restricted to a single expression

	```python
	def make_incrementor(n):
		return lambda x: x + n

	f = make_incrementor(42)
	f(0)
	f(1)
	```
* Pass small function as an argument

	```python
	pairs = [(1, 'one'), (2, 'two'), (3, 'three'), (4, 'four')]
	pairs.sort(key=lambda pair: pair[1])
	pairs
	```

