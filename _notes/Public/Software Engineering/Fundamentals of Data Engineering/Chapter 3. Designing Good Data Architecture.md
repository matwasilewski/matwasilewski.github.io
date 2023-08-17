---
title : Designing Good Data Architecture
notetype : feed
date : 06-06-2022
---

# What is Data Architecture?
## Enterprise Architecture

![[Pasted image 20220629132534.png]]

* *Enterprise architecture is the design of systems to support change in the enterprise, achieved by flexible and reversible decisions reached through careful evaluation of trade-offs.*
* *Technical solutions exist not for their own sake but in support of business goals.*
* Central point: enterprise architecture balances flexibility and trade-offs. This isn't always an easy balance

## Data Architecture
**Definition:** *Data architecture is the design of systems to support the evolving data needs of an enterprise, achieved by flexible and reversible decisions reached through a careful evaluation of trade-offs.*

Data engineering architecture is the system and frameworks that make up the key section of the data engineering lifec cycle. We'll use *data architecture* interchangeably with *data engineering architecture* through the book.

**Operational architecture**: Functional requirements of what needs to happen related to people, processes and technology.

![[Pasted image 20220629133430.png]]

## "Good" data architecture
Good architecture serves business requirements with a common, widely reusable set of building blocks while maintaining flexibility and making the appropriate trade-offs. Good architecture is flexible and easily maintainable, and evolves in response to business needs. It is written with reversebility in mind - changes should be as cheap as possible.

Bad architecture is authoritatian and tries to cram a bunch of one-size-fits-all decisions into a big ball of mud. It is tightly coupled, rigid, overly centralized, or uses the wrong tools for the job, hampering development and change management. 

# Principles of Good Data Architecture
By AWS [Well-Architected Framework](https://docs.aws.amazon.com/wellarchitected/latest/framework/welcome.html):
* Operational excellence
* Security
* Reliablity
* Performance efficiency
* Cost optimization
* Sustainability

Google Cloud's [Five Principles for Cloud Native Architecture](https://cloud.google.com/blog/products/application-development/5-principles-for-cloud-native-architecture-what-it-is-and-how-to-master-it) are:
* Design for automation
* Be smart with state
* Favour managed services
* Practice defense in depth
* Always be architecting


As per the book:
1. Choose common components wisely.
2. Plan for failure.
3. Architect for scalability.
4. Architecture is leadership.
5. Always be architecting.
6. Build loosely coupled systems.
7. Make reversible decisions.
8. Prioritize security.
9. Embrace FinOps.

### Principle 1: Choose Common Components Wisely
Choose components that have a good trade-off of inter-operability, ease of use but also are appropriate for the engineering tasks at hand.

### Principle 3: Architect for scalability
Note that deploying inappropriate scaling strategies can result in overcomplicated systems and high costs. A straightforward relational database with one failover node may be appropriate for an application instead of a complex cluster arrangement. Measure your current load, approximate load spikes, and estimate load over the next several years to determine if your database architecture is appropriate.

