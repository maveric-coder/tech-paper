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

<img src = "https://github.com/maveric-coder/tech-paper/blob/main/pics/prototype.png" />

Prototype is a creational design pattern that makes use of existing objects without making the code dependent on their classes. It avoids the inherent cost of creating a new object in the standard way. The implementation of this method is typical in all classes. This method creates an object of the current class and carries over all of the field values of the old object into the new one. 

<img src = "https://github.com/maveric-coder/tech-paper/blob/main/pics/example.png" />


```
# The example class that has cloning ability. We'll see how the values of field
# with different types will be cloned.
class Prototype
  attr_accessor :primitive, :component, :circular_reference

  def initialize
    @primitive = nil
    @component = nil
    @circular_reference = nil
  end

  # @return [Prototype]
  def clone
    @component = deep_copy(@component)

    # Cloning an object that has a nested object with backreference requires
    # special treatment. After the cloning is completed, the nested object
    # should point to the cloned object, instead of the original object.
    @circular_reference = deep_copy(@circular_reference)
    @circular_reference.prototype = self
    deep_copy(self)
  end

  # deep_copy is the usual Marshalling hack to make a deep copy. But it's rather
  # slow and inefficient, therefore, in real applications, use a special gem
  private def deep_copy(object)
    Marshal.load(Marshal.dump(object))
  end
end

class ComponentWithBackReference
  attr_accessor :prototype

  # @param [Prototype] prototype
  def initialize(prototype)
    @prototype = prototype
  end
end

# The client code.
p1 = Prototype.new
p1.primitive = 245
p1.component = Time.now
p1.circular_reference = ComponentWithBackReference.new(p1)

p2 = p1.clone

if p1.primitive == p2.primitive
  puts 'Primitive field values have been carried over to a clone. Yay!'
else
  puts 'Primitive field values have not been copied. Booo!'
end

if p1.component.equal?(p2.component)
  puts 'Simple component has not been cloned. Booo!'
else
  puts 'Simple component has been cloned. Yay!'
end

if p1.circular_reference.equal?(p2.circular_reference)
  puts 'Component with back reference has not been cloned. Booo!'
else
  puts 'Component with back reference has been cloned. Yay!'
end

if p1.circular_reference.prototype.equal?(p2.circular_reference.prototype)
  print 'Component with back reference is linked to original object. Booo!'
else
  print 'Component with back reference is linked to the clone. Yay!'
end

```



## 2. Decorator
The decorator is a design pattern that enables us to add behaviours to an individual object dynamically without affecting the behaviour of other objects of the same class. It adheres to the Single Responsibility Principle as it allows functionality to be divided between classes with distinct demands. They are more efficient than subclassing as new behaviours can be added to an object without instantiating a completely new object.

Extending a class is the first thing that comes to mind when we need to alter an object’s behaviour. However, inheritance has several serious limitations.
Inheritance is static. We can't alter the behaviour of an existing object at runtime. We will have to replace the whole object with another object of a different subclass.
In many programming languages, inheritance doesn't allow to inherit behaviours of multiple classes simultaneously.
One of the most convenient ways to tackle these limitations is by using Aggregation/Composition rather than using inheritance. With this approach, it becomes very easy to substitute the linked "helper" object with another which ultimately enables us to change the behaviour of the container at runtime. 







## 3. Composite

This pattern describes a group of objects that are treated similarly as that of a single instance of the same type of object. It intends to compose objects into tree structures and then work with these structures as if they were individual objects.
