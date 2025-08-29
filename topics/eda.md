# Event Driven Architecture

Event-Driven Architecture (EDA) is a software architecture pattern centered around the concept of events. An event represents a significant change in state or a notable occurrence within a system. In an EDA, components of the system react to these events as they happen, rather than relying on direct, synchronous requests.

## Key Features

* **Events**: A record of something that has happened. Events are typically immutable and contain data about the occurrence. Examples include "user logged in," "product added to cart," "temperature reading updated."
* **Event Producers**: Services or components that emit or publish events when a state change occurs or an action is taken. The producer is generally unaware of which (or if any) consumers will receive the event.
* **Event Consumers**: Services or components that subscribe to or listen for specific events. When a relevant event is received, the consumer performs an action in response. Consumers are generally unaware of the event producers.
* **Event Bus/Broker**: A central infrastructure component (like Kafka, RabbitMQ, or cloud-based event services) that facilitates the routing and delivery of events from producers to consumers. It acts as a mediator, decoupling producers and consumers.
* **Channels/Topics/Queues**: Mechanisms within the event bus that allow consumers to subscribe to specific streams or categories of events.

## Characteristics

* **Asynchronous Communication**: Producers and consumers don't directly interact and don't wait for a response from each other. Communication happens asynchronously through events.
* **Loose Coupling**: Services are highly decoupled because producers don't need to know about consumers, and vice versa. They interact indirectly through the event bus.
* **Scalability**: Individual services can be scaled independently based on their event processing needs.
* **Resilience**: If a consumer is temporarily unavailable, events can often be queued by the event bus and processed later, improving system resilience.
* **Real-time Processing**: EDA is well-suited for applications that need to react to events in near real-time.
* **Flexibility and Extensibility**: New services can be added to consume existing events without requiring changes to the producers.
* **Improved Auditability**: Events provide a clear history of what has happened in the system.

## Two Main Patterns

* **Publish/Subscribe (Pub/Sub)**: Producers publish events to a topic or channel, and multiple consumers can subscribe to that topic and receive all events published to it.
* **Event Streaming**: Events are treated as a continuous stream of data. Consumers can subscribe to and process this stream in real-time or replay historical events.

## Supporting Technologies

* [NATS](https://github.com/paulwizviz/learn-nats)
* [Kafka](https://github.com/paulwizviz/learn-kafka)

## References

* [The Many Meanings of Event-Driven Architecture by Martin Fowler](https://www.youtube.com/watch?v=STKCRSUsyP0)
* [Creating event-driven microservices: the why, how and what by Andrew Schofield](https://www.youtube.com/watch?v=ksRCq0BJef8)
