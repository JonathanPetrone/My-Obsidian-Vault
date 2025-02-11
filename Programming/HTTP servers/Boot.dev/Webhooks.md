Webhooks sound like a scary advanced concept, but they're quite simple.

A webhook is just an event that's sent to your server by an external service when something happens.

The only real difference between a webhook and a typical `HTTP` request is that the system making the request is an automated system, not a human loading a webpage or web app. As such, webhook handlers must be [idempotent](https://en.wikipedia.org/wiki/Idempotence) because the system on the other side may retry the request multiple times.

Idempotent, or "idempotence", is a fancy word that means "the same result no matter how many times you do it". For example, your typical `POST /api/chirps` (create a chirp) endpoint will _not_ be idempotent. If you send the same request twice, you'll end up with two chirps with the same information but different IDs.

Webhooks, on the other hand, should be idempotent, and it's typically easy to build them this way because the client sends some kind of "event" and usually provides its own unique ID.

A webhook is just an event that's sent to your server by an external service. There are just a couple of things to keep in mind when building a webhook handler:

- The third-party system will probably retry requests multiple times, so your handler should be [idempotent](https://en.wikipedia.org/wiki/Idempotence).
- Be extra careful to never "acknowledge" a webhook request unless you processed it successfully. By sending a `2XX` code, you're telling the third-party system that you processed the request successfully, and they'll stop retrying it.
- When you're writing a server, you typically get to define the API. However, when you're integrating a webhook from a service like Stripe, you'll probably need to adhere to their API: they'll tell you what shape the events will be sent in.

## Are Webhooks and Websockets the Same Thing?

Nope! A websocket is a persistent connection between a client and a server. Websockets are typically used for real-time communication, like chat apps. Webhooks are a one-way communication from a third-party service to your server.

We'll talk about websockets in a future course.
