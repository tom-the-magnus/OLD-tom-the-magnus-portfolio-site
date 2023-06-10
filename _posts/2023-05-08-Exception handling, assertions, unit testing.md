---
layout: post
title: "Debugging 101: Exception handling, assertions, unit testing"
description: "Learn how to detect errors and squash bugs in Python"
date: 2023-05-08
feature_image: images/python-bugs.png
tags: [pythong, debugging]
---

When you're writing a program in Python, it's good to test each part of your program as you build it so that you can detect **errors** and **limitations** with your code early on.

To do this, programmers write **unit tests**. These allow you to spot errors and limitations in your code, understand their root causes, and fix up your code as your building it. 

<!--more-->

## Errors in Python

There are two main kinds of `errors` in Python. 

One is a `syntax error`. This means you included or forgot to include a **token**(a letter, number, or symbol) in a phrase that ought not have been there. This makes it so that Python compiler could not parse your program.

If you make a `syntax error` , your code will fail to compile, so there's really no way to avoid recognizing these. In the console, they may look something like:

```
SyntaxError: invalid syntax
SyntaxError: EOL while scanning string literal
```

The other type of error is an `exception`. An exception is basically a logical error in your code - you tried to divide by zero, concatenate a string with an integer, or call a variable that was not defined. These are things which make your program logically impossible, so Python cannot compile it.

Some `exceptions` are pre-written in Python and go by specific names. For example, if you try to divide by zero, Python will not compile your code because of a `ZeroDivisionError`. If you try to add a string and a number together, your code will fail to compile because of a  `TypeError`. They may look something like:

```
ZeroDivisionError: division by zero
TypeError: cannot concatenate 'str' and 'int ' objects
ValueError: substring not found
```

These are built into Python and their logic is immutable. However, the exceptions can have custom responses, which is exactly what programmers do when unit testing. 

## Unit Testing: Try and Except

So how exactly would someone write a custom response to a built-in exception? Observe the below:

```
try:
	# This is called the "try block" and has some operation in it

except ExceptionName:
	# We would specify an exception above, like an IOError or TypeError, that       would create some output. 
	
except ExceptName2:
	# And we cam actually have multiple exceptions for a single try block           which is neato.
```

To give a very simple application of this, observe this as well:

```
try: 
	# This is the 'try block' 
	x = 1 / 0 # This will raise a ZeroDivisionError 
	
except ZeroDivisionError: # We are specifying a ZeroDivisionError
	# This is the 'except block' 
	print("You can't divide by zero!")
```

So there are some common elements to all try-except clauses, those being:

- `try` - An operation your program is going to attempt to run
- `except` - An exception that your program will have a custom response to, whether that be a print statement or something else

With those common elements in mind, let's try and read the below:

```
try:
	fh = open("testfile", "r")
	fh.write("This is my test file for exception handling!!")
except IOError:
	print "Error: can\'t find file or read data'"
else:
	print "Written in the file successfully"
```

So in this example, we are trying to write to a file called `fh`. If we cannot write to it because the file does not exist, so an `IOError` will be raised. If we can write to the file, we'll make it to the `else` block and get a print statement confirming that the file has been written to successfully.

There's also something called `finally` for when we always want something to be executed, which looks like this:

```
try:
	# An operation goes here, which may be skipped because of some exception
except Blah:
	# That's not a real error code or anything, but this is just an example
	  though, okay?
finally:
	# This would be executed always, no matter what happens
```

For example, we could add this to our code above to make sure that `fh` is closed every time.

```
try:
	fh = open("testfile", "r")
	fh.write("This is my test file for exception handling!!")
except IOError:
	print "Error: can\'t find file or read data'"
else:
	print "Written in the file successfully"
finally: 
	fh.close() 
	print("File closed successfully")
```

## Forcing and chaining

You can also force exceptions to be **raised** so long as they have been previously defined, whether they are built-in or user-made. You can even do this outside of a try-except clause. For instance:

This can allow you to use errors to catch bad user input or unusual behavior with your program. For instance:

```
def sqrt(x): 
	if x < 0: 
		raise ValueError("Input should be a positive number") 
	return x ** 0.5```
```

So we could use this to catch bad use input. Or here's another example:

```
class AbstractClass: 
	def do_something(self): 
		raise NotImplementedError("Subclass must implement this method")
```

In this example the error is use to make sure a a method is implemented, but really we could just to make sure that a programmer is aware of how our classes and methods ought to be used and is prevented from using them improperly.

You can also **chain** exceptions, which can help with traceback:

```
def do_something():
    try:
        open("/path/to/some/file")
    except FileNotFoundError as e:
        raise RuntimeError("Failed to open the file") from e

try:
    do_something()
except RuntimeError as e:
    print(e)
    print(e.__cause__)
```

In this example, if the file is not found, a `FileNotFoundError` is raised. A new `RuntimeError` is then raised with the original exception as the cause. If you were to run this code and the file did not exist, you will see an output that shows both the new and original exceptions. The `e.__cause__` will give you the original exception details.

You could chain exceptions for various reasons, some of them being abstraction and identifying unexpected errors. For now, it's enough to be aware that this does exist. 

## Unit Testing

Exceptions are used in a process called `Unit Testing`, which is a methodology for testing out small pieces of a script as you build to make sure they are functional. A bit confusingly, while `Unit Testing` is a collection of practices in Computer Science, `UnitTest` is actually a module you have to import, like this:

```
import unittest

def div(a,b):
	return a/b
class raiseTest(unittest.TestCase):
	def testraise(self):
		self.assertRaises(ZeroDivisionError, div, 1,0)

if __name__ == '__main__':
	uniittest.main()
```

Let's break this down line by line. 

```
import unittest
```

This imports the test module.

```
def div(a,b):
	return a/b
```

This defines a method for dividing `a` over `b`.

```
class raiseTest(unittest.TestCase):
	def testraise(self):
		self.assertRaises(ZeroDivisionError, div, 1,0)
```

This defines an object with a method of `unittest.TestCase` as a parameter. 

The object itself contains a method that runs `assertRaises`, for which there is a specific syntax:

```
assertRaises(exception, callable, *args, **kwds)
```

The last two parameters in this are a bit tricky, so let's take a close look. The `args` in this case are just `div` - the method we are running through the unit test.  The `kwds` (keywords) are the parameters for the method we are defining as the `arg` (`a`,`b`) .

So, just to be clear - `div` is the method we are running through `assertRaises`, `kwds` are the parameters of that method.

In this case, since we are trying to divide 1 by 0, the error would be raised. If we changed the script to:

```
import unittest

def div(a,b):
	return a/b
class raiseTest(unittest.TestCase):
	def testraise(self):
		self.assertRaises(ZeroDivisionError, div, 1,1)

if __name__ == '__main__':
	uniittest.main()
```

Then no exception would be raised. 

## Unit Testing: assertEquals

Another commonly used method in the `unittest` module is `assertEquals`. This method tests that two values are equal. If they are not, the test fails. Here's how you could use it:

```
import unittest  
def add(a, b):     
	return a + b  

class AddTest(unittest.TestCase):     
	def test_add(self):         
		result = add(10, 5)         
		self.assertEqual(result, 15)  

if __name__ == '__main__':     
	unittest.main()`
```

In this code snippet, we define a function `add()` which adds two numbers. We then create a test case `AddTest` which is a subclass of `unittest.TestCase`. Inside `AddTest`, we define a method `test_add` that uses `self.assertEqual` to verify that `add(10, 5)` is equal to `15`. If it is, the test passes. If not, the test fails.

## Testing for Exceptions with assertRaises

As we discussed before, `assertRaises` is a useful method for testing whether a piece of code raises a certain exception when it's executed. However, it's also possible to use it as a context manager.

Here is an example:

pythonCopy code

```
import unittest  

def div(a,b):     
	return a/b  
	
class DivTest(unittest.TestCase):     
	def test_division_by_zero(self):         
		with self.assertRaises(ZeroDivisionError):             
			div(1, 0)  

if __name__ == '__main__':     
	unittest.main()`
```
In this example, the `with` statement creates a context where `self.assertRaises(ZeroDivisionError)` acts as a context manager. 

If the code inside the `with` block (in this case `div(1, 0)`) raises a `ZeroDivisionError`, the test passes. If the code does not raise a `ZeroDivisionError`, or if it raises a different kind of exception, the test fails.

## Wrapping Up

So far, we've seen how to use the `unittest` module in Python to write tests for our code. We've seen how to test for equality with `assertEquals`, and how to test for exceptions with `assertRaises`. There are many other methods available in the `unittest` module for more complex tests, but with just these two methods, you can already write a wide variety of useful tests for your Python code.

Remember, writing tests for your code can help you catch errors and unexpected behavior early, before your code ends up in a production environment. Even if it takes some time to write the tests, it's usually worth it for the time you save debugging and fixing problems later on.

Now, you can start writing tests for your Python code using the `unittest` module. Happy testing! If you want to check out some related tutorials, I've linked them below. 

- *Right now there are actually no related articles, but I'm working on it! *
