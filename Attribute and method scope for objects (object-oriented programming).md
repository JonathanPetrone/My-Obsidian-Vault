When you create an object it is constructed separately and is allocated its own separate memory. This makes it so that each of these objects has a unique identity and state. 

However, some attributes (data) and methods may, if properly declared, be shared by all the objects instantiated from the same class, thus sharing the memory allocated for these class attributes and methods.

In OOP-languages there are three types of scopes for attributes:

-   **Local attributes**
	- Local attributes are owned by a specific method
-   **Object attributes**
	- Object attributes are owned by a specific object, to access a object variable, you can use a pointer called `this` in the C-based languages:
-   **Class attributes**
	- It is possible for two or more objects of the same class to share attributes. In Java, C#, C++, and Swift, you do this by making the attribute _static_. Thus, all objects of the class use the same memory location for the variable declared as static. Essentially, the class then has a single copy. This is about as close to global data as we get in OO design.


See also: [[Object-oriented programming]]

## Reference:
- Object-Oriented Thought Process, 5th Edition (ch. 3) - M. Weisfeld 

## Similar:
[[Scope in programming]]

## Opposite:

## Theme/Questions:

## What does this lead to?