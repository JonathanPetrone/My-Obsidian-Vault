When you take reusability in mind you can use parts of your system again and again. [[Inheritance (object-oriented programming)]] and [[Composition (object-oriented programming)]] are both important techniques when building reusable OO systems.

Using any amount of inheritance or composition comes down to trade-offs.

**Challenges and opportunities with inheritance:**

- You don't have to rewrite shared methods, they are in a shared place.
- For shared methods and attribute you could make changes in one place and it affects all children.
- Maybe all future children of the parent doesn't share all characteristics of their parent, lets say a child class of the bird class that can't fly, then you have to create another inheritance level below the bird class for all the current and future children, for birds that can fly and birds that cannot.
- You often start with a single class, and then factor out some of the commonality so that the parent is the most generalized version. This concept, sometimes called _generalization-specialization_, is yet another important consideration when using inheritance. The idea is that as you make your way down the inheritance tree, things get more specific. 
- As you factor more and more out, the more complex your system gets. This leaves you with a conundrum: Do you want to live with a more accurate model or a system with less complexity?
- When using inheritance, [[Encapsulation (object-oriented programming)]] is inherently weakened within a class hierarchy.
- Lets you use polymorphic behavior in subclasses for an example

**Challenges and opportunities with composition:**
- Using too much composition in a system can also lead to more complexity
- It is natural to think of objects as containing other objects


see also: [[Object-oriented programming]]

## Reference:
Object-Oriented Thought Process, 5th Edition (ch. 7) - M. Weisfeld

## Similar:

## Opposite:

## Theme/Questions:

## What does this lead to?