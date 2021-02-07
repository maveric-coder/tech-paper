# Design Patterns


### What is a Design Pattern?

We encounter problems in our day-to-day life and so as the programmers while designing or creating programs. Most of the times the problem faced can be categorised and people have found universal ways or patterns to solve these problems. And as a programmer, we also face problems; which can be classified and can be solved using patterns.
In other words, it can be stated as a solution to recurring problems that software engineers face often. And it is more like a procedure or description on how to resolve the issues and build an efficient solution.

<img src = "https://github.com/maveric-coder/tech-paper/blob/main/pics/ruby.png" />

### Types of design patterns
There are approximately 22 design patterns and  are grouped into 3 types:

1. Creational: 
These patterns provide various object creation mechanisms, which increase flexibility and reuse of existing code.

2. Structural: These patterns explain how to assemble objects and classes into larger structures while keeping these structures flexible and efficient.

3. Behavioral: These patterns are designed depending on how one class communicates with others.


## 5 Types of degin patterns:

## 1. Prototype
Prototype is a creational design pattern that makes use of existing objects without making the code dependent on their classes. It avoids the inherent cost of creating a new object in the standard way. The implementation of this method is typical in all classes. This method creates an object of the current class and carries over all of the field values of the old object into the new one. 

<img src = "https://github.com/maveric-coder/tech-paper/blob/main/pics/prototype.png" />


## 2. Decorator
The decorator is a design pattern that enables us to add behaviours to an individual object dynamically without affecting the behaviour of other objects of the same class. It adheres to the Single Responsibility Principle as it allows functionality to be divided between classes with distinct demands. They are more efficient than subclassing as new behaviours can be added to an object without instantiating a completely new object.

Extending a class is the first thing that comes to mind when we need to alter an object’s behaviour. However, inheritance has several serious limitations.
Inheritance is static. We can't alter the behaviour of an existing object at runtime. We will have to replace the whole object with another object of a different subclass.
In many programming languages, inheritance doesn't allow to inherit behaviours of multiple classes simultaneously.
One of the most convenient ways to tackle these limitations is by using Aggregation/Composition rather than using inheritance. With this approach, it becomes very easy to substitute the linked "helper" object with another which ultimately enables us to change the behaviour of the container at runtime. 







## 3. Composite

This pattern describes a group of objects that are treated similarly as that of a single instance of the same type of object. It intends to compose objects into tree structures and then work with these structures as if they were individual objects.
