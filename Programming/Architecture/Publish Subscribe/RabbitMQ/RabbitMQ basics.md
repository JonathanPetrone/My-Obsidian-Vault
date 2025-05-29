In RabbitMQ, an [exchange](https://www.rabbitmq.com/tutorials/amqp-concepts#exchanges) is where publishers send messages, typically with a routing key.

The exchange takes the message, uses the routing key as a filter, and sends the message to any queues that are listening for that routing key.

![[Pasted image 20250323130851.png]]
#### Terminology:
- [Exchange](https://www.rabbitmq.com/tutorials/amqp-concepts#exchanges): A routing agent that sends messages to queues. 
	- [[Exchanges]] notes
- [Binding](https://www.rabbitmq.com/tutorials/amqp-concepts#bindings): A link between an exchange and a queue that uses a **routing key** to decide which messages go to the queue.
- [Queue](https://www.rabbitmq.com/tutorials/amqp-concepts#queues): A buffer in the RabbitMQ server that holds messages until they are consumed.
- [Channel](https://www.rabbitmq.com/tutorials/amqp-concepts#amqp-channels): A virtual connection inside a connection that allows you to create queues, exchanges, and publish messages.
- [Connection](https://www.rabbitmq.com/tutorials/amqp-concepts#amqp-connections): A [TCP](https://en.wikipedia.org/wiki/Transmission_Control_Protocol) connection to the RabbitMQ server.