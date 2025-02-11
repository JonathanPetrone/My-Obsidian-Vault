https://medium.com/@leodahal4/handle-errors-in-go-like-a-pro-5f2ab97c660b

In Go, error handling works a little differently than in other languages like Java, or Python, where we have try/catch to handle the errors and stack traces to trace them down but, errors in Go are not considered as exceptions but rather as values that can be assigned to variables just like integers or any other data type.

In Go, we can handle errors in two primary ways: **panic**, which will crash the application, or **gracefully**, which allows us to log the error and return the error value without disrupting the application’s normal flow.

In Go, the error type is represented by an interface that contains a single method: **Error**(). Any value that implements this method is considered an error. The **Error**() method simply returns a string that represents the error message. Example:

type error interface {  
    Error() string  
}

In Go, errors can be created on the fly using the built-in errors or fmt packages. For instance, if you want to return a new error with a static error message, you can use the errors package’s New() function, as shown in the example below:

import "errors"  
  
func DoSomething() error {  
    return errors.New("something didn't work")  
}

## Wrapping Errors

Wrapping errors becomes particularly important when dealing with more complex programs that involve multiple layers of functions. In the previous examples, we focused on errors that were created, returned, and handled within a single function call. However, in real-world scenarios, errors may traverse through a stack of functions, starting from where they are generated to where they are eventually handled, with potentially several intermediate functions in between.

To address this, Go 1.13 introduced new error APIs, including **errors.Wrap** and **errors.Unwrap**, which offer powerful capabilities for adding contextual information to errors as they propagate through the call stack. With **errors.Wrap**, we can easily wrap an existing error with additional context, allowing us to provide more details about the error's origin or the specific conditions that led to it.