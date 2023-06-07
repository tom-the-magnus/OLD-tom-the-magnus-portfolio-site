---
layout: post
title: "Python - Comprehension Basics"
description: "An overview of Python comprehensions."
date: 2023-05-01
feature_image: images/python-lc.png
author: Tom
tags: [python]
---

Comprehensions are used to transform one list into another list using only a single line of code. So if you already have a list and want to produce a new list by performing some operation on every element of the existing list, you use a comprehension.

<!--more-->

There are list comprehensions and dictionary comprehensions. This document addresses both.

## List Comprehensions

The syntax is:

```
`new_list = [expression for item in iterable]`
```

So, all list comprehensions have these four elements:

- a new list
- an expression 
- a for statement 
- an iterable 

To see how this would be an implemented, here's an example:

```
# Our iterable, a list of numbers
numbers = [1, 2, 3, 4, 5] 

# A new list we are going to create with list comprehension
squares = [num**2 for num in numbers]
```

In this example, the elements of our comprehension are:

- a new list - `squares`
- an expression - `num**2`
- a for statement - `for num`
- a list to be iterated on - `numbers`

If we ran this code, `squares` would be equal to: 

```
[1,4,9,16,25]
```

If it's your first time seeing a comprehension, the syntax may be a bit confusing. To put it into plain English, think of:

```
squares = [num**2 for num in numbers]
```

as:

>"Squares is equal to each element to the power of two in the iterable called 'numbers'"

So the logic here is that each element in `numbers` gets squared and those numbers are added to `squares`. `1` in `numbers` stays `1`, `2` becomes `4`, and `3` becomes `9`.

## Comprehension Conditionals

You can include conditional statements on the end of a comprehension to exclude certain values from the final list. For example, say we wanted a list of squares, but only if they were even. We could write: 

```
# An existing list
numbers = [1, 2, 3, 4, 5] 

# A new list we are going to create with list comprehension
squares = [num**2 for num in numbers if num % 2 ==0] 
```

Since we've added that `if num % 2 == 0 ` at the end, we'll only get even outputs. So the final list produced would be:

```
[4,16]
```

## Dictionary Comprehensions

If you can wrap your head around list comprehensions, dictionary comprehensions are syntactically and logically very similar.

Actually thought, before we talk about dictionary comprehensions, let's discuss some built-in methods to transform dictionaries into a set of lists, as this is many times the more simple way to comprehend a dictionary.

First, you can use these built-in methods to generate a list of the values or keys in a dictionary.

```
dict1 = {'a': 1, 'b': 2, 'c': 3, 'd': 4}
key_list = dict1.keys()
val_list = dict1.val()
```

Pretty simple - it just takes all the keys or values and puts them into a list, so they'd look like:

```
key_list = [a,b,c,d]
val_list = [1,2,3,4]
```

If we wanted a list of the key value pairs, each pair being an element in the list, we would use:

```
dict_items = dict1.items()
```

Which would look like:

```
dict_items = ([('c', 3), ('d', 4), ('a', 1), ('b', 2)])
```

What does this mean? **It means that very often you can work around making a dictionary comprehension by converting the dictionary values to a list.**

That said, there are going to be times when that is not possible and you will have to write a comprehension for the dictionary itself. The syntax to overall pretty similar:

```
dict_variable = {key:value for (key,value) in dictonary.items()}
```

and you can add a condition by doing something like:

```
dict_list = {key:value for (key,value) in dict_items if value < 5000}
```


## Alternative Methods

But what if you cannot use a comprehension for some reason? There are other ways to produce a new list based on some iterable. For example, instead of using a comprehension, we could just use a for loop like this:

```
numbers = [1,2,3,4,5]  
squares =[]  
  
for num in numbers:  
	if num % 2 == 0:  
		squares.append(num**2)  
```

Or you could even use a combination of map, filter, and lambda to achieve this output.

```
numbers = [1, 2, 3, 4, 5] 
squares = list(map(lambda num: num**2, numbers)) 
even_numbers = list(filter(lambda num: num % 2 == 0, numbers)) 
```

If you're unfamiliar with `map`, `filter`, or `lambda`, you can read about these in my [[map(),  filter(), lambda]] article.

So, there's a bunch of different ways we could do this same task. What are the unique advantages of comprehension, then? 

Really, comprehensions are many times just the least line intensive and most legible method to perform an operation on an iterable. If you can use a comprehension, you probably should.

Well done, that's it! If you want to check out some related tutorials, I've linked them below. 

- [[map(), filter(), lambda]]

