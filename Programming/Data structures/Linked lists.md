A linked list is a linear data structure where elements are not stored next to each other in memory. The elements in a linked list are linked using references to each other.

In a linked list, _there are no indexes_. Each node contains the data itself as well as a reference to the next node in the list. Iterating over a linked list requires starting at the head node and following the `next` references until you reach the end.

### Why use linked list?

We use them sometimes because linked lists are much faster to make updates to, _particularly when inserting or deleting items from the middle_. In a normal list, if you insert an item in the middle, you have to shift all the items after it down one spot `(O(n))`:

![[Pasted image 20241223154551.png|400]]
![[Pasted image 20241223154623.png|400]]

The **implementation of linked lists** can vary across programming languages, especially depending on whether the language supports **Object-Oriented Programming (OOP)** or uses other paradigms like **procedural** or **functional** programming. Here's how it differs:

### **1. OOP Languages**

Languages like **Java**, **C++**, or **Python** often rely on defining **node classes** and **list classes** for linked list implementations. Each node contains:

- A reference (or pointer) to the next node.
- The data it holds.

For example:
#### python
```
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

class LinkedList:
    def __init__(self):
        self.head = None
```

- Encapsulation makes the implementation modular.
- Methods like `add`, `remove`, and `search` are typically defined within the `LinkedList` class.

---

### **2. Non-OOP Languages**

Languages like **C** or **Go** (which aren't strictly OOP) implement linked lists procedurally:

- You define a `struct` for the node.
- Functions operate on these nodes rather than having methods tied to a class.

For example, in C:
#### C
```
struct Node {
    int data;
    struct Node* next;
};
```

- Functions like `insert`, `delete`, or `search` manipulate the list directly.

---

### **3. Functional Programming Languages**

Languages like **Haskell** or **Erlang** tend to avoid explicit pointer manipulation. Instead:

- Linked lists are often built into the language as recursive data structures.
- Operations like adding or removing are done immutably (creating a new list rather than modifying the original).

Example in Haskell:
#### Haskell
```
data LinkedList a = Empty | Node a (LinkedList a)
```


---

### **4. Built-In Support**

Some languages provide **native linked list implementations** (e.g., Python's `collections.deque` or Java’s `LinkedList` class). In these cases, you rarely need to implement linked lists manually.

---

## **Use cases**
Linked lists are not as commonly used as arrays or other data structures in modern programming, but they remain highly relevant in specific scenarios. 

Here's when and why linked lists are used:

### **When Are Linked Lists Used?**

1. **Dynamic Memory Allocation**
    
    - Linked lists are used when the number of elements is unknown or changes frequently.
    - Unlike arrays, they don't require resizing or reallocation because they grow dynamically.
2. **Efficient Insertions and Deletions**
    
    - Linked lists allow O(1) insertions and deletions at the beginning or middle of the list, compared to O(n) for arrays.
    - Useful for applications where elements are frequently added or removed (e.g., undo functionality).
3. **Memory Efficiency for Sparse Data**
    
    - Linked lists are preferred when memory usage needs to be minimized for sparse data (e.g., adjacency lists in graph representation).
4. **Data Structures Built on Linked Lists**
    
    - Linked lists form the basis of other data structures:
        - **Stacks**: Using a singly linked list.
        - **Queues**: Using a singly or doubly linked list.
        - **Hash tables**: Chaining to resolve collisions.
5. **Low-Level Systems Programming**
    
    - Linked lists are used in systems programming (e.g., operating systems, embedded systems) for tasks like managing memory blocks (free lists) or process control blocks (PCB).
6. **When Contiguous Memory Isn't Practical**
    
    - Linked lists don't require contiguous memory, making them ideal for environments with fragmented memory.

---

### **What Are Linked Lists Used For?**

- **Undo/Redo Functionality**: Maintaining a history of actions in applications like text editors.
- **Dynamic Data Storage**: Maintaining lists that grow and shrink (e.g., playlists, dynamic tables).
- **Graph Representations**: Representing adjacency lists in graphs.
- **Memory Management**: Managing free memory blocks in custom memory allocators.
- **Polynomial Arithmetic**: Representing polynomials where coefficients and exponents can be stored in nodes.

---

### **Are They Used Often?**

- **In Practice**: Linked lists are less common in high-level application code, where arrays, dynamic arrays (e.g., Python lists or Java `ArrayList`), or built-in libraries are used.
- **In System Design**: They're more common in low-level or specialized scenarios, where their specific advantages (e.g., dynamic growth, efficient insertion/deletion) are required.