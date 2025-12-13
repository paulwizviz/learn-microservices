# Designing and Implementing an Extensible Software: A Recommendation

## About this Handbook

This handbook provides a synthesis of knowledge from various sources, compiled by its [contributors](./CONTRIBUTORS). It presents an approach to engineering techniques like Domain-Driven Design (DDD), Semantic Data Modelling (SDM), and architectural patterns to help you design and build extensible software.

In this handbook, you will be taken on a step-by-step journey through a process of designing and implementing extensible software within a hypothetical scenario. The walk-through is presented in two parts: Part I focuses on using analytical techniques to understand the problem space, and Part II demonstrates how to map that analysis to a well-architected solution, covering everything from naming conventions to deployment patterns. Refer to Figure 1 for a summary of the journey you will take.

<figure>
  <img src="./assets/img/the-process.png" alt="The Process Diagram">
  <figcaption>Figure 1: A diagram illustrating the journey you will take.</figcaption></br></br>
</figure>

The process you are about to embark on may seem different from a typical software development process. You will not start with the User Interface (UI) or backend systems as the basis of your analysis. Instead, you will begin by immersing yourself in the business domain, treating it as the main focus of your efforts. This "domain-first" approach is crucial for building extensible and robust systems, as it gives you the basis to match technology with business goals.

Although the journey is depicted as a clear path in Figure 1, real-world application is rarely so linear. The process is iterative, often requiring you to revisit earlier stages. For clarity, this handbook presents each stage as a distinct step, but in practice, the boundaries are much more fluid.

These techniques are suited for systems of any scale, but you will find them most useful for large-scale enterprise systems.

This is not a textbook, but as a living document that will evolve with the ongoing learning and diverse viewpoints of the community.

## Our Hypothetical Scenario

For your journey, imagine you are part of a fictional company, **Datum**. Its mission is to build a centralised system that allows other businesses, like banks or regulated firms, to outsource their complex and critical Know Your Customer (KYC) processes.

KYC is typically conducted by regulated institutions themselves, but in this hypothetical scenario, you will imagine Datum as an outsourcing entity. Third-party institutions will use `Datum` to verify that their own customers have undergone the necessary KYC, without having to hold customer data themselves. You should bear this scenario in mind, as it influences the outcome of the DDD and SDM analysis; an in-house solution would have a different context.

## Part I: Understanding the Business Domain

In Part I, you will begin with the crucial first step: understanding the problem space our fictional company, **Datum**. This part of the exercise will take you through two key stages.

First, you will understand the nature of the business domain, establishing the boundaries and context for the system you are tasked to build.

Second, you will analyse this domain in depth, relating your findings to the core elements of DDD and SDM. Finally, you will translate your understanding into a clear and precise data model, laying a robust foundation for the solution you will build in Part II.

## Scope of Domain

You will need to survey the landscape to understand the context in which the solution you are building is expected to operate. In the real world, this means immersing yourself, as a software engineer, in the business domain with domain experts.

Imagine you and your team are gathered around a whiteboard, collaboratively brainstorming, sketching diagrams, and capturing conversations to build a shared understanding. Your goal is to ask probing questions and distil the complex processes into a coherent picture.

Assume you have gone through this collaborative process, how should you capture knowledge and share it with your team?

You are not bound by any single formal methodology for knowledge capture. You can use any techniques that could involve structured and/or unstructured data.

For the purpose of this illustration, you have decided to capture it in the form of a [structured document](./outcomes/problem.md).

## Applying Domain-Driven Design

With the problem statement defined, you will now bridge the understanding between business and technical experts through Domain-Driven Design (DDD). In this stage, you will establish a **shared language** that will directly inform the names you use in your code, ensuring the solution aligns with business goals.

Your DDD analysis will unfold in two parts: **Strategic Design** to map the overall business domain, and **Tactical Design** to define the specific building blocks of your system. You can refer to the [Domain-Driven Design: Key Concepts](./concepts/domain.md) guide for more details.

Your objective in **Strategic Design** is to carve the system into well-defined, independent parts called **Bounded Contexts**. Each context is a self-contained sub-domain with its own language and model. To identify these, you should start with first-cut contexts and then move to Tactical Design to see if there are overlapping vocabularies. If so, you will revisit and redraw your boundaries until the artefacts within each are logically clustered.

Following your Strategic Design, your task in **Tactical Design** is to drill down into each Bounded Context to identify the core building blocks: **Aggregates**, **Entities**, **Value Objects**, and **Domain Events**.

Let's assumed that you have decided to record your findings as [Agile style artefacts](./outcomes/domain.md).

## Semantic Data Modelling

With your DDD components identified, your next step is to build a more precise map of the domain using **Semantic Data Modelling (SDM)**. The task here is to refine the DD artefacts. This means examining the semantics and how the DD artefacts related to each other from a business perspective. For more details, refer to the [Semantic Data Modelling: Core Concepts](./concepts/semantic.md) guide.

You could use a number of techniques like UML to record your analysis. Let us assumed you produce a [report](./outcomes/semantics.md) using a mix of text and UML. It is worth noting that the report is only for illustration and the analysis only cover a single context.

## Part II: Architecture and Implementation

In Part II of the handbook, your primary task is to translate the conceptual knowledge gained from Part I into tangible, implementable artefacts. This involves creating the deployable units and source code that will bring your system to life.

To accomplish this, you will need to focus on three critical areas:

1. **Conduct a Use Case Analysis**
2. **Define the Deployment Architecture:** Next, you must decide on how your system will be deployed, considering patterns like the monolith, microservices, or an Event-Driven Architecture (EDA).
3. **Design the Code Architecture:** Finally, your task is to design the layout of the source code to be clean, maintainable, and scalable.

## Use Case Analysis

Your task in this phase is to model how your expected users, or personas, will interact with the system you intend to build. Part of the task is to identify and create a profile of personas. One recommended approach is Roman Picher's [From personas to user stories](https://www.romanpichler.com/blog/personas-epics-user-stories/). You could also augment the analysis with UML use case notation. Let's assumed you have decided to follow the recommendation and produce a [report](./outcomes/usecase.md).

## Deployment Architecture

Here, your task is model how subsystems relate with each other. You can find inspiration in the [Deployment Architectural Patterns](./concepts/arch.md) guide.

Let's use this to illustrate how you could report [your deployment architecture](./outcomes/deployment.md).

## Code Architecture

In this final stage, your task is to translate the conceptual models from your previous work into source code. The primary goal here is to use the outcomes of DDD and SDM as the shared, ubiquitous language for naming components, variables, and other constructs within the code. This practice ensures the implementation remains aligned with business requirements.

You should be aware of a potential challenge: the idiomatic conventions and organisational philosophies of a programming language can sometimes conflict with the structure of the ubiquitous language. For example, a language may favour certain naming conventions that differ from the terms used by domain experts.

For guidance, you can refer to the well-known patterns described in our [Source Code Architecture](./concepts/code.md) segment. However, remember that the core goal is to align with the ubiquitous language, so you may need to adjust these patterns accordingly.

Let's assumed that you are task with mapping your findings to SQL and Go. Here is an [illustration of the mapping process](./outcomes/code.md).

## Summary of the Process

In this handbook, we have taken you on a journey through a comprehensive, domain-first approach to software design and implementation. By following the steps we have laid out, you are now equipped to create extensible solutions that align closely with your business goals.

In **Part I**, you began by immersing yourself in the problem domain, using our hypothetical KYC scenario to ground your analysis. We then guided you in applying the principles of **Domain-Driven Design (DDD)**, from using Strategic Design to define your Bounded Contexts, to using Tactical Design to identify your core Aggregates, Entities, and Value Objects. Following this, you used **Semantic Data Modelling (SDM)** to construct a precise, business-aligned data model.

In **Part II**, your focus shifted to implementation. You started with a **Use Case Analysis** to model system interactions and defined a suitable **Deployment Architecture**—in our case, an Event-Driven Architecture. Finally, you explored **Code Architecture**, where we demonstrated how you can translate your conceptual models into tangible code using **SQL** and **Go**, ensuring that the ubiquitous language from your domain analysis is reflected in the implementation.

By embracing this end-to-end process, you can confidently build software systems that are not only robust and maintainable but also deeply connected to the business domain they serve.

## Disclaimer

The content of this repository is for educational purposes only. While the examples are inspired by real-world scenarios, they are simplified for clarity and may not be suitable for production use. The opinions and techniques presented here are intended as learning materials, and their applicability to your own projects is not guaranteed.

## Licence

This work is licensed under a [Creative Commons Attribution-ShareAlike 4.0 International License](http://creativecommons.org/licenses/by-sa/4.0/).