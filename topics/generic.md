# Generic Entity Model

Generic Entity Modelling refers to an approach to data modelling that aims to identify common, reusable patterns and structures for representing entities and their relationships across different parts of a system or even across multiple systems or domains. Instead of modelling each specific entity and its attributes from scratch, it looks for underlying similarities and tries to create more abstract, general-purpose models

## Key Characteristics

* Identifying Common Patterns and Structures: Generic entity modelling aims to identify reusable patterns and structures applicable across different parts of the domain or multiple domains.
* Improving Model Flexibility and Extensibility: By identifying generic entity types and relationships, you can create more flexible models adaptable to future changes and evolving business needs without requiring significant rework.
* Accelerating Initial Model Creation: Generic models or metamodels can provide a starting point for domain modelling, offering common entity types and specialised relationships for the specific domain.
* Enhancing Data Consistency and Integrity: Applying consistent modelling patterns based on generic entity concepts can help ensure better data consistency and integrity across the system.
* Supporting the Identification of Technical Services: Generic entity models can sometimes highlight opportunities for creating reusable technical services that operate on similar data structures.

## A Simplified Example

Imagine we're trying to model various types of "Things" in a system. Instead of creating separate models for "Customer", "Product", "Order", etc., we can start with a more generic concept:

```cpp
@startuml
class GenericEntity {
  + EntityID : UUID
  + EntityType : String
  -- Metadata --
  + GetMetadata(key: String) : String
  + SetMetadata(key: String, value: String)
}

class GenericRelationship {
  + RelationshipID : UUID
  + RelationshipType : String
  + SourceEntityID : UUID
  + TargetEntityID : UUID
  -- Metadata --
  + GetMetadata(key: String) : String
  + SetMetadata(key: String, value: String)
}

GenericEntity "1..*" -- "*" GenericRelationship : relates >
GenericEntity "*" -- "1..*" GenericRelationship : < is related to

note over GenericEntity : Represents any identifiable thing in the system.
note over GenericRelationship : Represents a connection between two GenericEntities.
note over GenericEntity::Metadata : Stores specific attributes for each Entity instance.
note over GenericRelationship::Metadata : Stores specific attributes for each Relationship instance.
@enduml
```

### Explanation of the UML Diagram

* **GenericEntity Class**
  * **EntityID:** A universally unique identifier (UUID) to uniquely identify each instance of a "Thing".
  * **EntityType:** A string that specifies the specific type of the entity (e.g., "Customer", "Product", "Order"). This allows us to differentiate between the various kinds of "Things" we're modelling.
  * **Metadata**: This section represents a flexible way to store specific attributes for each GenericEntity. Instead of having fixed attributes directly in the GenericEntity class, we use a mechanism (like a key-value store internally) to hold properties that are specific to a particular EntityType.
  * **GetMetadata(key: String):** String: An operation to retrieve a specific attribute's value using a key.
  * **SetMetadata(key: String, value: String):** String: An operation to set or update a specific attribute's value using a key-value pair.
* **GenericRelationship Class**
  * **RelationshipID:** A UUID to uniquely identify each instance of a relationship.
RelationshipType: A string that specifies the type of relationship (e.g., "has ordered", "is a customer of", "is located at").
  * **SourceEntityID:** A reference (using the EntityID) to the GenericEntity that is the source of the relationship.
  * **TargetEntityID:** A reference (using the EntityID) to the GenericEntity that is the target of the relationship.
  * **Metadata:** Similar to GenericEntity, this allows us to store specific attributes about the relationship itself (e.g., the date of the order for an "has ordered" relationship).
  * **GetMetadata(key: String):** String: Retrieves metadata for the relationship.
  * **SetMetadata(key: String, value: String):** String: Sets metadata for the relationship.
Relationships between GenericEntity and GenericRelationship:

The lines with arrows indicate associations (relationships) between GenericEntity and GenericRelationship.
GenericEntity "1..*" -- "*" GenericRelationship : relates >: One GenericEntity can be the source of zero or more (*) GenericRelationship instances. The arrow indicates the direction of the "relates" perspective.
GenericEntity "*" -- "1..*" GenericRelationship : < is related to: One GenericEntity can be the target of zero or more (*) GenericRelationship instances. The arrow indicates the direction of the "is related to" perspective.

### How it Works in Practice

To model a specific domain using this generic model, you would:

* Create instances of GenericEntity:
  * An entity of type "Customer" with EntityID = 123. Its metadata might include "Name" = "John Smith", "Email" = `"john.smith@example.com"`.
  * An entity of type "Product" with EntityID = 456. Its metadata might include "ProductName" = "Widget A", "Price" = "10.99".
  * An entity of type "Order" with EntityID = 789. Its metadata might include "OrderDate" = "2025-05-13", "OrderStatus" = "Pending".
* Create instances of GenericRelationship to link these entities:
  * A relationship of type "has ordered" with RelationshipID = abc, SourceEntityID = 123 (Customer), and TargetEntityID = 789 (Order). This relationship's metadata might include "Quantity" = "2".
  * A relationship of type "contains product" with RelationshipID = def, SourceEntityID = 789 (Order), and TargetEntityID = 456 (Product). This relationship's metadata might include "LineItemNumber" = "1".
