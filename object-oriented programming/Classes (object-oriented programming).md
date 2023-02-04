A class is a blueprint for an object. When you instantiate (create) an object, you use a class as the basis for how the object is built. A class defines the **attributes** (data) and **behaviors** (methods) that all objects created with this class will possess.

In OO software, the class comes before the object since an object cannot be instantiated without a class.

![[Pasted image 20221227183105.png]]

You can use [[UML]] (_Unified Modeling Language_) class diagrams to illustrate the classes that you build.

### Building blocks of a class

Classes generally consists of:
- **A class name**
	- For identification, preferably descriptive.
- **Comments**
	- Providing extra documentation.
- **Attributes**
	- Which stores data about objects, which could include references to other objects. (see also: [[Attribute and method scope for objects (object-oriented programming)]])
- **Methods**
	- Public interface methods:
		- [[Constructors (object-oriented programming)]]
		- [[Accessor methods (object-oriented programming)]]
	- Private implementation methods:
		- Private methods called internally from the class itself
			- an example of this is an internal method that provides encryption

### Design guideline for classes:
- "If a class does not provide a useful service to the user it shouldn't have been built in the first place"
- Provide the minimum public interface to makes the class as concise as possible
- A constructor provided should put an object into an initial, safe state. This includes issues such as attribute initialization and memory management. Memory management can be handled via a garbage collection mechanism, C# and Java do this automatically.
- Designing how a class handles potential errors is important, see: [[Error handling in programming]].
- Put comments in your code explaining the class.
- When designing a class, make sure you are aware of how other objects will interact with it.
- Think about reusability and extensibility with your classes, note that classes should be open for extension but closed for modification.
- Provide a way to copy and compare objects (see: [[Object operations and their complexity (object-oriented programming)]])
- Localize attributes and behaviors as much as possible [[Scope in programming]]
- Design with maintainability in mind. Separate pieces of code tend to be more maintainable, but you also need to reduce interdependent code—that is, changes in one class have no impact or minimal impact on other classes, to increase maintainability.

### More on class design: 
If the class **Is A** then -> Use inheritance
If the class **Has A** then -> Use composition
If the class **implements** X -> Use interface


See also: [[Object-oriented programming]], [[Objects (object-oriented programming)]]

## Reference:
Object-Oriented Thought Process, 5th Edition (ch. 1, 4 & 5) - M. Weisfeld

## Similar:

## Opposite:

## Theme/Questions:

## What does this lead to?
[[Encapsulation (object-oriented programming)]]