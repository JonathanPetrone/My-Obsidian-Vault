An [[Interface]] can be defined as: "a point where two systems, subjects, organizations, etc. meet and interact."

Implementation on the other hand, in the context of OOP, could be seen as how something is built or how it works.

Technically in OOP, **anything that is not a public interface can be considered the implementation**.  

In a car the steering wheel is part of the **interface**, but some parts of the cars inner workings i.e its **implementations** are hidden from the driver. 

When you design a class, what the other party needs to know and, what the other party does not need to know are of vital importance. Doing this right leads to a better OO design. In the best case, _only_ the services the other party needs are presented, so the general rule is to always provide as little knowledge of the inner workings of the class as possible.


## Design goal regarding interfaces vs. implementation

One goal regarding the implementation should be kept in mind: A change to the implementation _should not_ require a change to the other party's code (what he/she/it needs to communicate). The interface includes the syntax to call a method and return a value. If this interface does not change, the other party does not care whether the implementation is changed.

See also: [[Object-oriented programming]]

## Reference:
Object-Oriented Thought Process, 5th Edition (ch. 2) - M. Weisfeld

## Similar:

## Opposite:

## Theme/Questions:

## What does this lead to?