
Publish–subscribe architecture or pub/sub is a messaging pattern where *publishers* categorize messages into classes that are received by *subscribers*.

*Subscribers* express interest in one or more classes and only receive messages that are of interest, without knowledge of which *publishers*, if any, there are.

This is contrasted to the typical messaging pattern model where publishers send messages directly to subscribers.

Pub/Sub systems are often used to enable "event-driven design", or "event-driven architecture". An event-driven architecture uses events to trigger and communicate between decoupled systems.

![[Skärmavbild 2025-03-23 kl. 12.51.22.png]]

#### Different terminology for Publisher and Subscriber
- Publisher = Producer = Sender
- Subscriber = Consumer = Receiver

### The Broker
In a Pub/Sub (Publish-Subscribe) architecture, the *broker* (also called a message broker or message bus) is **responsible for receiving messages from publishers and delivering them to subscribers.**

Examples of brokers: **Kafka, RabbitMQ, Redis Pub/Sub, Google Pub/Sub, AWS SNS/SQS**.

To read more about details regarding brokers: [[Brokers]]