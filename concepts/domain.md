# Domain Driven Design: Key Concepts

Domain-Driven Design (DDD) is a software development approach that centres on a structure and language of the code that matches the business domain. It’s about creating a shared language between domain and technical experts, so the words used by the domain expert are the same as those in the code. The shared language is then used to build a rich model of the domain, describing the objects and the interactions between them. (see [DDD Building Blocks](https://www.youtube.com/watch?v=xFl-QQZJFTA)).

## Core Elements of a Domain Model

A **model** is a simplified representation of a real-world object, system, or concept. Its purpose is to help us understand, analyze, or predict the behavior of something that is too complex, large, or abstract to be studied directly.

A toy like LEGO is compose is multiple components: bricks, plates, tiles, etc. The components could be assembled to resemble a real world thing like a house. Each component could represent a component of a real world house. Likewise with DDD, there are a number of components that could be use to model a business domain, and these are:

* [Aggregates](#aggregates)
* [Bounded Context](#bounded-context)
* [Domain Events](#domain-events)
* [Entities](#entities)
* [Repository](#repository)
* [Subdmains](#subdomains)
* [Ubiquitous Language](#the-ubiquitous-language)
* [Value Objects](#value-objects)

### Aggregates

An Aggregate is a cluster of objects (Entities and Value Objects) that are treated as a single unit. It has a single public-facing entry point called the Aggregate Root.

* **The purpose:** Aggregates enforce business rules and maintain consistency. All changes to the objects within an aggregate must go through the Aggregate Root. This prevents other parts of the system from creating inconsistent states.
* **Example:** An Order aggregate might include the Order entity (the Aggregate Root), a list of LineItem Value Objects, and an Address Value Object. The LineItems cannot be added or removed directly; you must do it through the Order entity. This ensures that the total price of the order is always correctly calculated.

### Bounded Context

A Bounded Context is a logical boundary that defines the scope of a particular model. It is a key concept for managing complexity.

* **Why it’s needed:** In a large application, the word “product” might mean something different to the inventory team than it does to the marketing team. For the inventory team, “product” might include weight and dimensions. For the marketing team, it might include a description and images. A single, monolithic model for “product” would be a mess.
* **How it works:** You define a separate Bounded Context for each of these different meanings. Within each context, the Ubiquitous Language is consistent, but it’s acceptable for the same term to have different meanings in different contexts.
* **Example:** You might have an Order Bounded Context, a Shipping Bounded Context, and a Billing Bounded Context. The “order” object in the Shipping context would only contain the information relevant to shipping, such as the address and tracking number.

### Entities

An Entity is an object that has a unique identity and a lifecycle. It is defined by its identity, not by its attributes.

* **Key characteristic:** Even if all of an entity’s attributes change, it remains the same entity if its unique ID stays the same.
* **Example:** A Customer is an entity. A customer’s name, address, and telephone number can all change, but it remains the same person with the same unique customerId.

### The Ubiquitous Language

At the heart of DDD is the Ubiquitous Language. This is a shared, common language that is used by everyone on the team—developers, domain experts, and business stakeholders. It is not just a list of terms; it is a living vocabulary that describes the domain.

* **Why it’s crucial:** The Ubiquitous Language eliminates misunderstandings. When a business person says “customer”, everyone knows exactly what that means. If a developer uses a different term like “user account”, it creates confusion.
* **Where it lives:** This language should be reflected directly in your code. Class names, method names, and variable names should all use the terms from the Ubiquitous Language.

### Value Objects

A Value Object is an object that has no conceptual identity. It is defined by its attributes.

* **Key characteristic:** Two Value Objects are considered equal if all of their attributes are the same. They are immutable, meaning you don’t change them; you replace them.
* **Example:** A Money object with a currency and an amount. A Money object representing £5.00 is identical to any other Money object representing £5.00. You don’t update a Money object; you replace it with a new one.

### Subdomains

Subdomains are a strategic concept that helps manage a large, complex business domain. The entire business domain is often too big to be modelled as a single, coherent unit. You break it down into smaller, more manageable pieces, which are the Subdomains.

* **How it relates to Bounded Contexts:** A Bounded Context is a technical implementation of a subdomain. A single subdomain may contain one or more bounded contexts, and each bounded context will have its own unique model and language. The Bounded Context is where the software is actually built, while the Subdomain is a business-level concept.
* **Example:** An e-commerce business might have a Sales subdomain, a Fulfilment subdomain, and a Customer Support subdomain. Each of these will have its own model and language, and each will likely correspond to one or more Bounded Contexts in the software.

### Domain Events

A Domain Event is something that has happened in the past that is significant to the business. It is a key part of building a decoupled and reactive system. Domain Events are immutable and represent a change of state within a single Aggregate.

* **How it relates to Aggregates:** An aggregate will publish a domain event when a significant change occurs. For example, an Order aggregate might publish an OrderPlacedEvent or OrderShippedEvent. These events can then be consumed by other parts of the system (in different Bounded Contexts) to trigger new actions without tight coupling.
* **Example:** When the Order aggregate is successfully placed, it raises an OrderPlaced domain event. The Shipping Bounded Context can subscribe to this event and begin the process of preparing a shipment. The Billing Bounded Context can also subscribe to the same event to generate an invoice. This loose coupling is a major benefit of DDD.

### Repository

A Repository is a pattern that provides a layer of abstraction for accessing aggregates. It acts as a bridge between the domain model and the data persistence layer (such as a database).

* **How it relates to Aggregates:** You don’t directly query the database for individual entities. Instead, you use a repository to retrieve entire aggregates. This ensures that the aggregate’s consistency rules are maintained, as you always load the Aggregate Root and all its contained objects together. The repository is responsible for persisting and retrieving the full aggregate.
* **Example:** Instead of writing a database query to find a customer, you would use a CustomerRepository. The code would look something like customer_repository.get_by_id(customer_id). The repository handles the details of how that customer data is fetched from the database and reconstructed into a Customer aggregate.

## Strategic vs Tactical Design

* **Strategic Design** is the “big picture” view – how you organise a large, complex domain and its teams.  
* **Tactical Design** is the “detail-level” view – the patterns you apply to build a high-quality domain model within a single part of the system.

### Strategic Design: The “Big Picture”

Strategic Design manages the complexity of an entire enterprise. It starts before a single line of code is written, with the aim of defining clear boundaries for the problem space.  

Core concepts of Strategic Design:

* **Subdomains:** Identify the major business areas within your domain. For an e-commerce company, this might include Sales, Fulfilment, Billing, and Customer Support.  
* **Bounded Contexts:** Define the technical and linguistic boundaries for each subdomain. Within a context, a ubiquitous language is applied consistently. For example, “Product” in Inventory may differ from “Product” in Marketing.  
* **Context Maps:** Visualise the relationships between Bounded Contexts. They reveal dependencies and integration points, supporting informed architectural decisions.  

Think of Strategic Design as a **city planner’s map**: districts are defined, their boundaries are clear, and the connections between them are explicit.

### Tactical Design: The “Detail Work”

Tactical Design focuses within a single Bounded Context. Once boundaries are set, you use design patterns to create a robust, expressive, and maintainable domain model.  

Core concepts of Tactical Design:

* **Entities:** Objects with a unique identity (e.g. a Customer with a `customer_id`).  
* **Value Objects:** Immutable objects defined only by attributes (e.g. a Money object with amount and currency).  
* **Aggregates:** Groups of related entities and value objects treated as a single unit for consistency. Each aggregate has an **Aggregate Root** (e.g. an Order aggregate with LineItem value objects).  -* **Repositories:** Abstractions for storing and retrieving aggregates. They hide persistence details and always operate at the aggregate level.  
* **Domain Events:** Represent significant occurrences within the domain (e.g. `OrderShipped`). Events are named in the past tense and often trigger reactions in other contexts.  

Think of Tactical Design as a **blueprint for a single building**: detailed enough for construction crews to execute, precise enough to enforce structure.

### Analogy

* **Strategic Design:** You decide your e-commerce platform requires a Catalogue Bounded Context, an Ordering Context, a Shipping Context, and a Payment Context. You map how these interact – for instance, the Ordering Context publishes an `OrderPlaced` event consumed by Shipping and Payment.  
* **Tactical Design:** Within the Ordering Context, you define the Order aggregate with the Order entity as root, LineItem value objects, and an Address value object. An `OrderRepository` stores and retrieves orders. The `add_item(product_id, quantity)` method enforces all business rules for adding an item.

## Identifying Core Domain

To identify the core domain artefacts, follow these steps:  

**STEP 1: Understand Business Goals and Engage Experts**  

* Clarify why the business exists and what value it delivers.  
* Focus on activities that directly create this value.  
* Collaborate with domain experts – analysts, subject-matter experts, or experienced users – to uncover critical processes and challenges.  

**STEP 2: Analyse Existing Sources**

* Review documents, workflows, system specifications, and legacy systems.  
* Look for recurring nouns (potential entities and value objects) and verbs (potential domain services).  
* Be critical – existing systems may not reflect the ideal domain.  

**STEP 3: Define Key Concepts and Language**  

* Capture the important terms and ensure they are used consistently.  
* Establish a ubiquitous language across the team.  

**STEP 4: Model Core Processes**  
  
* Focus on processes central to the business’s value proposition.  
* Use flowcharts or activity diagrams to map interactions between entities.  

**STEP 5: Distinguish Entities, Value Objects, and Services**  

* **Entities:** Unique identity and lifecycle (e.g. Customer, Order, Product).  
* **Value Objects:** Immutable, defined by attributes (e.g. Address, Colour, Money).  
* **Domain Services:** Operations that don’t naturally belong to entities or value objects (e.g. `OrderPlacementService`, `ShippingCalculatorService`).  

**STEP 6: Define Aggregates and Roots**  

* Group related entities and value objects into aggregates.  
* Select one entity as the Aggregate Root to enforce consistency.  

**STEP 7: Establish Bounded Contexts (if needed)**  

* For larger domains, set boundaries where a model and its language stay consistent.  
* Recognise that the same concept may differ across contexts.  

**STEP 8: Iterate and Refine**  

* Domain modelling is iterative.  
* Regularly refine your model based on feedback and deeper understanding.  
* Use simple diagrams or UML class models to validate understanding.

## References

* [What is DDD - Eric Evans - DDD Europe 2019](https://www.youtube.com/watch?v=pMuiVlnGqjk)
