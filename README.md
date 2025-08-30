# Designing and Implementing an Extensible Software: A Recommendation

## About this Handbook

This handbook presents curated opinions on the application of Domain-Driven Design (DDD), Semantic Data Modelling and Architectural Patterns (e.g. clean architecture, microservices, etc) to build extensible software. This handbook is written for technical practitioners, such as software engineers and architects, who design and build extensible systems. In the age of AI, the principles in this handbook also provide the context needed to effectively guide LLMs in generating robust solutions. It represents a synthesis of knowledge and perspectives from various sources compiled and identified by the handbook's [contributors](./CONTRIBUTORS). Please note that this is not a textbook but rather a living document that reflects the ongoing learning and diverse viewpoints of the community.

To illustrate these techniques in practice, this handbook walks you through a hypothetical scenario. The walk-through is presented in two parts: Part I focuses on using analytical techniques to understand the problem space, and Part II demonstrates how to map that analysis to a well-architected solution, covering everything from naming conventions to deployment patterns.

Please refer to the CONTRIBUTORS file for a list of contributors or the repository's commit history.

## Our Hypothetical Scenario

The purpose of this system is to manage identities, company information, and the relationships between individuals and companies. The current process is manual and time-consuming, involving verifying company records, director details, and the relationships between them. Our goal is to automate and streamline this process by designing and building a robust, extensible software solution.

## Part I: What Problem are we Solving?

All software exists to solve a problem. This handbook focuses on large-scale enterprise software—the kind that requires a coordinated team to build and maintain. The first and most critical step in such a project is developing a shared understanding of the problem domain. This part of the handbook will illustrate how to use Domain-Driven Design and Semantic Data Modelling to analyse the problem space and create a ubiquitous language for that shared understanding.

## Disclaimer

The content of this repository is for educational purposes only. While the examples are inspired by real-world scenarios, they are simplified for clarity and may not be suitable for production use. The opinions and techniques presented here are intended as learning materials, and their applicability to your own projects is not guaranteed.

## License

This work is licensed under a [Creative Commons Attribution-ShareAlike 4.0 International License](http.creativecommons.org/licenses/by-sa/4.0/).
