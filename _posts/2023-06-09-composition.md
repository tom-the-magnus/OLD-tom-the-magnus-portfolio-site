---
layout: post
title: "Python OOP - Compositions"
description: "Guide on python comprehensions"
date: 2023-06-10
feature_image: images/python-compositions.png
tags: [python]
---

To better demonstrate composition using our car dealership scenario from [Python OOP - Inheritance]([url](http://tom-blog.xyz/Inheritance)), let's revisit the previous code and tweak the structure. 

Let's say we want to add the behavior of selling a car to our `Car` class. However, instead of subclassing each type of car, we can define a separate class, `SalesBehavior`, which will handle the sales-related operations.

<!--more-->

The `SalesBehavior` class could look something like this:

```python
# In sales_behavior.py

class SalesBehavior:
    def __init__(self, car_name):
        self.car_name = car_name

    def sell(self, quantity):
        print(f'Sold {quantity} units of {self.car_name}.')
```

The `SalesBehavior` class takes a `car_name` as an attribute and implements a `sell` method, which takes a `quantity` argument.

Now, let's use this `SalesBehavior` class in our `Car` class via composition. We will add a new attribute, `sales_behavior`, to our `Car` class, which will be an instance of `SalesBehavior`:

```python
# In sales_behavior.py

class SalesBehavior:
    def __init__(self, car_name):
        self.car_name = car_name

    def sell(self, quantity):
        print(f'Sold {quantity} units of {self.car_name}.')

```

Now, each instance of `Car` has its own `sales_behavior` which is an instance of `SalesBehavior`. The `Car` class doesn't know anything about how selling works, it just delegates the `sell` method to its `sales_behavior` instance.

```python
# In program.py

import vehicles
import inventory
import sales

car1 = vehicles.Car(1, 'Ferrari F8', 274280)
car2 = vehicles.Car(2, 'Honda Accord', 24525)
car3 = vehicles.Car(3, 'Toyota Yaris', 15950)
car4 = vehicles.Car(4, 'Ford F-150', 28155)
cars = [car1, car2, car3, car4]
sales_system = sales.SalesSystem()
sales_system.track(cars, 5)
inventory_system = inventory.InventorySystem()
inventory_system.track_inventory(cars)

```

In this way, you can avoid the "subclass explosion" problem by using composition instead of inheritance to add new behaviors to a class. If we wanted to add more features, like leasing or financing, we could do so by defining new classes (`LeasingBehavior`, `FinancingBehavior`, etc.) and adding instances of these classes to our `Car` class.

The advantage of this approach is that our `Car` class doesn't become bloated with multiple methods for all these behaviors. Instead, each behavior is encapsulated in its own class, and a `Car` simply has instances of these behavior classes. Furthermore, it's easy to add, remove, or modify behaviors without affecting the `Car` class or any of its instances.

As a result, composition provides us with a way to manage complexity in our programs by breaking down large classes into smaller, more manageable components. In turn, these components can be easily tested, debugged, and maintained, making our programs more robust and easier to understand.
