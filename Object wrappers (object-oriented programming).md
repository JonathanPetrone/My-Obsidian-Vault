
In a software context, the term “wrapper” refers to programs or code that literally wrap around other program components.  The primary purpose of object wrappers is to provide consistent interfaces for the programmers who are using the code.

So when we wrap something in an object we do something like this:

![The illustration of Wrapping Structured Code resembles three concentric squares. The inner-most square denotes Structured Code, the middle denotes Method, and the outer-most denotes Object.](https://learning.oreilly.com/api/v2/epubs/urn:orm:book:9780135182130/files/graphics/06fig03.jpg)

We can use wrappers to encapsulate many types of functionality, which can range from traditional structured (legacy) and object-oriented (classes) code to nonportable (native) code. 

### More examples of this with different design patterns:
Object-oriented programming uses different **structural patterns** that basically always work in the same way regardless of the programming language used. The Adapter and Decorator design patterns are structural patterns and are also called wrappers.

An **adapter** conceals incompatible interfaces between individual classes. By translating one interface into another, an adapter allows the classes to communicate with each other. The wrapper – in this case the adapter – is the crucial link in the communication.

A **decorator** enables functions to be added to a class without changing the class itself. To the calling program object, the decorator has the same interface as the original class. In this way, nothing needs to be changed in the calling object. As the wrapper, the decorator passes the calls on to the class. The decorator directly handles new functions that are not included in the class. It returns the results in such a way that they appear to the calling object like results of the decorated class.

see also: [[Object-oriented programming]], [[Objects (object-oriented programming)]]

## Reference:
- Object-Oriented Thought Process, 5th Edition (ch. 6) - M. Weisfeld
- https://www.ionos.com/digitalguide/websites/web-development/what-is-a-wrapper/

## Similar:

## Opposite:

## Theme/Questions:

## What does this lead to?