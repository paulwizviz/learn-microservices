# Source Code Architecture

* [Simple Layed Architecture](#simple-layerd-architecture)
* [Clean Architecture](#clean-architecture)
* [Hexagon Architecture](#hexagonal-architecture-ports-and-adapters)

## Simple Layerd Architecture

This is the most basic pattern. It organises code into horizontal layers like Presentation, Business Logic, and Data Access.

* **Presentation Layer:** Handles user requests (e.g., controllers).
* **Business/Service Layer:** Contains the core business logic.
* **Data Access Layer:** Manages interaction with the database.

Dependencies flow downwards, but there are no strict rules like the Dependency Inversion Principle. It is simpler to set up but can lead to tight coupling between layers.

The core rule is that dependencies flow downwards. It is simple and effective for many projects, but it can lead to tight coupling because the business logic is aware of the data access layer.

## Clean Architecture

Clean Architecture is a software design philosophy that advocates for a clear separation of concerns, resulting in a more maintainable and scalable codebase. This is achieved by organising the software into concentric layers, each with a distinct set of responsibilities. The fundamental tenet of this architectural pattern is the Dependency Rule, which mandates that all dependencies must flow inwards. This ensures that the core business logic remains decoupled from and independent of external elements such as frameworks, databases, and user interfaces.

### The Layers

* **Entities**: At the core of the architecture, Entities encapsulate the fundamental business rules and data structures of the application. They are the most abstract and high-level concepts, representing the essence of the business domain. A critical characteristic of Entities is their independence; they have no dependencies on any other component within the system. They embody pure business logic, such as object relationships and validation rules.
* **Use Cases (or Interactors)**: This layer orchestrates the flow of data to and from the Entities to execute application-specific business rules. Use Cases define and implement the operations that the system can perform, effectively encapsulating the application's behaviour. While they have knowledge of the Entities, they remain entirely decoupled from the outer layers, such as the database or user interface.
* **Interface Adapters**: This layer acts as a set of translators, converting data between the format required by the Use Cases and the format suitable for external agencies, such as the database or the web. It is composed of several components:
  * **Controllers**: These components receive input from external sources (e.g., a web request or a command-line interface) and translate it into a format that is consumable by the Use Cases.
  * **Presenters**: Conversely, Presenters take the output from the Use Cases and reformat it for presentation to the user, for instance, by preparing it for display in a graphical user interface (GUI) or as a JSON response from an API.
  * **Gateways (or Repositories)**: These are interfaces that define the methods for data persistence and retrieval. They provide an abstraction layer that separates the Use Cases from the underlying data storage mechanisms. The concrete implementations of these interfaces are located in the outermost layer.
* **Frameworks and Drivers**: The outermost layer of the architecture consists of the specific tools and technologies that are used to build the application. This includes web frameworks, database management systems, user interface frameworks, and other external libraries. This layer contains the implementation details and is the most volatile part of the system. The Dependency Rule is paramount here: this layer depends on the inner layers, but the inner layers have no knowledge of it.

### The Dependency Rule

The Dependency Rule is the cornerstone of Clean Architecture. It stipulates that source code dependencies can only point inwards. Nothing in an inner layer can know anything at all about something in an outer layer. In particular, the name of something declared in an outer layer must not be mentioned by the code in an inner layer.

### The Rationale Behind Clean Architecture

The principles underpinning Clean Architecture are not arbitrary; they are a direct response to prevalent challenges in the field of software engineering. The adoption of this architectural pattern yields several substantial benefits:

* **Maintainability**: A clear separation of concerns renders the codebase more comprehensible and amenable to modification. Alterations to peripheral components, such as the database or user interface, have a negligible impact on the core business logic. This architectural decoupling mitigates the risk of introducing defects and enhances the system's resilience to change.
* **Testability**: The decoupling of layers intrinsically improves the testability of the code. The core business logic, encapsulated within the Entities and Use Cases, can be tested in complete isolation, without the need for a live database or a graphical user interface. This facilitates the creation of faster, more reliable, and more comprehensive test suites.
* **Flexibility**: Clean Architecture fosters independence from specific frameworks and external services. As the core business logic is oblivious to the outer layers, developers can readily substitute technologies. For instance, one could replace the database, web framework, or user interface technology without necessitating a rewrite of the fundamental business rules.
* **Developer Productivity**: A well-organised codebase is more straightforward for developers to navigate and comprehend. This is particularly advantageous for new team members, who can become productive more rapidly. The consistent structure also simplifies the process of locating and rectifying defects, thereby increasing the overall productivity of the development team.

## Hexagonal Architecture (Ports and Adapters)

Hexagonal Architecture, also known as the Ports and Adapters pattern, offers a strong alternative to the layered architecture. It organises the application into two main regions: the core application and the external world, connected by a series of ports and adapters. This structure is designed to isolate the core business logic from outside concerns, such as databases, user interfaces, or third-party services.

### The Core Application

At the centre of the hexagon is the core application, which contains the pure business logic. This area is equivalent to the inner layers (Entities and Use Cases) of Clean Architecture.

*   It contains all the business rules and processes, completely independent of any external technology.
*   It defines the interfaces, or **ports**, through which it communicates with the outside world, but it has no knowledge of the concrete implementations of these ports.

### Ports and Adapters

The boundary of the core application is defined by a set of ports, which act as the gateways for all communication.

*   **Ports**: A port is simply an interface that defines a contract for interaction. There are two primary types:
    *   **Driving Ports (Input Ports)**: These expose the core application's API to the outside world. They are called by primary adapters (like a UI or a test script) to trigger application behaviour.
    *   **Driven Ports (Output Ports)**: These are the interfaces the core application *requires* to connect to external services, such as a database or a messaging queue. The core defines the interface (e.g., `OrderRepository`), but the implementation is external.

*   **Adapters**: An adapter is a component that bridges the gap between a port and an external technology.
    *   **Driving Adapters (Primary Adapters)**: These adapters wrap external technologies and use them to call the driving ports. For example, a REST controller is a driving adapter that translates an HTTP request into a method call on a use case in the core.
    *   **Driven Adapters (Secondary Adapters)**: These adapters are concrete implementations of the driven ports. For example, a `PostgresOrderRepository` would be a driven adapter that implements the `OrderRepository` port to persist data in a PostgreSQL database.

### Dependency Flow

Similar to Clean Architecture, Hexagonal Architecture enforces a strict dependency rule: all dependencies must point inwards, towards the core application.

Adapters depend on the ports defined within the core, but the core never depends on the adapters. This inversion of control ensures that the business logic remains pure and decoupled. For instance, you can swap a database adapter for a different one (e.g., from SQL to a NoSQL database) without any changes to the core application, as long as the new adapter implements the same port (interface).

## Relationship with Domain-Driven Design (DDD)

The architectural patterns discussed here are focused on code layout and the naming of components, with the primary intention of reducing cognitive overload for software engineers.

Domain-Driven Design (DDD), in contrast, is used to identify the concepts that make up a business domain. When it comes to naming things in code, engineers should be informed by the ubiquitous language that is developed through the DDD process. This ensures that the names used in the code accurately reflect the business domain.