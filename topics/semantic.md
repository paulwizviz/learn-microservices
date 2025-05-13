# Semantic Data Modelling

Semantic Data Modelling refers to a method of organising data that focuses on capturing the meaning of data elements and the relationships between them, rather than just their structure or format. It's about understanding what the data means in the real world or within a specific business context.

## Key Concepts

* Enhanced Understanding of the Ubiquitous Language: Semantic data modelling focuses on the meaning of data and the relationships between concepts. It can significantly contribute to establishing a clear and consistent ubiquitous language in DDD by explicitly defining terms and their semantic connections. It reduces ambiguity and ensures everyone shares a common understanding of the domain concepts.
* Identifying Core Domain Concepts and Relationships: Semantic models, like ontologies or semantic networks, can help in formally identifying the key entities, their attributes, and the various relationships that exist within the domain. It can provide a solid foundation for identifying DDD aggregates, entities, and value objects.
* Improving Communication with Domain Experts: Semantic models can act as a visual and structured way to communicate complex domain knowledge with non-technical experts. The explicit representation of concepts and their relationships can facilitate better understanding and validation of the domain model.
* Facilitating Bounded Context Identification: By analysing the semantic coherence of different parts of the domain, semantic modelling can help identify natural boundaries and potential bounded contexts within a bigger system. Areas with distinct sets of concepts and relationships might suggest different bounded contexts.
* Supporting Data Integration and Interoperability: In systems that need to interact with other systems or data sources, a semantically rich domain model can facilitate better data integration and interoperability by providing a common understanding of the underlying data structures and their meanings.

## A Simplified Example

Imagine we're trying to model some basic information about books and authors.

Instead of just saying "Book has Title" and "Author has Name", we'd explicitly define the meaning and relationships involved.

We might use a simple notation like Subject-Predicate-Object (like the basis of RDF triples), which is a common way to represent semantic relationships.

### Our Domain: Books and Authors

**Entities:** Book, Author
**Properties (Attributes):** Title (of Book), Name (of Author)
**Relationships:** Author writes Book, Book has author Author

### Semantic Model Representation (using Subject-Predicate-Object)

Here is the model:

* Instance of an Author:
  * Subject: Author:JaneAusten
  * Predicate: hasName
  * Object: "Jane Austen"
* Instance of a Book:
  * Subject: Book:PrideAndPrejudice
  * Predicate: hasTitle
  * Object: "Pride and Prejudice"
* The Relationship between the Author and the Book:
  * Subject: Book:PrideAndPrejudice
  * Predicate: hasAuthor
  * Object: Author:JaneAusten
  * Alternatively, we could also represent the relationship from the author's perspective:
  * Subject: Author:JaneAusten
  * Predicate: writes
  * Object: Book:PrideAndPrejudice

Here is the explanation:

* We're not just storing strings "Jane Austen" or "Pride and Prejudice". We're identifying them as specific entities (Author:JaneAusten, Book:PrideAndPrejudice). The prefix (e.g., Author:, Book:) is a simple way to denote the type of entity. In a more formal system, these might be URIs (Uniform Resource Identifiers) providing a unique and global identity.
* The predicates (hasName, hasTitle, hasAuthor, writes) explicitly define the nature of the relationship between the subject and the object. They give meaning to the connection. We understand that hasName means the name of the author, and hasAuthor means the author of the book.
* The objects are either literal values (like the strings for names and titles) or references to other entities (like Author:JaneAusten being the object of the hasAuthor predicate for Book:PrideAndPrejudice).

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
