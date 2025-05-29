Queues are where the messages are stored after being routed through the exchange. Messages sit in a queue until they are consumed by a subscriber.

A queue can have 0, 1, or many consumers.

![[Pasted image 20250323131913.png]]

- If a queue has no consumers, messages will accumulate in the queue and never be processed.
- If a queue has one consumer, that consumer will process all messages in the queue (assuming it can keep up).
- If a queue has many consumers, messages will be distributed between them in a round-robin fashion (unless you set a [priority](https://www.rabbitmq.com/docs/consumers#priority)).

### Durability
Queues can be ["durable"](https://www.rabbitmq.com/docs/queues#durability) or "transient". Durable queues survive a RabbitMQ server restart, while transient queues do not.

The metadata of a durable queue is stored on disk, while transient queues are only stored in memory.

### Transient Queues (Volatile)

- **Not persisted to disk**—messages are stored in memory only.
- **Lost if the broker restarts** (e.g., if RabbitMQ crashes, the queue is gone).
- **Faster** than durable queues because there’s no disk I/O.
- Suitable for **temporary** or **real-time processing** where message loss is acceptable.

✅ **Use case:** Live chat, real-time logs, temporary event streams.

---
### Durable Queues (Persistent)

- **Persisted to disk**, meaning they **survive broker restarts**.
- Messages remain **until they are consumed** or expire.
- **Slower** due to disk writes but ensures reliability.
- Requires both:
    - The **queue to be marked durable**.
    - The **messages to be marked persistent** (if using RabbitMQ with AMQP).

✅ **Use case:** Order processing, financial transactions, email queues—where messages **must not be lost**.

### Queue types
Generally speaking, there are 2 queue types to worry about:

1. [Classic queues](https://www.rabbitmq.com/docs/classic-queues) (we've been using these)
2. [Quorum queues](https://www.rabbitmq.com/docs/quorum-queues)

Classic queues are the default and are great for most use cases. They are fast and simple. However, they have a single point of failure: the node that the queue is on. If that node goes down, the queue is lost.

You might be thinking, "Wait! You told me Rabbit is a distributed system!" And you're right, _Rabbit_ is distributed, but classic queues are not. They are stored on a single node. If that node goes down, the queue is lost, at least until the node comes back online.

Quorum queues are designed to be more resilient. They are stored on multiple nodes, so if one node goes down, the queue is still available. The tradeoff is that because quorum queues are stored on multiple nodes, they are slower than classic queues.

![[Pasted image 20250323153235.png]]
_As a general rule, I use classic queues for my ephemeral queues (transient, auto-delete, etc). I use quorum queues for most of my durable queues._