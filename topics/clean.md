# Clean Architecture

Clean Architecture is a software design pattern that provides a clear and maintainable way to organize a codebase. It achieves this by separating the code into distinct layers, each with a specific responsibility. The core principle of this pattern is the Dependency Rule, which dictates that dependencies should only point inwards, making the core business logic independent of external concerns like frameworks and databases.

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

## The Rationale Behind Clean Architecture

The principles of Clean Architecture are not just arbitrary rules; they are designed to address common challenges in software development. By adopting this pattern, you can achieve several key benefits:

* **Maintainability**: With a clear separation of concerns, the codebase becomes easier to understand and modify. Changes to one part of the system, such as the database or user interface, have a minimal impact on the core business logic. This reduces the risk of introducing bugs and makes the system more resilient to change.
* **Testability**: The decoupling of layers makes the code inherently more testable. The core business logic (Entities and Use Cases) can be tested in isolation, without the need for a running database or a user interface. This leads to faster, more reliable, and more comprehensive tests.
* **Flexibility**: Clean Architecture promotes independence from frameworks and external services. Because the core business logic has no knowledge of the outer layers, you can more easily swap out technologies. For example, you could replace your database, web framework, or UI technology without having to rewrite your business rules.
* **Developer Productivity**: A well-organized codebase is easier for developers to navigate and understand. This is especially beneficial for new team members, who can get up to speed more quickly. The consistent structure also makes it easier to find and fix bugs, leading to increased productivity across the team.

## Clean Architecture vs Domain Driven Design

* Clean Architecture - Focus on the software architecture.
* DDD - Focus on understanding and modelling the business domain.
