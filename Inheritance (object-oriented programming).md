Inheritance enables a class to inherit the attributes (data) and behaviors (methods) of another class. This gives us the ability to create new classes by abstracting out (see: [[Abstraction (object-oriented programming)]]) common attributes and behaviors from another class. 

The superclass, or parent class (sometimes called base class), contains all the attributes and behaviors that are common to classes that inherit from it. 

The subclass, or child class (sometimes called derived class) is an extension of the superclass.

Dog (parent class) **->** French Bulldog (child class)

This relationship between a parent class and a child is often referred to as an _is-a relationship_. Thus in the above example French Bulldog is-a Dog (parent class)

### Multiple inheritance
There is also multiple inheritance which allows a class to inherit from more than one class. However, multiple inheritance can significantly increase the complexity of a system.

Multiple inheritance allows programmers to use more than one totally orthogonal (unrelated) hierarchy simultaneously, such as allowing Dog to inherit from _Cartoon character_ and _Pet_ and _Mammal_ and access features from within all of those classes.

see also: [[Object-oriented programming]]

## Reference:
- Object-Oriented Thought Process, 5th Edition (ch. 1 & 3) - M. Weisfeld
- https://en.wikipedia.org/wiki/Multiple_inheritance

## Similar:

## Opposite:
[[Composition (object-oriented programming)]]

## Theme/Questions:

## What does this lead to?

