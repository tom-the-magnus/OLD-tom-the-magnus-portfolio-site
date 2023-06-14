---
layout: post
title: "Python: Time Complexity, Space Complexity"
description: "Measuring space and time complexity in python"
date: 2019-01-03
feature_image: images/python-time-complexity
tags: [python]
---

## Overview

When we're designing algorithms, it's important to make sure that we're designing algorithms that are efficient. There are two main ways an algorithm can be efficient - with the time it takes to run and the space in memory it uses up when running. 

The time an algorithm takes to run is determined by 
its **time complexity**. If an algorithm took a very long time to run, we could describe the algorithm as being "very space complex" or "having high space complexity". You could probably just also say it's inefficient or takes a long time to process, though.

The same goes for space in memory. The space an algorithm takes to run is determined by its **space complexity**. We could describe algorithms and being "very space complex" or "having high space complexity" as well, although people may also just say an algorithm is "memory intensive" or "eats up RAM". 

So if we wanted to know how any given algorithm performs in these two criteria, how would we test for this? 

## Measuring Time Complexity
Of the two measures, time complexity is the one most programmers care about more. Also, it's easier to measure.

This measure partly depends on the hardware you're using, but if you're comparing between algorithms, the proportional differences between algorithms can still help you decide which algorithm is most efficient time-wise. 

#### Run Times
One part of time complexity is how fast an algorithm runs. To judge an algorithm's run tim, we can use a package called `timeit`. This just measures the time that our algorithm needs to run, in seconds. Here's an example comparing the time complexity of a comprehension to a for loop.

```python
from timeit import timeit

# Approach 1: List comprehension
list_comprehension_time = timeit('[i**2 for i in range(1000)]', number=100)

# Approach 2: For loop
for_loop_time = timeit('result = []\nfor i in range(1000):\n\tresult.append(i**2)', number=100)

print(f"List comprehension time: {list_comprehension_time}")
print(f"For loop time: {for_loop_time}")
```

In this example, the `number` represents the number of times that `timeit` will run the algorithm. So here, it'll run each 100 times. 

Out of those 100 times, we will select for the shortest time as our algorithm's true time. This might be unexpected - you probably thought that we would average every attempt. However, we actually want to take the fastest example because there's a certain amount of noise with time measurements due to other concurrent system processes taking up processing power. The fastest example is always the least noisy, which makes it the truest available representation of the algorithm's time to run.

But wait - if the run time is influenced by the hardware we run the algorithm on and there's noise during each run, how do we ever really know for absolute certain what algorithms are faster than others? 

That's what **Big O** is for.

#### Efficiency with Big O

Big O notation ( pronounced "big oh") provides a platform agnostic, noise-less method of comparing algorithm efficiency. It does this by not measuring runtime, but measuring how run time increases proportional to the size of the input. 

Assuming *n* is the size of your input, Big O would describe your algorithm's efficiency in terms of the difference between *n* and the number of steps the algorithm takes to find a solution.

Here are a few examples of different time complexities expressed in BigO. 

| Big O | Complexity | Explanation |
| ----- | ---------- | ----------- |
| O(1) | constant | When we talk about constant time or `O(1)`, we mean that the runtime doesn't change, no matter how big the input gets. This is like if `timeit` ran our algorithm in the exact same amount of time, every single time, regardless of the list size. An example of this would be looking up an item in a hash table. |
| O(n) | linear | Linear time, or `O(n)`, implies that as the size of our input grows, the runtime increases at a directly proportional rate. Think of `timeit` running an algorithm that checks every single item in a list. The more items, the longer it'll take. |
| O(n^2) | quadratic | For quadratic time, denoted as `O(n^2)`, imagine `timeit` running a sorting algorithm, where every item in a list needs to be compared to every other item. The runtime isn't just increasing—it's increasing at an increasing rate! |
| O(2^n) | exponential | Exponential time `O(2^n)` is even more intense. The runtime here grows exponentially as the size of the input increases. These kinds of algorithms can be extremely inefficient, and you'll see `timeit` taking longer and longer to run. An example of an exponential algorithm is the three-coloring problem. |
| O(log n) | logarithmic | Logarithmic time `O(log n)` is pretty cool. Even as the size of our input grows exponentially, the runtime increases linearly. So if `timeit` takes 1 second to process 1000 elements, it will take 2 seconds to process 10,000, 3 seconds for 100,000, and so on. A binary search is a perfect example of a logarithmic time algorithm. |

Big O notation helps us abstract away noise and hardware limitations to focus on the most important aspect: how the algorithm scales as the size of the input increases. It gives us a high-level view of the algorithm's efficiency by considering the number of operations an algorithm performs relative to the size of the input.

For instance, an algorithm with `O(n)` time complexity will perform *n* operations for an input of size *n*. Whether each operation takes a millisecond or a second doesn't matter when we're comparing it with an `O(n^2)` algorithm, because as *n* becomes very large, the quadratic algorithm will always take significantly more time than the linear one, regardless of hardware or noise.

So, while Big O notation doesn't directly deal with the time taken by an algorithm, it provides a measure of how the time to execute the algorithm grows as the size of the input increases, abstracting away the exact hardware and environment details. This allows us to make general statements about an algorithm's efficiency and how it will likely perform as the input size increases, regardless of the specific environment in which it's run.

Really, **the value of Big O notation lies in its ability to provide a hardware-agnostic, high-level understanding of an algorithm's time complexity.**

## Measuring Space Complexity

The other side of complexity is space complexity - how much memory an algorithm uses up when it's running. An algorithm can be super fast, but if it's eating up all your RAM, it can still lead to an inefficient and sluggish system.

Before we go any further, I think it's important to make once thing very clear - **When an algorithm is running, the parameters and outputs of that algorithm is many times stored in memory ( RAM ). So if you're running a for loop that uses a large dictionary to generate a list, both the dictionary and list are stored in memory while the loop runs.** 

So consider if we wanted to run a list comprehension on a list that is 20GB large. The resulting list will be 20GB itself and will be stored in memory (RAM) as the algorithm is running. 

The time complexity for comprehensions is generally O(n), which is pretty efficient. However, if this algorithm is taking up anywhere over 10GB of your RAM, your system is either going to be running very slowly or just not be able to run at all. So that's the important of space complexity - it tells us how much memory our system uses, which influences the efficiency of the algorithm. 

But how do we measure space complexity? In many ways, we measure it similarly to time complexity.

The space complexity of an algorithm depends on the amount of memory space that the algorithm needs to run to completion. The key parts of space complexity analysis include the following:

- **Fixed Space**: This is the space required for storing the constants and simple variables used by your function.
- **Variable Space**: This is the space used by variable data structures, such as arrays, lists, and dictionaries, that grow in size during the execution of the program. This can also include the space needed for recursion stack.
- **Dynamically Allocated Space**: This includes the space needed for objects and data structures that your code creates during runtime.

Let's look at a simple example to illustrate this concept:

```python
def create_list(n):
    result = []
    for i in range(n):
        result.append(i)
    return result
```

In this function, the size of the `result` list depends on the input `n`. As `n` increases, the amount of memory required to store `result` also increases. Hence, we say this function has a space complexity of `O(n)`.

On the other hand, if a function uses a fixed amount of space regardless of the input size, we say it has a constant space complexity, `O(1)`.

Here are a few examples of different space complexities expressed in BigO. 

| Big O | Complexity | Description |
| ----- | ---------- | ----------- |
| O(1) | constant | The memory usage is constant. It does not grow with the size of the input. Accessing a variable or an array element by index are examples of constant space complexity. |
| O(n) | linear | The memory usage grows linearly with the size of the input. For instance, if we are storing the elements of an input list in a new list, then the space complexity is O(n). |
| O(n^2) | quadratic | The memory usage is a quadratic function of the input size. This happens, for instance, when you store a two-dimensional square matrix of size `n` x `n`. |
| O(2^n) | exponential | The memory usage grows exponentially for a small increase in input size. An example is generating all subsets of a set, where the number of subsets doubles for each additional element. |
| O(log n) | logarithmic | The memory usage grows proportionally to the logarithm of the input size. This often happens in recursive algorithms that divide the problem into half at each level, like binary search. While such algorithms usually have a logarithmic time complexity, they don't typically have a logarithmic space complexity because of the memory used in the call stack. |

By assessing the space complexity of our algorithms, we can make sure they are not just quick, but also memory-efficient. This is especially important when we're dealing with big data or running our code on devices with limited memory.

## Wrap Up

Time complexity and space complexity are important concepts when measuring algorithm efficiency. We can use Big O notation to judge either. 

When you're building algorithms, consider that the most efficient algorithm isn't always the fastest one or the one that uses the least memory. It's usually the one that strikes the best balance between these two aspects, given the constraints you're working under.

Thanks for reading! 
