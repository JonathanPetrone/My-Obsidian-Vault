Access modifiers (or access specifiers) are keywords in object-oriented languages that set the accessibility of classes, methods, and other members. Access modifiers are a specific part of programming language syntax used to facilitate the encapsulation of components. 

Some exampels of accessibility levels from C#:

- **public**: Access isn't restricted.
- **protected**: Access is limited to the containing class or types derived from the containing class.
- **internal**: Access is limited to the current assembly.
- **protected internal**: Access is limited to the current assembly or types derived from the containing class.
- **private**: Access is limited to the containing type.
- **private protected**: Access is limited to the containing class or types derived from the containing class within the current assembly.
- **file**: The declared type is only visible in the current source file. File scoped types are generally used for source generators.

## Reference:
- https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/access-modifiers 
- https://en.wikipedia.org/wiki/Access_modifiers

## Similar:
[[Scope in programming]]

## Opposite:

## Theme/Questions:

## What does this lead to?
[[Encapsulation (object-oriented programming)]]