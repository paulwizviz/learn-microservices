# Clean Architecture

The Clean Architecture separates concerns by dividing the software into several layers. The key idea is that the inner layers are independent of the outer layers.

## Layers

* **Entities**: They encapsulate the core business rules and data. Entities shouldn't depend on anything else in the system. They are pure business logic (e.g. entity relationships, etc).
* **Use Cases (or Interactors)**: This layer contains the application-specific business rules. They orchestrate the data flowing to and from the entities to achieve a specific business goal. Use cases know about entities but are unaware of the outer layers, such as the database or UI.
* **Interface Adapters**: This is the translator layer between the use cases and the outer layers. It consists of:
  * **Controllers**: They take input from the outer world (like a web request or UI action) and format it in a way that's convenient for the use cases.
  * **Presenters**: They take the data from the use cases and format it for display in the UI.
  * **Gateways (or Repositories)**: These interfaces define how the use cases interact with external systems like databases or web services. The actual implementations of these interfaces reside in the outer layers.
* **Frameworks and Drivers**: This is the outermost layer and contains things like the UI, web frameworks, database systems, and external APIs. This layer is where all the messy details reside. The key is that this layer depends on the inner layers, but the inner layers don't depend on it.

## Dependency Rules

Dependencies should only point inwards. Inner layers should not have any knowledge about the outer layers.

## Clean Architecture vs Domain Driven Design

* Clean Architecture - Focus on the software architecture.
* DDD - Focus on understanding and modelling the business domain.
