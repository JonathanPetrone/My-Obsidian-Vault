## Three general groups of design patterns in OO

- **Creational patterns** create objects for you, rather than having you instantiate objects directly. This gives your program more flexibility in deciding which objects need to be created for a given case.
	- ex. *The factory method* that is useful when you dont know what kind of object you need before-hand.
- **Structural patterns** help you compose groups of objects into larger structures, such as complex user interfaces or accounting data.
	- ex. *The adapter design pattern* that is a way for you to create a different interface for a class that already exists. You create a new class that incorporates (wraps) the functionality of an existing class with a new and—ideally—better interface.
- **Behavioral patterns** help you define the communication between objects in your system and how the flow is controlled in a complex program.
	- ex. *The iterator design pattern* which provides a standard mechanism for traversing a collection, such as a vector. Functionality must be provided so that each item of the collection can be accessed one at a time. The iterator pattern provides information hiding, keeping the internal structure of the collection secure.

see also: [[Object-oriented programming]], [[Design patterns]]

## Reference:
Object-Oriented Thought Process, 5th Edition (ch. 10) - M. Weisfeld

## Similar:

## Opposite:
[[Anti-patterns]]

## Theme/Questions:

## What does this lead to?