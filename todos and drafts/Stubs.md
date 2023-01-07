#todo 

For OOP

#### Testing the Interface

The minimal implementations of the interface are often called _stubs._ (Gilbert and McCarty have a good discussion on stubs in _Object-Oriented Design in Java_.) By using stubs, you can test the interfaces without writing any _real_ code. In the following example, rather than connecting to an actual database, stubs are used to verify that the interfaces are working properly (from the user’s perspective—remember that interfaces are meant for the user). Thus, the implementation is not necessary at this point. In fact, it might cost valuable time and energy to complete the implementation yet because the design of the interface will affect the implementation, and the interface is not yet complete.

In [Figure 5.6](https://learning.oreilly.com/library/view/object-oriented-thought-process/9780135182130/ch05.xhtml#ch05fig06), note that when a user class sends a message to the `DataBaseReader` class, the information returned to the user class is provided by code stubs and not by the actual database. (In fact, the database most likely does not exist yet.) When the interface is complete and the implementation is under development, the database can then be connected and the stubs disconnected.

## Reference
Object-Oriented Thought Process, 5th Edition (ch. 5) - M. Weisfeld

## Similar:

## Opposite: 

## Theme/Questions:

## What does this lead to?