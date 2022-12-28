#todo 

#### Serializing and Marshaling Objects

We have already discussed the problem of using objects in environments that were originally designed for structured programming. The middleware example, where we wrote objects to a relational database, is one good example. We also touched on the problem of writing an object to a flat file or sending it over a network.

To send an object over a wire (for example, to a file, over a network), the system must deconstruct the object (flatten it out), send it over the wire, and then reconstruct it on the other end of the wire. This process is called _serializing_ an object. The act of sending the object across a wire is called _marshaling_ an object. A serialized object, in theory, can be written to a flat file and retrieved later, in the same state in which it was written.

The major issue here is that the serialization and deserialization must use the same specifications. It is sort of like an encryption algorithm. If one object encrypts a string, the object that wants to decrypt it must use the same encryption algorithm. Java provides an interface called `Serializable` that provides this translation.

This is another reason why data is separated from behaviors nowadays. It’s far simpler to create an interface for a data contract and push that out to a web service than it is to make sure people have the same code on both sides.

## Reference:
- Object-Oriented Thought Process, 5th Edition (ch. 5) - M. Weisfeld

## Similar:

## Opposite: 

## Theme/Questions:

## What does this lead to?