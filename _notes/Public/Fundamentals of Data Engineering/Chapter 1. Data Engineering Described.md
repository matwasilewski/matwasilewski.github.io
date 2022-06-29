---
title : Chapter 1. Data Engineering Described
notetype : feed
date : 04-06-2022
---

This is the first in the series of my notes on [Fundamentals of Data Engineering](https://www.oreilly.com/library/view/fundamentals-of-data/9781098108298/)

# Chapter 1. Data Engineering Described
## What is Data Engineering?

A data engineer gets data, stores it, and prepares it for consumption by data scientists, analysts, and others. We define data engineering and data engineer as follows:
_Data engineering is the development, implementation, and maintenance of systems and processes that take in raw data and produce high-quality, consistent information that supports downstream use cases, such as analysis and machine learning. Data engineering is the intersection of security, data management, DataOps, data architecture, orchestration, and software engineering. A data engineer manages the data engineering lifecycle, beginning with getting data from source systems and ending with serving data for use cases, such as analysis or machine learning._

## Data Engineering lifecycle
![[Screenshot 2022-06-27 at 16.04.14.png]]
We should not fixate on technology and miss the forest for the trees. Data must serve an end goal; stages of data engineering lifecycle are as follows:
* Generation
* Storage
* Ingestion
* Transformation
* Serving


Data engineering also has a notion of _undercurrents_ - concepts and principles that are relevant through the whole lifecycle. These include:
* Security
* Data Management
* DataOps
* Data Architecture
* Orchestration
* Software Engineering

## Data Science, Data Engineering
Data Engineering is an upstream task as compared to data science - meaning that data engineers provide input for data scientists.

![[Screenshot 2022-06-27 at 16.13.14.png]]

About 80% of a Data Scientist's time is spent toiling on the three lower levels of the pyramid, pipeline plumbing and extracting data. Data Engineer's role is to provide production-grade systems for them. 

### Data Engineer's balancing acts
* Cost
* Agility
* Scalability
* Simplicity
* Reuse
* Interoperability

## Data Maturity and Data Engineer
_Data maturity_ is the progression towards higher data utilization, capabilities and integration across the organization.

The books suggests the following Data Maturity Model (DMM):

### 1. Starting with data
Characteristics:
- Data team is small
- Goals are not defined, or are loosely defined
- Data Engineer is a generalist
- Getting ML wins at this stage is rare but possible, as without a solid data foundation creating and deploying them in production is difficult

What should a data engineer focus on at this stage?

- Get a buy-in from stakeholders, including the execs. Ideally, a data engineer should have a sponsor for critical initiatives to design and build a data architecturet to support the company's goals.
- Define the right data architecture:
	- Account for business goals
	- Account for the competetive advantage you're aiming to achieve with your data initiative
- Build a solid data foundation for future data analysts and data scientists to generate reports and models that provide competetive value.

Typical pitfalls & tips:
- Organizotional willpower will wane in absence of quick wins and value. Keep in mind that quick means usually mean acquiring technical debt.
- Avoid working in silos - go out and talk to people.
- Avoid undifferentiated deavy lifting; use off-the-shelf whenever possible.
- Build custom solutions and code only where this creates competetive advantage.
- 

### 2. Scaling with data
Characteristics:
- Practices established; no more ad hoc solutions.
- Challange is to create scalable data infrastructure
- Plan for future where company is genuinely data-driven

Data Engineer's focus:
- Establish formal data practices
- Create scalable and robust data architectures
- Adopt DevOps and DataOps practices
- Build systems that support ML
- Continue to avoid undifferentiated heavy lifting and customize only when a competetive advantage results

Typical pitfalls & tips:
- There's a temptation to adopt bleeding-edge tech which is rarely a good use of time and energy. __Any and every decision should be driven by the value delivered to customer__
- The main bottleneck for scalling is the data engineering team. Focus on simple solutions.
- You’ll be tempted to frame yourself as a technologist, a data genius who can deliver magical products. Shift your focus instead to pragmatic leadership and begin transitioning to the next maturity stage; communicate with other teams about the practical utility of data. Teach the organization how to consume and leverage data. 

### 3. Leading with data
Characteristics:
- Company is data-driven.
- Self-service analytics and ML possible.
- Introducing new data sources is seamless.

Data Engineer's focus:
- Create automation for introduction of new data
- Focus on custom tools and systems for competetive advantage
- Focus on data management, governance, quality and DataOps.
- Deploy tools that expose and disseminate data throughout the organization, including data catalogs, data lineage tools and metadata management systems.
- Collaborate efficiently with software engineers, ML engineers, analysts and others.

Typical pitfalls & tips:
- Complacency is a significant danger - constant focus on maintanence is a must.
- Technology distractions are a more significant danger here than in the other stages. There’s a temptation to pursue expensive hobby projects that don’t deliver value to the business. Utilize custom-built technology only where it provides a competitive advantage.

## Skills of data engineer
- Know how to communicate with technical and non-technical people
- Understand how to scope and gather business and product requirements: You need to know what to build and ensure that your stakeholders agree with your assessment. __In addition, develop a sense of how data and technology decisions impact the business__
- Understand the cultural foundations of Agile, DevOps and DataOps.
- Control costs
- Learn continuously
- See the bigger picture

_Communication is vital, both for technical and nontechnical people. We often see data teams succeed based on their communication with other stakeholders; success or failure is rarely a technology issue. Knowing how to navigate an organization, scope and gather requirements, control costs, and continuously learn will set you apart from the data engineers who rely solely on their technical abilities to carry their career._

## Keeping pace in a fast-moving field
How to keep your skills sharp in a rapidly changing field that is engineering? The advice is to focus on fundamentals rather than on the latest tools, as the former will not change; however, keep your eye on changes on the tech landscape.


