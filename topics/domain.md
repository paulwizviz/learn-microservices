# Domain Driven Design

Domain-Driven Design (DDD) is a software development approach that centres the development process on a deep understanding of the business domain.

The core idea behind DDD is to create a domain model conceptually representative of the business domain. This model acts as a shared language and understanding between the business and technical sides, influencing the design and structure of the software.

## Key Concepts

* **Domain**: The specific subject area or business context the software addresses (e.g., finance, healthcare, e-commerce).
* **Subdomains**: Larger domains are often divided into smaller, more manageable subdomains, each focusing on a specific part of the business. These are known as Core, Supporting, or Generic subdomains.
* **Bounded Context**: A specific boundary within the domain where a particular model and language are consistent and applicable. Different bounded contexts can have different interpretations of the same terms.
* **Entities**: Domain objects with a unique identity that persists over time, regardless of changes to their attributes (e.g., a Customer, an Order).
* **Value Objects**: Domain objects are defined by their attributes rather than their unique identity. They are immutable and represent descriptive aspects of the domain (e.g., an Address, a Colour).
* **Aggregates**: Clusters of related entities and value objects treated as a single unit for data changes to ensure consistency. An aggregate has a root entity that controls access to the other objects.
* **Domain Events**: Significant occurrences within the domain that the business cares about. These events can trigger actions within the system or in other bounded contexts.

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
  * Identifying Common Patterns and Structures: Generic entity modelling aims to identify reusable patterns and structures applicable across different parts of the domain or multiple domains.
  * Improving Model Flexibility and Extensibility: By identifying generic entity types and relationships, you can create more flexible models adaptable to future changes and evolving business needs without requiring significant rework.
  * Accelerating Initial Model Creation: Generic models or metamodels can provide a starting point for domain modelling, offering common entity types and specialised relationships for the specific domain.
  * Enhancing Data Consistency and Integrity: Applying consistent modelling patterns based on generic entity concepts can help ensure better data consistency and integrity across the system.
  * Supporting the Identification of Technical Services: Generic entity models can sometimes highlight opportunities for creating reusable technical services that operate on similar data structures.

## References

* [What is DDD - Eric Evans - DDD Europe 2019](https://www.youtube.com/watch?v=pMuiVlnGqjk)
