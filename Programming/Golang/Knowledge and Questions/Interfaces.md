
### **What is the most basic thing an interface does?**

In Go, an interface defines **a set of method signatures** that a type ([[Types]]) must implement to "satisfy" the interface. This allows different types to provide their own implementation of these methods and then to be used interchangeably wherever the interface is expected.

---

### **Why is this functionality useful?**

The ability to treat different types interchangeably offers several key benefits:

- **Polymorphism**: Use different types with shared behavior in the same way.
- **Decoupling**: Reduce dependencies, making code more flexible and maintainable.
- **Extensibility**: Add new types or behaviors without changing existing code.
- **Testing**: Create mock objects for isolated, reliable testing.
- **Composition**: Combine behaviors without relying on inheritance.


```
type Shape interface {
    Area() float64
}
```




