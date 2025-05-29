RabbitMQ and most other message brokers are [distributed systems](https://en.wikipedia.org/wiki/Distributed_computing). Our local RabbitMQ server is just a single node, but in production, you'd likely have an entire cluster of nodes. Some advantages of a large cluster include:

1. **High Availability**: If one node goes down, other nodes can take over.
2. **Scalability**: You're not constrained by the resources of a single machine.
3. **Redundancy**: If one node goes down, the messages aren't lost.
#### Resources
1. **CPU**: Faster nodes (more [cores](https://en.wikipedia.org/wiki/Multi-core_processor), higher [clock speed](https://en.wikipedia.org/wiki/Clock_rate)) and more nodes can both help.
2. **Memory**: More [RAM](https://en.wikipedia.org/wiki/Random-access_memory) per node and more nodes can both help.
3. **Disk**: More disk space per node and more nodes can both help.
4. **Network Bandwidth**: In a cloud setting, bandwidth is usually provisioned in proportion to a node's size.

I've found (boot.dev) that using a cluster of 3 nodes is a solid starting point for most production applications, even if you're processing thousands of messages per second. I've also found that when you find your nodes starting to hit limits on CPU, RAM, or Disk, it's generally better to scale vertically first (more powerful nodes) before you go crazy horizontally (larger number of nodes).

More nodes mean more resources, but it also means more management overhead and complexity.

#### Prefetch
When you run a consumer, you may have assumed this process for message consumption:

1. Fetch a message from the queue (across the network, which can be slow)
2. Process the message
3. Acknowledge the message
4. Repeat

But that would slow everything down to a crawl due to the full network round trip for every message. Instead, RabbitMQ allows you to prefetch messages. When you [prefetch](https://www.rabbitmq.com/consumer-prefetch.html) messages, RabbitMQ will send you a batch of messages at once, the client library will store them in memory, and you can process them one by one. _Much faster_. The diagram shows 3 consumers each prefetching batches of 2.

![[Pasted image 20250323153118.png]]