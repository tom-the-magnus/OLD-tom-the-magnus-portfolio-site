---
layout: post
title: "Python OOP: Inheritance"
description: "Article on inheritance"
date: 2023-06-09
feature_image: images/python-inheritance.png
tags: [python, python OOP]
---
This article will cover the function of Python inheritance, composition, and polymorphism.

This is a great topic because it's where we start to not only discuss what is *logically necessary* for a program to run, but we also discuss many best practices that will make your code more legible and resilient. 

<!--more-->

With inheritance, there are many times where you have the choice to implement a best practice or not. This implementation is totally up to you - it's not like the code will fail to compile without them. However, there are many best practices with inheritance that ought to be adhered to if you'd like to avoid frustrating, complicated situations when making changes to your code. 

So let's begin! 

## Basics of Inheritance

Let's say that you need to build an application that has cars as objects. Let's say that it's an application to manage the inventory for a car dealership. 

Since there are different cars in a dealership, we want there to be different kinds of objects to represent different kind of cars. 

We want these different cars to have different properties. If a car is a classic sports car, we'll want information on the year it was produced and its condition. If a car is a hybrid, we'll want information on its miles per gallon when its in eco mode. 

When we build this application, we could build it so that the objects that represent different classes of cars (sports car, hybrids, etc) are completely isolated and not logically related in any way. We could just make completely separate objects for each class of car that are not related in any way. 

That would be wasteful though, because really each one of these objects is going to share a lot of the same features. Also, it would simply make our code legible and easy to follow if these objects are logically related.

This is the function of **inheritance** - it allows us to make it such that some objects are "parents" and "children" to each other. In our car dealership example, we could create something like a car object and then have the specific car types be variations on this baseline car object.

Let's just take a look at some code.

```python
class Car:
    def __init__(self, id, name, make, price):
        self._id = id
        self._name = name
        self._make = make
        self._price = price

    def get_name(self):
        return self._name

    def calc_price(self):
        return self._price

class Sports_Car(Car):
    def __init__(self, id, name, make, price):
        super().__init__(id, name, make, price)

class EV_Car(Car):
    def __init__(self, id, name, make, price, discount):
        super().__init__(id, name, make, price)
        self._discount = discount

    def calc_price(self):
         return super().calc_price() * self._discount
```

What's happening here is that the `Sports_Car` and `EV_Car` objects are using inheritance to use the `id`, `name`, `make`, and `price` parameters from the `Car` object. By using **inheritance**, we've made an **object hierarchy** with a **parent-child** relationship ( `Car` being the parent, `Sports_Car` and `EV_Car` being the children).

## Python abc's - Parent and Child Classes

In our example, `Car` is the parent class. We should initiate `Sports_Car` or `EV_Car` or make a new child-class of `Car`,

 like `SUV_Car` or `Truck_Car`, whenever we need a car object.

## Encapsulation 

Encapsulation is the practice of keeping fields in a class private. Then providing access to the fields via public methods. This way, we're able to change private fields (like, in our example, `id`, `name`, `make`, and `price`), without affecting other parts of our code that use these fields, as long as the methods through which these are accessed stay the same.

Let's take a look at some code that demonstrates this.

```python
class Purchase_Car:
    def __init__(self, car):
        self._car = car

    def purchase(self):
        print("Purchased Car with ID: ", self._car.get_id(), " and Name: ", self._car.get_name())
```

In the `Purchase_Car` class, the code accesses `id` and `name` via the `get_id` and `get_name` methods, upholding the encapsulation practice.

## Polymorphism 

Polymorphism is the practice of designing objects to share behaviors. With Python, polymorphism refers to the way in which object classes can share the same method name.

This is best explained with some code:

```python
class Not_a_Car:
    def __init__(self, id, name):
        self._id = id
        self._name = name

    def get_name(self):
        return self._name

    def get_id(self):
        return self._id

def purchase_item(item):
    print("Purchased item with ID: ", item.get_id(), " and Name: ", item.get_name())

car = Sports_Car(1, "Fast Car", "Ferrari", 100000)
not_car = Not_a_Car(2, "Bike")

purchase_item(car)
purchase_item(not_car)
```

In the above example, we've created a `Not_a_Car` class which shares the same methods `get_id` and `get_name` as the `Car` class. This way, we can use the `purchase_item` function for objects of both classes. The function does not need to know the type of the object it is dealing with as long as the object has the necessary methods, demonstrating **polymorphism**.

And that's a wrap! The above example covers the core concepts around Python Inheritance, Composition, and Polymorphism. Let's build better software with these concepts in mind.
