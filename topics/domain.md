# Domain Driven Design

Domain-Driven Design (DDD) is a software development approach that centres on a structure and language of the code that matches that of the business domain. It's about creating a lanuage shared by domain experts and technical experts. So the words used by the domain expert are the same as those in the code. The shared language are then use to build a rich model of the domain describing the objects and interactions between the objects. (see [DDD Building Blocks](https://www.youtube.com/watch?v=xFl-QQZJFTA)).

## Core Elements of a Domain Model

The following are core modeling elements to construct a domain model:

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
* **Example:** An Order aggregate might include the Order entity (the Aggregate Root), a list of LineItem Value Objects, and an Address Value Object. The LineItems can't be added or removed directly; you must do it through the Order entity. This ensures that the total price of the order is always correctly calculated.

### Bounded Context

A Bounded Context is a logical boundary that defines the scope of a particular model. It's a key concept for managing complexity.

* **Why it's needed:** In a large application, the word "product" might mean something different to the inventory team than it does to the marketing team. For the inventory team, "product" might include weight and dimensions. For the marketing team, it might include a description and images. A single, monolithic model for "product" would be a mess.
* **How it works:** You define a separate Bounded Context for each of these different meanings. Within each context, the Ubiquitous Language is consistent, but it's okay for the same term to have different meanings in different contexts.
* **Example:** You might have an Order Bounded Context, a Shipping Bounded Context, and a Billing Bounded Context. The "order" object in the Shipping context would only contain the information relevant to shipping, such as the address and tracking number.

### Entities

An Entity is an object that has a unique identity and a lifecycle. It's defined by its identity, not by its attributes.

* **Key characteristic:** Even if all of an entity's attributes change, it's still the same entity if its unique ID remains the same.
* **Example:** A Customer is an entity. A customer's name, address, and phone number can all change, but it's still the same person with the same unique customerId.

### The Ubiquitous Language

At the heart of DDD is the Ubiquitous Language. This is a shared, common language that is used by everyone on the team—developers, domain experts, and business stakeholders. It's not just a list of terms; it's a living vocabulary that describes the domain.

* **Why it's crucial:** The Ubiquitous Language eliminates misunderstandings. When a business person says "customer," everyone knows exactly what that means. If a developer uses a different term like "user account," it creates confusion.
* **Where it lives:** This language should be reflected directly in your code. Class names, method names, and variable names should all use the terms from the Ubiquitous Language.

### Value Objects

A Value Object is an object that has no conceptual identity. It's defined by its attributes.

* **Key characteristic:** Two Value Objects are considered equal if all of their attributes are the same. They are immutable, meaning you don't change them; you replace them.
* **Example:** A Money object with a currency and an amount. A Money object representing $5.00 is identical to any other Money object representing $5.00. You don't update a Money object; you replace it with a new one.

### Subdomains

Subdomains are a strategic concept that helps manage a large, complex business domain. The entire business domain is often too big to be modeled as a single, coherent unit. You break it down into smaller, more manageable pieces, which are the Subdomains.

* **How it relates to Bounded Contexts:** A Bounded Context is a technical implementation of a subdomain. A single subdomain may contain one or more bounded contexts, and each bounded context will have its own unique model and language. The Bounded Context is where the software is actually built, while the Subdomain is a business-level concept.
* **Example:** An e-commerce business might have a Sales subdomain, a Fulfillment subdomain, and a Customer Support subdomain. Each of these will have its own model and language, and each will likely correspond to one or more Bounded Contexts in the software.

### Domain Events

A Domain Event is something that happened in the past that is significant to the business. It's a key part of building a decoupled and reactive system. They are immutable and represent a change of state within a single Aggregate.

* **How it relates to Aggregates:** An aggregate will publish a domain event when a significant change occurs. For example, an Order aggregate might publish an OrderPlacedEvent or OrderShippedEvent. These events can then be consumed by other parts of the system (in different Bounded Contexts) to trigger new actions without tight coupling.
* **Example:** When the Order aggregate is successfully placed, it raises an OrderPlaced domain event. The Shipping Bounded Context can subscribe to this event and begin the process of preparing a shipment. The Billing Bounded Context can also subscribe to the same event to generate an invoice. This loose coupling is a major benefit of DDD.

### Repository

A Repository is a pattern that provides a layer of abstraction for accessing aggregates. It acts as a bridge between the domain model and the data persistence layer (like a database).

* **How it relates to Aggregates:** You don't directly query the database for individual entities. Instead, you use a repository to retrieve entire aggregates. This ensures that the aggregate's consistency rules are maintained, as you always load the aggregate root and all its contained objects together. The repository is responsible for persisting and retrieving the full aggregate.
* **Example:** Instead of writing a database query to find a customer, you'd use a CustomerRepository. The code would look something like customer_repository.get_by_id(customer_id). The repository handles the details of how that customer data is fetched from the database and reconstructed into a Customer aggregate.

## Strategic vs Tactical Design

* **Strategic Design** is the "big picture" view. It's about how you organize a large, complex domain and its teams.
* **Tactical Design** is the "in the weeds" view. It's about the specific patterns you use to build a high-quality domain model within a single part of the system.

### Strategic Design: The "Big Picture"

Strategic design is about managing the complexity of an entire enterprise. It's where you start, before you write a single line of code. The goal is to define the boundaries of your problem space.

Core Concepts of Strategic Design:

* **Subdomains:** You identify the different business areas within your overall domain. For an e-commerce company, this might include Sales, Fulfillment, Billing, and Customer Support.
* **Bounded Contexts:** You define the technical and linguistic boundaries for each subdomain. A Bounded Context is a specific software model where a particular ubiquitous language is consistently applied. This is where you acknowledge that "Product" in the Inventory context is different from "Product" in the Marketing context.
* **Context Maps:** You visualize the relationships between your Bounded Contexts. A Context Map shows how the different parts of your system integrate and communicate. This helps you understand dependencies and make informed decisions about your architecture.

Think of Strategic Design as:

* A city planner's map that shows the different districts of a city (residential, commercial, industrial).
* The work of a high-level architect who decides where the different wings of a building will go and how they will connect.
* The work of a general in a war room, planning the overall campaign and where each division will be deployed.

### Tactical Design: The "In the Weeds"

Tactical design is what you do inside a single Bounded Context. Once you've defined the boundaries, this is where you apply a set of design patterns to build a robust, expressive, and maintainable domain model.

Core Concepts of Tactical Design:

* **Entities:** Objects with a unique identity (e.g., a Customer with a customer_id).
* **Value Objects:** Objects defined by their attributes, with no unique identity (e.g., a Money object with an amount and currency).
* **Aggregates:** Clusters of related entities and value objects that are treated as a single unit to enforce consistency. They have a single entry point called the Aggregate Root (e.g., an Order aggregate with LineItem value objects).
* **Repositories:** Abstractions that provide a way to retrieve and store entire aggregates. They hide the details of the data persistence layer.
* **Domain Events:** Events that represent a significant occurrence within an aggregate that other parts of the system might need to know about (e.g., OrderShipped).

Think of Tactical Design as:

* A blueprint for a single building in the city, detailing the layout of the rooms, the plumbing, and the electrical wiring.
* The work of a construction crew, building a specific part of the building according to the detailed plans.
* A squad leader, focusing on a specific objective and using the right tools and tactics to achieve it.

### Analogy

* **Strategic Design:** You decide that your e-commerce platform needs a Catalog Bounded Context, an Ordering Bounded Context, a Shipping Bounded Context, and a Payment Bounded Context. You map out how these contexts will communicate—for instance, the Ordering context will publish an OrderPlaced event that the Shipping and Payment contexts will subscribe to.
* **Tactical Design:** You zoom in on the Ordering Bounded Context. You define the Order aggregate with the Order entity as its root, a collection of LineItem value objects, and an Address value object. You create an OrderRepository to save and retrieve Order aggregates. The Order aggregate has a method called add_item(product_id, quantity) that ensures all business rules for adding an item are respected.

## Identifying Core Domain

Here are the steps to help identify the core domain artefacts:

* **STEP 1:** Understand the Business Goals and Value Propositions:
  * Why does this business exist? What problems does it solve? What value does it provide to its customers?
  * Focus on the activities that directly contribute to the core value proposition. These are often where the most critical domain concepts reside.
  * Engage with stakeholders to understand their perspectives on what is most important for the business's success.
* **STEP 2** - Conduct Domain Expert Interviews and Workshops:
  * Engage the people who understand the business. These people are your domain experts. They might be business analysts, subject matter experts, or experienced users.
  * Ask open-ended questions about their daily work, the processes they follow, the data they interact with, and the challenges they face.
  * Focus on the "what" and "why" of their activities, not just the "how" of current systems.
  * Identify key business processes and workflows. Map out the steps involved in achieving business goals.
* **STEP 3:** Analyse Existing Documentation and Systems:
  * Review existing business documents, process flows, system specifications, and legacy systems.
  * Look for recurring nouns (potential entities and value objects) and verbs (potential domain services and operations).
  * Be critical of existing systems – they might not perfectly reflect the ideal domain model.
* **STEP 4:** Identify Key Domain Concepts and Terms:
  * As you gather information, start listing the important nouns and concepts frequently used by domain experts.
  * Pay attention to the ubiquitous language – the shared vocabulary of the business.
  * Define these terms clearly and ensure everyone on the team has a common understanding.
* **STEP 5:** Model Core Business Processes:
  * Focus on the processes central to the business's value proposition.
  * Use visual tools like flowcharts or activity diagrams to represent these processes.
  * Identify the entities that interact with each other.
* **STEP 6:** Distinguish Entities, Value Objects, and Services:
  * **Entities:** Identify objects with a unique identity and a lifecycle. Focus on things that need to be tracked and change over time (e.g., Customer, Order, Product).
  * **Value Objects:** Identify objects defined by their attributes and do not have a unique identity. They are immutable and represent descriptive aspects (e.g., Address, Colour, Money).
  * **Domain Services:** Identify operations or behaviours that don't naturally belong to an entity or value object but are still crucial to the domain logic (e.g., OrderPlacementService, ShippingCalculatorService).
* **STEP 7:** Identify Aggregates and Aggregate Roots:
  * Group related entities and value objects into aggregates.
  * Choose one entity within each aggregate to be the aggregate root, which controls access and ensures consistency within the aggregate boundary: this helps manage complexity and maintain data integrity.
* **STEP 8:** Define Bounded Contexts (if applicable):
  * For larger, more complex domains, identify the boundaries within which a particular domain model and ubiquitous language are consistent.
  * Recognise that the same business concept might have different meanings or behaviours.
* **STEP 9:** Iterate and Refine:
  * Domain modelling is an iterative process. Your initial understanding might not be perfect.
  * Continuously review and refine your model based on feedback from domain experts and as your understanding evolves.
  * Use visual models (like UML class diagrams or simple sketches) to communicate and validate your understanding.

## Supporting Techniques

These techniques are supportive of the DDD process:

* [Semantic Data Modelling](./semantic.md)
* [Generic Entity Modelling](./generic.md)
* [UML Notations](./uml.md)

## Working Examples

* [Person Profile](../domains/person-profile.md)

## References

* [What is DDD - Eric Evans - DDD Europe 2019](https://www.youtube.com/watch?v=pMuiVlnGqjk)
