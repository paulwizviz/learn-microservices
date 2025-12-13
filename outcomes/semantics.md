# Semantic Data Modelling: Identifying Core Entities and Relationships

Based on information gathered from the [DDD analysis](./domain.md), the team began a detailed analysis of each artefact, expanding their definitions here. This report also defines the relationships between the artefacts.

## Epic 1: Analysis of Customer Onboarding Context

Having analysed the artefacts in this context, we conclude that a `Customer` is not a core entity that can exist on its own terms. It only exists as a role establishing a relationship between a `Person` and an `Organisation`. A `Customer` is, therefore, an association entity.

`Person` and `Organisation`, however, are core entities that can exist on their own terms.

Having identified the `Person`, `Organisation`, and `Customer` entities, we modelled the relationships diagrammatically, as shown in Figure 2.

<figure>
  <img src="../assets/img/kyc-sdm.png" alt="Data Model">
  <figcaption>Figure 2: A diagrammatic representation of a KYC data model.</figcaption></br></br>
</figure>

## Epic 2: Analysis of Risk Management Context

Analysis of this context yields two core entities similar to Epic 1: `Person` and `Organisation`. The DDD artefact, `Customer Risk Profile`, is a synthesis of this analysis. A risk profile is, therefore, an association between a `Person` and an `Organisation`. The association reflects an `Organisation`, or specifically a risk assessor, assigning a `Customer Risk Profile` to either a `Person` or an `Organisation`.

> Note: A diagram is not included, as this is an abbreviated analysis.

## Epic 3: Analysis of Ongoing Monitoring Context

Analysis here also yielded two entities similar to Epics 1 and 2: `Person` and `Organisation`. When a `Risk Profile Updated` event is triggered relating to a `Person` or an `Organisation`, a `Monitoring Alert` entity is created and associated with that `Person` or `Organisation`.

> Note: A diagram is not included, as this is an abbreviated analysis.

## Common Entities

The following entities are common across all domains:

* `Person` entity
* `Organisation` entity
