# Module #

* Module: a file containing Python definitions and statements
* File name is the module name, suffix `.py`
* Module's name available as the global variable `__name__`

	```python
	# suppose we created a module called fib, which has two function fib and fib2
	# fib prints Fibonacci series up to n
	# fib2 returns Fibonacci series up to n

	import fibo

	fibo.fib(1000)
	fibo.fib(100)
	fibo.__name__

	# assign it to a local name
	fib = fibo.fib
	fib(500)
	```

## 1. More on Modules ##
* Module, contains executable statements and function definitions
	* Statements:
		1. initialize the module
		2. executed only the first time, module import statement
		3. also run if the file is executed as a script

* Module, has its own private symbol table, used as the global symbol table by all functions defined in the module.
	1. Module author, can use global variables in the module, won't clash with the user's global variables.
	2. If a user want to use a module's global variable, `modname.itemname`

* Module can import other module
* Customary, place all `import` at the beginning of a module
* Imported module names placed in the importing module's global symbol table
* `from fibo import fib, fib2`, a variant of the `import` statment
	1. imports names from a module directly into the importing module's symbol table
	2. does not introduce the module name to the importer's symbol table

* `from fibo import *`, imports all names except those beginning with an underscore. (In practice, frown up)

* In interpreter session, if a module is changed, need to `reload(modulename)`, to see these changes

#### 1.1 Executing modules as scripts ####
* Run a Python module, `python fibo.py <arguments>`
	* by setting `__name__` to `"__main__"`

* If the module is imported, the code is not run

#### 1.2 Module Search Path ####
* Module `spam` imported
	1. first searches for a built-in module
	2. not found, searches for a file named `spam.py` in a list of directories given by `sys.path`, which is initialized from:
	   * current directory
	   * `PYTHONPATH`
	   * installation-dependent default

## 2.Standard Modules ##
* Provide access to operating system primitives
* `sys.ps1`, `sys.ps2`, `sys.path`

## 3. `dir()` function ##
* find out which names a module defines
* without arguments, lists the names the user have defined



