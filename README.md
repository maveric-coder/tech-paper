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

### *#Pseudocode*
In this example, the Composite pattern enables us to implement stacking of geometric shapes in a graphical editor.
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

<img src = "https://github.com/maveric-coder/tech-paper/blob/main/pics/decorator.png" />

The decorator is a design pattern that enables us to add behaviours to an individual object dynamically without affecting the behaviour of other objects of the same class. It adheres to the Single Responsibility Principle as it allows functionality to be divided between classes with distinct demands. They are more efficient than subclassing as new behaviours can be added to an object without instantiating a completely new object.

Extending a class is the first thing that comes to mind when we need to alter an object’s behaviour. However, inheritance has several serious limitations.
Inheritance is static. We can't alter the behaviour of an existing object at runtime. We will have to replace the whole object with another object of a different subclass.
In many programming languages, inheritance doesn't allow to inherit behaviours of multiple classes simultaneously.
One of the most convenient ways to tackle these limitations is by using Aggregation/Composition rather than using inheritance. With this approach, it becomes very easy to substitute the linked "helper" object with another which ultimately enables us to change the behaviour of the container at runtime. 





### *#Pseudocode*

In this example, the Decorator pattern lets you compress and encrypt sensitive data independently from the code that actually uses this data.

<img src = "https://github.com/maveric-coder/tech-paper/blob/main/pics/example%20(1).png" />

```

# The base Component interface defines operations that can be altered by
# decorators.
class Component
  # @return [String]
  def operation
    raise NotImplementedError, "#{self.class} has not implemented method '#{__method__}'"
  end
end

# Concrete Components provide default implementations of the operations. There
# might be several variations of these classes.
class ConcreteComponent < Component
  # @return [String]
  def operation
    'ConcreteComponent'
  end
end

# The base Decorator class follows the same interface as the other components.
# The primary purpose of this class is to define the wrapping interface for all
# concrete decorators. The default implementation of the wrapping code might
# include a field for storing a wrapped component and the means to initialize
# it.
class Decorator < Component
  attr_accessor :component

  # @param [Component] component
  def initialize(component)
    @component = component
  end

  # The Decorator delegates all work to the wrapped component.
  def operation
    @component.operation
  end
end

# Concrete Decorators call the wrapped object and alter its result in some way.
class ConcreteDecoratorA < Decorator
  # Decorators may call parent implementation of the operation, instead of
  # calling the wrapped object directly. This approach simplifies extension of
  # decorator classes.
  def operation
    "ConcreteDecoratorA(#{@component.operation})"
  end
end

# Decorators can execute their behavior either before or after the call to a
# wrapped object.
class ConcreteDecoratorB < Decorator
  # @return [String]
  def operation
    "ConcreteDecoratorB(#{@component.operation})"
  end
end

# The client code works with all objects using the Component interface. This way
# it can stay independent of the concrete classes of components it works with.
def client_code(component)
  # ...

  print "RESULT: #{component.operation}"

  # ...
end

# This way the client code can support both simple components...
simple = ConcreteComponent.new
puts 'Client: I\'ve got a simple component:'
client_code(simple)
puts "\n\n"

# ...as well as decorated ones.
#
# Note how decorators can wrap not only simple components but the other
# decorators as well.
decorator1 = ConcreteDecoratorA.new(simple)
decorator2 = ConcreteDecoratorB.new(decorator1)
puts 'Client: Now I\'ve got a decorated component:'
client_code(decorator2)
```



## 3. Composite

This pattern describes a group of objects that are treated similarly as that of a single instance of the same type of object. It intends to compose objects into tree structures and then work with these structures as if they were individual objects.


### *#Pseudocode*

The CompoundGraphic class is a container that can consist of any sub-shape, but the compound shape has limited methods as a simple shape. However, instead of doing something by itself, a compound shape passes the request recursively to its children and aggregates the result.
The client code works with all shaps through the single interface typical to all shape classes. Thus, the performance does not get affected, and the client can work with sophisticated object structures.

```
# The base Component class declares common operations for both simple and
# complex objects of a composition.
class Component
  # @return [Component]
  def parent
    @parent
  end

  # Optionally, the base Component can declare an interface for setting and
  # accessing a parent of the component in a tree structure. It can also provide
  # some default implementation for these methods.
  def parent=(parent)
    @parent = parent
  end

  # In some cases, it would be beneficial to define the child-management
  # operations right in the base Component class. This way, you won't need to
  # expose any concrete component classes to the client code, even during the
  # object tree assembly. The downside is that these methods will be empty for
  # the leaf-level components.
  def add(component)
    raise NotImplementedError, "#{self.class} has not implemented method '#{__method__}'"
  end

  # @abstract
  #
  # @param [Component] component
  def remove(component)
    raise NotImplementedError, "#{self.class} has not implemented method '#{__method__}'"
  end

  # You can provide a method that lets the client code figure out whether a
  # component can bear children.
  def composite?
    false
  end

  # The base Component may implement some default behavior or leave it to
  # concrete classes (by declaring the method containing the behavior as
  # "abstract").
  def operation
    raise NotImplementedError, "#{self.class} has not implemented method '#{__method__}'"
  end
end

# The Leaf class represents the end objects of a composition. A leaf can't have
# any children.
#
# Usually, it's the Leaf objects that do the actual work, whereas Composite
# objects only delegate to their sub-components.
class Leaf < Component
  # return [String]
  def operation
    'Leaf'
  end
end

# The Composite class represents the complex components that may have children.
# Usually, the Composite objects delegate the actual work to their children and
# then "sum-up" the result.
class Composite < Component
  def initialize
    @children = []
  end

  # A composite object can add or remove other components (both simple or
  # complex) to or from its child list.

  # @param [Component] component
  def add(component)
    @children.append(component)
    component.parent = self
  end

  # @param [Component] component
  def remove(component)
    @children.remove(component)
    component.parent = nil
  end

  # @return [Boolean]
  def composite?
    true
  end

  # The Composite executes its primary logic in a particular way. It traverses
  # recursively through all its children, collecting and summing their results.
  # Since the composite's children pass these calls to their children and so
  # forth, the whole object tree is traversed as a result.
  def operation
    results = []
    @children.each { |child| results.append(child.operation) }
    "Branch(#{results.join('+')})"
  end
end

# The client code works with all of the components via the base interface.
def client_code(component)
  puts "RESULT: #{component.operation}"
end

# Thanks to the fact that the child-management operations are declared in the
# base Component class, the client code can work with any component, simple or
# complex, without depending on their concrete classes.
def client_code2(component1, component2)
  component1.add(component2) if component1.composite?

  print "RESULT: #{component1.operation}"
end

# This way the client code can support the simple leaf components...
simple = Leaf.new
puts 'Client: I\'ve got a simple component:'
client_code(simple)
puts "\n"

# ...as well as the complex composites.
tree = Composite.new

branch1 = Composite.new
branch1.add(Leaf.new)
branch1.add(Leaf.new)

branch2 = Composite.new
branch2.add(Leaf.new)

tree.add(branch1)
tree.add(branch2)

puts 'Client: Now I\'ve got a composite tree:'
client_code(tree)
puts "\n"

puts 'Client: I don\'t need to check the components classes even when managing the tree:'
client_code2(tree, simple)
```



## 4. Observer

This design pattern enables to define a subscription mechanism which notifies multiple objects automatically, in case of any changes made on the method under observation. It is used for implementing distributed event handling systems in event-driven software. 


### *#Pseudocode*

In this example, the Observer pattern lets the text editor object notify other service objects about changes in its state. The list of subscribers is compiled dynamically: objects can start or stop listening to notifications at runtime, depending on the desired behavior of the app.

```
# The Subject interface declares a set of methods for managing subscribers.
class Subject
  # Attach an observer to the subject.
  def attach(observer)
    raise NotImplementedError, "#{self.class} has not implemented method '#{__method__}'"
  end

  # Detach an observer from the subject.
  def detach(observer)
    raise NotImplementedError, "#{self.class} has not implemented method '#{__method__}'"
  end

  # Notify all observers about an event.
  def notify
    raise NotImplementedError, "#{self.class} has not implemented method '#{__method__}'"
  end
end

# The Subject owns some important state and notifies observers when the state
# changes.
class ConcreteSubject < Subject
  # For the sake of simplicity, the Subject's state, essential to all
  # subscribers, is stored in this variable.
  attr_accessor :state

  # @!attribute observers
  # @return [Array<Observer>] attr_accessor :observers private :observers

  def initialize
    @observers = []
  end

  # List of subscribers. In real life, the list of subscribers can be stored
  # more comprehensively (categorized by event type, etc.).

  # @param [Obserser] observer
  def attach(observer)
    puts 'Subject: Attached an observer.'
    @observers << observer
  end

  # @param [Obserser] observer
  def detach(observer)
    @observers.delete(observer)
  end

  # The subscription management methods.

  # Trigger an update in each subscriber.
  def notify
    puts 'Subject: Notifying observers...'
    @observers.each { |observer| observer.update(self) }
  end

  # Usually, the subscription logic is only a fraction of what a Subject can
  # really do. Subjects commonly hold some important business logic, that
  # triggers a notification method whenever something important is about to
  # happen (or after it).
  def some_business_logic
    puts "\nSubject: I'm doing something important."
    @state = rand(0..10)

    puts "Subject: My state has just changed to: #{@state}"
    notify
  end
end

# The Observer interface declares the update method, used by subjects.
class Observer
  # Receive update from subject.
  def update(_subject)
    raise NotImplementedError, "#{self.class} has not implemented method '#{__method__}'"
  end
end

# Concrete Observers react to the updates issued by the Subject they had been
# attached to.

class ConcreteObserverA < Observer
  # @param [Subject] subject
  def update(subject)
    puts 'ConcreteObserverA: Reacted to the event' if subject.state < 3
  end
end

class ConcreteObserverB < Observer
  # @param [Subject] subject
  def update(subject)
    return unless subject.state.zero? || subject.state >= 2

    puts 'ConcreteObserverB: Reacted to the event'
  end
end

# The client code.

subject = ConcreteSubject.new

observer_a = ConcreteObserverA.new
subject.attach(observer_a)

observer_b = ConcreteObserverB.new
subject.attach(observer_b)

subject.some_business_logic
subject.some_business_logic

subject.detach(observer_a)

subject.some_business_logic
```
## 5. Command

It is a design pattern that turns a request into a stand-alone object that consists of all the information about the request.
It is a design pattern in which an object is used to encapsulate all the information needed to perform an action or trigger an event at a later time. 


### *#Pseudocode*

In this example, the Command pattern helps to track the history of executed operations and makes it possible to revert an operation if needed.


```# The Command interface declares a method for executing a command.
class Command
  # @abstract
  def execute
    raise NotImplementedError, "#{self.class} has not implemented method '#{__method__}'"
  end
end

# Some commands can implement simple operations on their own.
class SimpleCommand < Command
  # @param [String] payload
  def initialize(payload)
    @payload = payload
  end

  def execute
    puts "SimpleCommand: See, I can do simple things like printing (#{@payload})"
  end
end

# However, some commands can delegate more complex operations to other objects,
# called "receivers".
class ComplexCommand < Command
  # Complex commands can accept one or several receiver objects along with any
  # context data via the constructor.
  def initialize(receiver, a, b)
    @receiver = receiver
    @a = a
    @b = b
  end

  # Commands can delegate to any methods of a receiver.
  def execute
    print 'ComplexCommand: Complex stuff should be done by a receiver object'
    @receiver.do_something(@a)
    @receiver.do_something_else(@b)
  end
end

# The Receiver classes contain some important business logic. They know how to
# perform all kinds of operations, associated with carrying out a request. In
# fact, any class may serve as a Receiver.
class Receiver
  # @param [String] a
  def do_something(a)
    print "\nReceiver: Working on (#{a}.)"
  end

  # @param [String] b
  def do_something_else(b)
    print "\nReceiver: Also working on (#{b}.)"
  end
end

# The Invoker is associated with one or several commands. It sends a request to
# the command.
class Invoker
  # Initialize commands.

  # @param [Command] command
  def on_start=(command)
    @on_start = command
  end

  # @param [Command] command
  def on_finish=(command)
    @on_finish = command
  end

  # The Invoker does not depend on concrete command or receiver classes. The
  # Invoker passes a request to a receiver indirectly, by executing a command.
  def do_something_important
    puts 'Invoker: Does anybody want something done before I begin?'
    @on_start.execute if @on_start.is_a? Command

    puts 'Invoker: ...doing something really important...'

    puts 'Invoker: Does anybody want something done after I finish?'
    @on_finish.execute if @on_finish.is_a? Command
  end
end

# The client code can parameterize an invoker with any commands.
invoker = Invoker.new
invoker.on_start = SimpleCommand.new('Say Hi!')
receiver = Receiver.new
invoker.on_finish = ComplexCommand.new(receiver, 'Send email', 'Save report')

invoker.do_something_important
```

