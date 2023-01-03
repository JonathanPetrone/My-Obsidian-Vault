The SOLID principles are one of the most influential sets of object-oriented guidelines used today. What is interesting about studying these principles is how they relate to the fundamental object-oriented encapsulation, inheritance, polymorphism, and composition, specifically in the debate of composition over inheritance.

## SOLID principles in object-oriented design:

1. **Single responsibility principle** - Each class and module should focus on one thing. The goal is that a change to one method will not affect other methods.
2. **Open/close principle** - You should be ablt to extend a class behavior without modifying it.
3. **Liskov substitution principle** - The design must provide the ability to replace any instance of a parent class with an instance of one of its child classes. If a parent class can do something, a child class must also be able to do it.
4. **Interface segregation principle** - It is better to have many small interfaces than a few larger ones.
5. **Dependency inversion principle** -  Code should depend on abstractions. Often the terms _dependency inversion_ and _dependency injection_ are used interchangeably; however, here are some key terms to understand as we discuss this principle:
	- **Dependency inversion**—The principle of inverting the dependencies
	- **Dependency injection**—The act of inverting the dependencies
	- **Constructor injection**—Performing dependency injection via the constructor
	- **Parameter injection**—Performing dependency injection via the parameter of a method, like a setter



see also: [[Object-oriented programming]]

## Reference:
Object-Oriented Thought Process, 5th Edition (ch. 12) - M. Weisfeld

## Similar:

## Opposite:

### Terms that signify un-reusable code (in contrast to SOLID principles):

- **Rigidity** - When a change to one part of a program can break another.
- **Fragility** - When things break in unrelated places.
- **Immobility** - When code cannot be reused outside its original context.

## Theme/Questions:

## What does this lead to?

