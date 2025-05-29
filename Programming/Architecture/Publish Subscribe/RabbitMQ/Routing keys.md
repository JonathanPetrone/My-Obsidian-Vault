Routing keys are one of the most powerful features of RabbitMQ. They allow the message broker to flexibly route messages to queues based on pattern matching, rather than exact matches.

[Routing keys](https://www.rabbitmq.com/tutorials/tutorial-five-python#topic-exchange) in RabbitMQ are made up of words separated by dots. For example, the routing key `user.created` is made up of two words: `user` and `created`. The routing key `peril.game.won` is made up of three words: `peril`, `game`, and `won`.

## Wildcards
RabbitMQ supports two types of wildcards in routing keys:
- `*` (star) substitutes for exactly one word
- `#` (hash) substitutes for zero or more words

## Examples
A queue bound to the key `peril.#` will pick up messages published to routing keys:
- `peril.game.won`
- `peril.game.lost`
- `peril.player`
- `peril`

