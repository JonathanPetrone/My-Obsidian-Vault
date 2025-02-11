Cardinality can be described as the number of objects that participate in an association, and whether the participation is optional or mandatory.

Here is an example from the book (Object-Oriented Thought Process ch. 9): 

## Example: Cardinality of Class Associations

| Optional/Association    | Cardinality | Mandatory | Meaning                                       |
| ----------------------- | ----------- | --------- | --------------------------------------------- |
| Employee/Division       | 1           | Mandatory | Every employee must have an assigned division |
| Employee/JobDescription | 1 . . n     | Mandatory | Every employee must have at least a job desciption, but can have many                                              |
| Employee/Spouse         | 0 . . 1     | Optional  | Every employee can have a Spouse                                             |
| Employee/Child          | 0 . . n     | Optional  | Every employee can have 0 or more children                                              |

**Multiple Object Associations**
Classes that have a **one-to-many** relationship are represented by arrays in the code. 

**Optional Associations**
One of the most important issues when dealing with associations is to make sure that your application is designed to check for optional associations. This means that your code must check to see whether the association is `null`.

see also: [[Object-oriented programming]], [[Composition (object-oriented programming)]]

## Reference:
Object-Oriented Thought Process, 5th Edition (ch.  9) - M. Weisfeld

## Similar:

## Opposite:

## Theme/Questions:

## What does this lead to?