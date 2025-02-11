A [queue](https://en.wikipedia.org/wiki/Queue_(abstract_data_type)) is a data structure that stores ordered item. It's _like_ a list, but like a stack, its design is more restrictive. A queue only allows items to be _added to the tail_ of the queue and _removed from the head_ of the queue.

![[Pasted image 20241223152717.png]]

A **queue** is a **First-In, First-Out (FIFO)** data structure.

![[Skärmavbild 2024-12-23 kl. 15.30.32.png]]

Just like a stack, it's _all_ O(1)! No matter how many items are in the queue, these operations will always take the same amount of time. The reason to choose a queue over a stack is all about ordering. Queues should be used when you need to process items in the order they were added.

Examples of use cases for queues in programming:
- **Task Scheduling**: Managing tasks in the order they arrive (e.g., CPU scheduling, printing jobs).
- **Breadth-First Search (BFS)**: Traversing graphs or trees level by level.
- **Asynchronous Messaging**: Handling communication between processes (e.g., RabbitMQ, Kafka).
- **Resource Management**: Allocating resources like customer service requests or database queries.
- **Real-Time Data Processing**: Temporarily storing data for real-time consumption (e.g., video streaming, sensor data).
- **Networking**: Queuing data packets or HTTP requests for processing.
- **Simulations**: Simulating sequential events (e.g., airport takeoffs, toll booths).
- **Background Tasks**: Processing tasks like email sending or video uploads asynchronously.