# Domain Driven Design (DDD): Analysis of the KYC Business Domain

This report is a synthesis of the [problem statement](./problem.md) and identifying DDD artefacts. The report structure follows Agile artefacts: product backlog and epics.

The product backlog equates to a container for findings from the strategic design. The findings of tactical design equates to Agile epics. Each epic equates to a boundary context.

## Product Backlog

The team has identified three epics:

1. **Customer Onboarding Context:** The entry point for new clients, concerned with collecting data and initiating verification.
2. **Risk Management Context:** The core component for assessing and managing customer risk.
3. **Ongoing Monitoring Context:** Ensures continuous compliance by monitoring transactions and customer information over time.

## Epic 1: Customer Onboarding Context

| Artefact | Type | Description |
|---|---|---|
| `Individual Customer` | Entity | Represents an individual person like Alex. |
| `Corporate Customer` | Entity | Represents a company like Innovate Inc. |
| `Ultimate Beneficial Owner (UBO)` | Entity | The natural person who owns or controls the business. |
| `Identity Submitted` | Event | A customer submits their personal data. |
| `Onboarding Completed` | Event | All initial verification checks are complete. |
| `Full Name` | Value Object | A combination of first, middle, and last name. |
| `Residential Address` | Value Object | Contains street, city, postal code, and country. |

## Epic 2: Risk Management Context

| Artefact | Type | Description |
|---|---|---|
| `Customer Risk Profile` | Entity | Holds a customer's risk score and changes over time. |
| `Risk Assessment Requested` | Event | Triggered after a customer's identity is verified. |
| `Risk Profile Assigned` | Event | An initial risk rating is assigned to the customer. |
| `Risk Score` | Value Object | Contains the assessed risk level (e.g., Low, Medium, High). |

## Epic 3: Ongoing Monitoring Context

| Artefact | Type | Description |
|---|---|---|
| `Monitoring Alert` | Entity | Created when a suspicious event is detected. |
| `Transaction Deviation Detected` | Event | A customer's transaction pattern deviates from the norm. |
| `Risk Profile Updated` | Event | A customer's risk score changes based on new data. |
| `Transaction Details` | Value Object | Describes a specific transaction (amount, type, etc.). |
