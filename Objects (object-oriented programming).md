An object can be defined as "a person or a thing to which a specified action or feeling is directed." ([[Object]])

Objects in OOP are defined by:

- **Attributes**, which is data stored in an object which represents the state of the object.
- **Behavior**, represents what the object can do, in OOP called methods.

When an object is created, we say that the objects are *instantiated*. But in OOP an object cannot be instantiated without a class (see: [[Classes (object-oriented programming)]]) .

### Communication between Objects
Objects communicates through messages between each other. This is their communication mechanism. 

Lets say **Object A** invokes a method of **Object B**, Object A is then sending a message to Object B. Object B’s response to the message is defined by its return value.

### Object persistance
_Persistence_ is the concept of maintaining the state of an object. When you run a program, if you don’t save the object in some manner, the object dies, never to be recovered. These transient objects might work in some applications, but in most business systems, the state of the object must be saved for later use.

In its simplest form, an object can persist by being serialized and written to a flat file. There are three primary storage devices to consider:

 - **Flat file system**—More often than not, objects are serialized to XML and/or JSON and written to some sort of file system or data store or web endpoint. They can be put into a database or written to disk, which is the most common practice nowadays.
- **Relational database**—Some sort of middleware is necessary to convert an object to a relational model.
- **NoSQL database**—MongoDB or Cosmos DB are two of the bigger names in this space.

see also: [[Object-oriented programming]], [[Attribute and method scope for objects (object-oriented programming)]]

## Reference:
Object-Oriented Thought Process, 5th Edition (ch. 1 & 5)  - M. Weisfeld

## Similar:

## Opposite:

## Theme/Questions:

## What does this lead to?