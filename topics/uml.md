# UML Notations

The Unified Modelling Language (UML) is a general-purpose visual modelling language used to visualise, specify, construct, and document the artifacts of software systems.

## Structural Notation

* [Diagrammatic](#diagrammatic-structural-notations)
* [PlantUML](#plantuml-structural-notations)
* [Best Practices](#structural-modelling-best-practices-and-rules-of-thumb)

### Diagrammatic structural notations

![img structural notation](../assets/img/uml-structural.png)

### PlantUML structural notations

```uml
@startuml
' Basic Class with Name
class ClassName

' Class with Attributes
class Person {
  + name: String
  - age: Integer
  # address: String
}

' Class with Operations (Methods)
class Calculator {
  + add(int a, int b): int
  - subtract(int a, int b): int
  # multiply(int a, int b): int
}

' Class with both Attributes and Operations
class Car {
  + model: String
  - speed: Integer
  --
  + accelerate(): void
  + brake(): void
  - getCurrentSpeed(): Integer
}

' Abstract Class (italicised name)
abstract AbstractClass {
  abstractMethod()
}

' Interface (with <<interface>> stereotype)
interface MyInterface {
  methodA()
  methodB()
}

' Class implementing an Interface (using --|)
class ConcreteClass implements MyInterface

ConcreteClass --| MyInterface

' Inheritance (using --|> )
class ChildClass extends ParentClass
ParentClass <|-- ChildClass

' Association (simple line)
class Customer
class Order
Customer -- Order

' Directed Association (arrow indicates direction)
class User
class Role
User --> Role : has

' Association with Multiplicity
class Library
class Book
Library "1" -- "*" Book : contains

' Aggregation (hollow diamond)
class Department
class Employee
Department o-- "*" Employee : employs

' Composition (filled diamond)
class House
class Room
House *-- "*" Room : has

@enduml
```

Association class.

```uml
@startuml
class Student
class Course
class Enrollment {
  + grade: String
  + enrollmentDate: Date
}

Student "*" -- Enrollment
Course "*" -- Enrollment
Student --o "enrolls in" Course
@enduml
```

### Structural modelling best practices and rules of thumb

Creating a clear and effective UML structural diagram is not just about knowing the notations; it's about applying them thoughtfully. Here are some rules of thumb to guide your modelling decisions:

* Use Role Names to Clarify Relationships
  * **When to use them:** Always use role names when a relationship is not immediately obvious from the context. They are essential for clarifying *how* one class relates to another.
  * **Rule of Thumb:** If you have to explain the relationship between two classes in plain English, that explanation should probably be captured as a role name on the association line. For example, instead of a simple line between `User` and `Role`, adding the role name `has` (`User --> Role : has`) makes the diagram much more expressive.
* Specify Cardinality to Enforce Business Rules
  * **When to use it:** Cardinality (or multiplicity) is crucial for defining the business rules that govern your entities. You should specify cardinality for all but the most trivial associations.
  * **Rule of Thumb:** Ask yourself: "How many of these can be related to one of those?". For example, if a `Library` must have at least one `Book`, and a `Book` can belong to only one `Library`, you would use `1` on the `Library` side and `*` on the `Book` side (`Library "1" -- "*" Book`). This immediately tells the reader about a key business rule.
* Use Association Classes for Rich Relationships
  * **When to use it:** An association class is the perfect tool for when a relationship between two entities has its own properties.
  * **Rule of Thumb:** If you find yourself wanting to add attributes to an association line, you need an association class. For example, consider a `Student` and a `Course`. The relationship "enrolls in" is more than just a line; it has its own data, such as `grade` and `enrollmentDate`. The `Enrollment` association class is the correct way to model this.
* Choose Aggregation vs. Composition to Show Ownership**
  * **Aggregation (hollow diamond):** Use aggregation to represent a "has-a" relationship where the child can exist independently of the parent.
    * **Example:** A `Department` "has-a" `Employee`. If the `Department` is disbanded, the `Employee` still exists.
  * **Composition (filled diamond):** Use composition to represent a stronger "owns-a" relationship, where the child cannot exist without the parent.
    * **Example:** A `House` "owns-a" `Room`. If the `House` is destroyed, the `Room` is also destroyed.
  * **Rule of Thumb:** Ask yourself: "If I delete the parent, does the child still have any meaning?". If the answer is no, use composition. Otherwise, aggregation is likely a better fit.

## Use Case Notations

* [Diagrammatic representation](#diagrammatic-use-case)
* [PlantUML representation](#plantuml-use-case)

### Diagrammatic use case

![img use case](../assets/img/uml-usecase.png)

### PlantUML use case

Actors definition:

```uml
@startuml
actor User
actor "Admin User" as Admin
actor ":System:" as System  ' Using quotes and alias for names with spaces or special characters
@enduml
```

Use case definition:

```uml
@startuml
usecase "Login to System" as Login
usecase "View Dashboard"
usecase "Edit Profile Details" as Edit
@enduml
```

Association between actors and use cases

```uml
@startuml
actor User
usecase "Login to System" as Login
usecase "View Dashboard"

User -- Login
User -- View Dashboard
@enduml
```

## Sequence Notations

* [Diagrammatic representation](#diagrammatic-sequence)
* [PlantUML representation](#plantuml-sequence)

### Diagrammatic sequence

![img sequence](../assets/img/uml-sequence.png)

### PlantUML sequence

```uml
@startuml
title Sequence Diagram: User Authentication

actor User
participant "Web Browser" as Browser
participant "Web Server" as Server
participant "Authentication Service" as AuthDB

User -> Browser: Requests login page
Browser -> Server: GET /login
Server -> Browser: Returns login form (HTML)
Browser -> User: Displays login form

User -> Browser: Enters username and password
Browser -> Server: POST /authenticate (username, password)
Server -> AuthDB: Queries user credentials (username, password)
AuthDB -> Server: Returns authentication result (success/failure)

alt Authentication Successful
  Server -> Browser: Redirects to home page
  Browser -> User: Displays home page
else Authentication Failed
  Server -> Browser: Returns error message
  Browser -> User: Displays error message
end

@enduml
```
