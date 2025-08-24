# Semantic Data Modelling

Semantic Data Modelling refers to a method of organising data that focuses on capturing the meaning of data elements and the relationships between them, rather than just their structure or format. It's about understanding what the data means in the real world or within a specific business context.

## Key Concepts

* **Enhanced Understanding of the Ubiquitous Language:** Semantic data modelling focuses on the meaning of data and the relationships between concepts. It can significantly contribute to establishing a clear and consistent ubiquitous language in DDD by explicitly defining terms and their semantic connections. It reduces ambiguity and ensures everyone shares a common understanding of the domain concepts.
* **Identifying Core Domain Concepts and Relationships:** Semantic models, like ontologies or semantic networks, can help in formally identifying the key entities, their attributes, and the various relationships that exist within the domain. It can provide a solid foundation for identifying DDD aggregates, entities, and value objects.
* **Improving Communication with Domain Experts:** Semantic models can act as a visual and structured way to communicate complex domain knowledge with non-technical experts. The explicit representation of concepts and their relationships can facilitate better understanding and validation of the domain model.
* **Facilitating Bounded Context Identification:** By analysing the semantic coherence of different parts of the domain, semantic modelling can help identify natural boundaries and potential bounded contexts within a bigger system. Areas with distinct sets of concepts and relationships might suggest different bounded contexts.
* **Supporting Data Integration and Interoperability:** In systems that need to interact with other systems or data sources, a semantically rich domain model can facilitate better data integration and interoperability by providing a common understanding of the underlying data structures and their meanings.

## Steps in Semantic Data Modelling

Semantic data modelling is a process of progressive refinement, moving from a high-level sketch to a more precise and meaningful model. Here’s a typical workflow:

1. **Produce a Rough-Cut Model:** Start by creating an initial model of the entities and relationships using everyday language, based on your initial understanding of the domain. For example, you might start with simple concepts like `Author` and `Book` and a relationship like "writes".
2. **Establish the Semantics and Refine the Model:** This is the core of the process. Critically examine the terms used in your initial model and refine them to be more semantically accurate. This is where you would distinguish between a core entity and the role it plays. For example, you might realize that `Author` is a role played by a `Person`, and you would update your model to reflect this deeper understanding.
3. **Define Relationships and Cardinality:** Once your entities are well-defined, formalize the relationships between them. This includes giving the relationships clear, descriptive names and defining their cardinality (e.g., one-to-one, one-to-many, many-to-many).
4. **Choose a Modelling Technique:** Select a suitable technique to represent your model. This could be a visual notation like UML, a formal language like RDF/OWL, or a simpler, more informal approach like the Subject-Predicate-Object triples shown in the example.
5. **Iterate and Refine:** Semantic modelling is not a one-time task. It is an iterative process of continuous learning and refinement. As you gain new insights into the domain, you should revisit and improve your model in collaboration with domain experts.

## A Simplified Example: Modelling the World of Books

Let's walk through the process of creating a semantic model for some basic information about books, following the steps outlined above.

### Step 1: Produce a Rough-Cut Model

Our initial, high-level understanding of the domain might look like this:

* **Entities:** `Author`, `Book`
* **Relationship:** An `Author` *writes* a `Book`.

This is a perfectly fine starting point. It captures the most obvious concepts and their relationship.

### Step 2: Establish the Semantics and Refine the Model

Now, we critically examine our initial model. Is "Author" a fundamental entity, or is it a role? A person can be an author, but they are fundamentally a `Person`. This distinction is crucial for a rich semantic model.

So, we refine our model:

* **Core Entity:** `Person`
* **Role:** `Author` (a role that a `Person` can play)
* **Entity:** `Book`
* **Refined Relationship:** A `Person` (in the role of an `Author`) *writes* a `Book`.

### Step 3: Define Relationships and Cardinality

Now we formalize the relationships:

* A `Person` can write one or more `Book`s (`1..*`).
* A `Book` can be written by one or more `Person`s (to allow for co-authorship) (`1..*`).

### Step 4: Choose a Modelling Technique

For this example, we'll use the simple Subject-Predicate-Object notation to represent our model.

### Step 5: Iterate and Refine (Putting it all together)

Here is our refined model, represented as Subject-Predicate-Object triples:

* **Instance of a Person:**
  * Subject: `Person:JaneAusten`
  * Predicate: `hasName`
  * Object: "Jane Austen"
* **The Role of the Person:**
  * Subject: `Person:JaneAusten`
  * Predicate: `playsRole`
  * Object: `Role:Author`
* **Instance of a Book:**
  * Subject: `Book:PrideAndPrejudice`
  * Predicate: `hasTitle`
  * Object: "Pride and Prejudice"
* **The Relationship:**
  * Subject: `Book:PrideAndPrejudice`
  * Predicate: `wasWrittenBy`
  * Object: `Person:JaneAusten`

By following these steps, we've moved from a simple, surface-level model to a more semantically rich and accurate representation of the domain.

As a rule of thumb, it's often best to model a role as a relationship between the core entities. In UML terms, this is often represented using an **Association Class**, which allows the relationship itself to have properties. For example, the 'authorship' role could be an association class between `Person` and `Book`, and it could have properties like `publicationDate` or `royaltyPercentage`.

### UML notations

```cpp
@startuml
class Book {
  + Title : String
}

class Author {
  + Name : String
}

Author "1..*" -- "*" Book : writes

note "Represents the act of an Author writing one or more Books, and a Book being written by one or more Authors." as WritesAssociation

@enduml
```

Explanation of the UML Diagram:

* Book Class:
  * We define a class called Book to represent a book.
  * It has one attribute: + Title : String, indicating that each book has a title, which is a string. The + symbol signifies public visibility.
* Author Class:
  * We define a class called Author to represent an author.
  * It has one attribute: + Name : String, indicating that each author has a name, which is a string. The + symbol signifies public visibility.
* Association (writes):
  * The line connecting the Author and Book classes represents an association, indicating a relationship between them.
  * The label writes on the association line gives a name to this relationship from the perspective of the Author.
  * The multiplicity notation `1..*` on the Author end means "one or more". This indicates that an author can write one or more books.
  * The multiplicity notation `*` on the Book end means "zero or more". This indicates that a book can be written by zero or more authors (although in many contexts, we'd expect at least one). If we wanted to enforce that a book must have at least one author, we'd use 1..* on the Book end as well.
* Note:
  * The note provides a textual description of the writes association, clarifying the meaning of the relationship and the multiplicities.
