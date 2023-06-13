---
layout: post
title: "Python: map(), filter(), lambda()"
description: "A basic guide to these built in functions in Python"
date: 2023-05-05
feature_image: images/python-map.png
tags: [python]
---

## Overview

`map()`, `filter()`, and `lambda` are three powerful built-in Python functions that can allow you to construct a new iterable ( a list, dictionary, set, etc ) based on an existing iterable. 

<!--more-->

Really, they accomplish the same general goal as comprehensions, but there are syntactic and logical differences. Let's find out what they are! 

Let's start with `map()`.

## map()

`map()` is really just a function for constructing an iterable based on an existing iterable, but there are some parts of its syntax that may be new to you. 

The trickiest thing about the syntax for map is that you typically define a method outside the `map()` method and then call it using `map()`. Here's a simple example:

```
def multiply_2(n):
	return n * 2

numbers = [1, 2, 3, 4, 5] 
times_two = map(multiply_2, numbers)) 
```

In this case, `map()` would take the `numbers` list, process each element according to `multiply_2`, and output this:

```
[2,4,6,8,10]
```

To break down the `map()` function, the syntax is really:

```
new_list = map(other_method, iterable)
```

So, all `map()` methods have these elements:

- `new_list` - the name of the new list we will create using `map()`
- `map()` - the built in `map()` method itself
- `other_method` - the pre-defined method outside `map()` we are going to use to process each element inside our `iterable`
- `iterable` - an iterable that the `map()` method will process using `other method`

The map function then returns an object that is another iterable sometimes called a **map object**. 

Sometimes though, it can be more concise to have the `other_method` be on the same line as the `map()` function itself. This is what `lambda()` is for. 

## lambda()

`lambda()` allows you to define what's called an "anonymous function", which is really just a function that has no `def` . Really, `lambbda()` behaves just like `def`  does when we're typically writing methods, but we don't have to give the `lambda()` function a name and whatever `lambda()` function we write cannot be called outside of where we write it.

If it's your first time learning about these functions, that may sound a bit strange. This example may make more sense of this:

```
numbers = [1, 2, 3, 4, 5] 
squares = map(lambda num: num**2, numbers)
```

So let's break this down according to the elements we talked about before. Recall that the parts of `map()` are:

- `map_method` - The name of the new method we will create with `map()`
- `map()` - the built in `map()` method itself
- `other_method` - the other method we are going to use to process each element
- `iterable` - an iterable that the `map()` method will process

In this method, those are:

- `map_method` - This is `squares`
- `map()` - This is `map()`
- `other_method` - This is `lambda num: num**2`. This takes every element (`num`) in the list and squares it. 
- `iterable` - This is `numbers`

So as you can see, combining `map()` and `lambda()` is a really concise way to construct a new iterable based on an existing iterable. The syntax can be a bit tricky at first, so make sure to take some time to review this.

However, sometimes you'll want to make a new iterable that excludes certain values. For that, you can use `filter()` instead of `map()`. 

## filter()

`filter()` is syntactically almost exactly the same as `map()`, it's just that it's used to exclude values from the list you create. Here's an example.

```
numbers = [1, 2, 3, 4, 5] 
even_numbers = list(filter(lambda num: num % 2 == 0, numbers))
```

The output of that would be:

```
[2, 4]
```

Also, just so long as we're discussing this, let's ask - What if we just swapped `filter()` for `map()` here, like the below:

```
numbers = [1, 2, 3, 4, 5] 
more_even_numbers = list(map(lambda num: num % 2 == 0, numbers))
```

The output would actually be in binary as:

```
[False, True, False, True, False]
```

So, as you can see see, `filter()` is really similar to `map()` except that `filter()` is used when we want to exclude certain values from our new iterable.

So really, `map()` is used if you want to construct a new iterable and keep every element. You use `filter()` if you want to exclude certain elements.

## Limits to map() and filter()

Overall, `map()` and `filter()` are good to use if you need to construct a new iterable using a method that is not logically very complicated. 

However, there are cases where a good ol' `for` loop may be a better choice, particularly if you have a very complex series of steps that a iterable must be processed with.

Really, my final guidance would be this - **`map()` and `filter()` are probably the most compact and legible way to process some iterable If you need to construct a new iterable with a single method and/or condition, use them.**

That's all! Well done and hope you enjoyed reading!
