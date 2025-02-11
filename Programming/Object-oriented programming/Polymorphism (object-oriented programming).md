**Polymorphism** is a Greek word that literally means many shapes.

Polymorphism describes situations in which something occurs in several different forms. In computer science, it often describes the concept that you can access objects of different types through the same interface. Each type can provide its own independent implementation of this interface.

In OOP when a message is sent to an object, the object must have a method defined to respond to that message. In an [[Inheritance (object-oriented programming)]] hierarchy, all subclasses inherit the interfaces from their superclass. However, because each subclass is a separate entity, each might require a separate response to the same message.

For example, consider a class of `Shape` and the behavior called `draw`. When you tell somebody to draw a shape, the first question asked is, “What shape?” No one can draw a shape, because it is an abstract concept. You must therefore specify a concrete shape. To do this, you provide the actual implementation in a class called `Circle` or whatever shape child class that floats your boat.

see also: [[Object-oriented programming]]

## Reference:
- Object-Oriented Thought Process, 5th Edition (ch. 1)  - M. Weisfeld
- https://stackify.com/oop-concept-polymorphism/

## Similar:

## Opposite:

## Theme/Questions:

## What does this lead to?