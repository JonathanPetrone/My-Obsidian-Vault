Functional programming is a style (or "paradigm" if you're pretentious) of programming where we compose functions instead of mutating state (updating the value of variables).

- **Functional programming** is more about declaring _what_ you want to happen, rather than _how_ you want it to happen.
- Imperative (or procedural) programming declares both the _what_ and the _how_.

## First-class functions:
**First-class functions** are a key concept in programming where functions are treated as first-class citizens. This means that functions in a language can be:

- **Assigned to variables**
- **Passed as arguments**
- **Returned from other functions**
- **Stored in data structures**

**Why Are First-Class Functions Useful?**
- **Flexibility**: Enables functional programming patterns like higher-order functions (functions that take or return other functions).
- **Code Reusability**: Reduces redundancy by abstracting operations.
- **Dynamic Behavior**: Allows creating and modifying behavior at runtime.

## Pure functions 
**Pure functions:**
- They _always_ return the same value given the same arguments.
- Running them causes no side effects

In short: pure functions don't do anything with anything that exists outside of their scope.

## Function transformations:
"Function transformation" is just a more concise way to describe a specific type of higher order function. It's when a function takes a function (or functions) as input and returns a _new_ function.

#### Currying

Function currying is a specific _kind_ of function transformation where we translate a single function that accepts multiple arguments into _multiple_ functions that each accept a _single_ argument.

This is a "normal" 3-argument function:

```py
box_volume(3, 4, 5)
```

This is a "curried" _series of functions_ that does the same thing:

```py
box_volume(3)(4)(5)
```


## Closures

A closure is a function that references variables from outside its own function body. The function definition _and its environment_ are bundled together into a single entity.

Put simply, a closure is just a function that **keeps track of some values** from the place where it was _defined_, no matter where it is executed later on.

# Decorators
Python decorators are just syntactic sugar for higher-order functions.



