In an asynchronous system like RabbitMQ, the sender and receiver are decoupled. The sender doesn't need to know if the message was successfully delivered to the receiver. That has benefits, like simplicity and performance, but it also means that the chance of bugs increases.

To address this, it's common in PubSub systems to aggregate messages that fail to be processed into a [dead letter queue](https://www.rabbitmq.com/dlx.html). Queues can be configured to send messages that fail to be processed to a dead letter exchange, which then routes the message to a dead letter queue.

![[Pasted image 20250323134842.png]]

### Ack and Nack
So how does a consumer tell the message broker that an individual message succeeded or failed to be processed? When a consumer receives a message, it must [acknowledge](https://www.rabbitmq.com/confirms.html) it.

If the subscriber crashes or fails to process the message, the message broker can just re-queue the message to be processed again, or discard it (perhaps to a dead-letter queue).

#### Nack
"Ack" is short for "acknowledge", and "Nack" is short for "negative acknowledge". There are really 3 options for acknowledging a message:

1. **Acknowledge**: Processed successfully.
2. **Nack** and requeue: Not processed successfully, but should be requeued on the same queue to be processed again (retry).
3. **Nack** and discard: Not processed successfully, and should be discarded (to a dead-letter queue if configured or just deleted entirely).

#### Exact Delivery
Delivering messages is hard. When you architect a system, you need to decide what guarantees to make. The three main types are:

1. **At-least-once delivery**: If the message broker isn't sure the consumer received the message, it's retried.
2. **At-most-once delivery**: If the message broker isn't sure the consumer received the message, it's discarded.
3. **Exactly-once delivery**: The message is guaranteed to be delivered once and only once.

![[Skärmavbild 2025-03-23 kl. 13.50.31.png]]