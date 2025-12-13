# Deployment Architecture

This report describes our decisions that have led to our choice of deployment architecture.

> NOTE:
> This is an abridge version of the report. This report only covers an overview of the our deployment architecture. A full version of this report would cover detail analysis of subsystem. For now we have included place holder sections.
> This aspect of the report is typically attached to a product backlog.

## Architecture Overview

After due considerations we have decided to adopt an event driven architecture.

<figure>
  <img src="../assets/img/architecture.png" alt="Deployment Architecture">
  <figcaption>Figure 1: Deployment Architecture.</figcaption></br></br>
</figure>

> NOTE:
> A full version here will include consideration of different configuration.

## API Gateway

We plan to use Kong API gateway.

> NOTE:
> A full version here will include consideration of different configuration.

## Event Bus

We plan to use Kafka.

> NOTE:
> A full version here will include consideration of different configuration.

## Services

We plan to implement microservices using Kubernetes.

> NOTE:
> A full version here will include consideration of different configuration.

## Database

We plan to use PostgreSQL as our database technologies.

> NOTE:
> A full version here will include consideration of different configuration.
