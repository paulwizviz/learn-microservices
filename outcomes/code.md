# Code Architecture

In this report, we took the information gathered from Part I, use case analysis and deployment architecture to map it to the code. In this case, we have elected to us Go and SQL.

> NOTE: This is an abridge verion of the report.

## Mapping to SQL

Consider the SDM model of a relationship between Entities and value objects as shown in Figure 1.

<figure>
  <img src="../assets/img/person-name-sdm.png" alt="Person name">
  <figcaption>Figure 1: Person name object values.</figcaption></br></br>
</figure>

Our recommendation is to map this to SQL in a way that gives us a high degree of flexibility.

```sql
-- Table for the 'person' entity.
CREATE TABLE IF NOT EXISTS person (
    id SERIAL PRIMARY KEY,
    person_uuid UUID UNIQUE NOT NULL,
    person_name_id INTEGER,
    residential_address_id INTEGER
);

-- Table for the 'person_name' value object.
CREATE TABLE IF NOT EXISTS person_name (
    id SERIAL PRIMARY KEY,
    first_name TEXT NOT NULL,
    middle_name TEXT,
    surname TEXT NOT NULL
);

-- Table for the 'address' value object.
CREATE TABLE IF NOT EXISTS address (
    id SERIAL PRIMARY KEY,
    building TEXT,
    street TEXT,
    city TEXT,
    county_state TEXT,
    country TEXT
);

-- Add constraints to ensure a 1-to-1 relationship.
ALTER TABLE person ADD CONSTRAINT fk_person_name FOREIGN KEY (person_name_id) REFERENCES person_name(id) ON DELETE CASCADE;
ALTER TABLE person ADD CONSTRAINT unique_person_name UNIQUE (person_name_id);
ALTER TABLE person ADD CONSTRAINT fk_residential_address FOREIGN KEY (residential_address_id) REFERENCES address(id) ON DELETE SET NULL;
ALTER TABLE person ADD CONSTRAINT unique_residential_address UNIQUE (residential_address_id);
```

Alternatively, we could denormalise the tables. However, this would make our schema inflexible. For example, we would not be able to represent a relationship suggesting a person's name has been verified by a government authority.

Revisiting the SDM, we find a model as shown in Figure 2.
<figure>
  <img src="../assets/img/verified-name.png" alt="Verified person name">
  <figcaption>Figure 2: Verified person name.</figcaption>
</figure></br>

We have decided to model Figure 2 in SQL as follows:

```sql
-- Table for the 'organisation' entity.
CREATE TABLE IF NOT EXISTS organisation (
    id SERIAL PRIMARY KEY,
    organisation_uuid UUID UNIQUE NOT NULL,
    organisation_name TEXT NOT NULL
);

-- Table for the 'registered_name' association.
CREATE TABLE IF NOT EXISTS registered_name (
    id SERIAL PRIMARY KEY,
    registered_name_uuid UUID UNIQUE NOT NULL,
    person_id INTEGER NOT NULL,
    person_name_id INTEGER NOT NULL,
    organisation_id INTEGER NOT NULL,
    registered_at TIMESTAMPTZ DEFAULT NOW() NOT NULL
);

-- Add foreign key constraints.
ALTER TABLE registered_name ADD CONSTRAINT fk_registered_name_to_person FOREIGN KEY(person_id) REFERENCES person(id) ON DELETE CASCADE;
ALTER TABLE registered_name ADD CONSTRAINT fk_registered_name_to_person_name FOREIGN KEY(person_name_id) REFERENCES person_name(id) ON DELETE CASCADE;
ALTER TABLE registered_name ADD CONSTRAINT fk_registered_name_to_organisation FOREIGN KEY(organisation_id) REFERENCES organisation(id) ON DELETE CASCADE;

-- Add unique constraint.
ALTER TABLE registered_name ADD CONSTRAINT unique_name_registration_per_org UNIQUE (person_id, person_name_id, organisation_id);
```

## Mapping to Go

When mapping our SDM to Go, we had to decide on the structure of our project. Go is not a prescriptive language; it does not enforce a single project layout. After reviewing this recommended way to [organise a Go module](https://go.dev/doc/modules/layout) for a recommended approach. After due consideration we decided to organise our project as follows:

```text
/cmd
  /onboarding <-- Onboarding context from DDD
    main.go
  /risk <-- Risk management context from DDD
    main.go
  /monitoring <-- Monitoring context from DDD
    main.go
/internal
  /api <-- implementation of API
    doc.go
    router.go
  /person
    doc.go
    entity.go
    service.go
    dto.go
  /org
    doc.go
    entity.go
    service.go
    dto.go
  /customer
    doc.go
    entity.go
    service.go
    dto.go
  /pgops <-- this package represents specific PostgreSQL operations
    doc.go
    pgops.go
```

Revisiting Figure 1, we decided to map `Person` entity as a Go package named `person`

```go
package person

type NamedProfile struct {
    FirstName  string
    MiddleName string
    Surname    string
}
```

Mapping the `Organisation` entity to a package named `org`:

```go
package org

// FinancialInst represents data about a financial institution.
type FinancialInst struct{
    // ...
}

// Regulator represents data about a regulator.
type Regulator struct{
    // ...
}
```
