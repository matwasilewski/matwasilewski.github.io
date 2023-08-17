---
title : Chapter 2: The Data Engineering Lifecycle
notetype : feed
date : 06-06-2022
---
This is second in the series of my notes on [Fundamentals of Data Engineering](https://www.oreilly.com/library/view/fundamentals-of-data/9781098108298/)

# What is the Data Engineering Lifecycle?
* Generation
* Storage
* Ingestion
* Transformation
* Serving Data

![[ZZZ_graphics/Screenshot 2022-06-27 at 19.05.17.png]]

# Generation: Source Streams

Sample source systems:
1. Application database
2. IoT swarm and messaging queue


## Evaluating source systems: key considerations
* What are essential characteristics of the source - is it an application, a swarm etc.?
* Is data persisted long term or quickly deleted?
* What is data generation rate (at events / second, Gb / hour)
* What level of consistency can we epect? Do we have any checks in place to get numbers on data consistency?
* Are there duplicates?
* Is some data late, possibly arriving much later?
* What is the schema of the ingested data? Will data engineers need to join multiple tables?
* If schema changes how is this dealth with and communicated to downstream stakeholders?
* How frequently should the data be pulled from the source system?
* For stateful systems (eg. a db tracking customer account information) is data provided as periodic snapshots or update events from change data capute (CDC)?
* Who / What is the data provider that will transmit the data for downstream consumption?
* Will reading from a data source impact the performance?
* Does the source system have upstream data dependencies? What are the characteristics of these upstream systems?
* Are data-quality checks in place to check for late or missing data?

# Schema
Schema is one of the most challanging nuances of data engineering. The schema defines the hierarchical organisation of data. Logically, we can think of data at the level of a whole source system, drilling down into individual tables, all the way to the structure of respective fields. The schema of data shipped from the source systems is handled in various ways. Two popular options are schemaless and fixed schema.

__Schemaless__ doesn't mean absence of schema. Rather, it means that the application defines the schema as data is written, whether to a messaging queue, a flat file, a blob, or a document database such as MongoDB. 

A more traditional model built on relational database storage uses a __fixed schema__ enforced in the database, to which the application must conform.

Either of these models presents challenges for data engineers. Schemas change over time; in fact, schema evolution is encouraged in the Agile approach to software development. A key part of the data engineer’s job is taking raw data input in the source system schema and transforming this into valuable output for analytics. This job becomes more challenging as the source schema evolves.

# Storage
* data architectures in the cloud often leverage several storage solutions
* Most of cloud storage solutions also support powrful transform capabilities.
* Storage stage frequently touches on other stages - ingestion, transformation and serving.
* Streaming frameworks such as Apache Kafka and Pulsar can function simultaneously as ingestion, storage, and query systems for messages, with object storage being a standard layer for data transmission.

## Evaluating storage systems: key engineering considerations
* Is this solution compatible with the required read / write capabilities?
* Will storage create a bottleneck for downstream processes?
* Do you understand how this storage technology works? Are you utilizing the storage system optimally or committing unnatural acts? For instance, are you applying a high rate of random access updates in an object storage system? (This is an antipattern with significant performance overhead.)
* Will the storage system handle anticipated future scale? Consider total available storage, read / write operation rate / volume etc.
* Will downstream users and processes be able to retrieve data in the required service level agreement (SLA)?
* Are you capturing metadata about schema evolution, data flows, data lineaege, and so forth? Metadata has a significant impact on the utility of data and is an investment in the future, __dramatically enhancing discoverability and institutional knowledge to streamline future projects and architecture changes__.
* Is this a pure storage solution (object storage) or does it support complex query patterns? 
* Is the storage schema-agnostic (object storage)? Flexible storage (Cassandra)? Enforced schema (a cloud warehouse)?
* How are you tracking master data, golden records data quality, and data lineage for data governance?
* How are you handling regulatory compliance and data sovereignty? 

## Data temperature
* __hot data__ accessed most frequently, requires high read volumes.
* __cold data__ accessed seldom; storage should be cheap and can be slow.

# Ingestion
Ingestion usually creates the most significant bottlenecks for data engineering lifecycle.

### Key engineering considerations for the ingestion phase
* What are the use cases for the ingested data? Can I reuse this data rather than create multiple versions of the same dataset?
* Are the systems generating and ingesting this data reliably, and is the data available when needed?
* What is the data destination after ingestion?
* How frequently will I need to access the data?
* In what volume will the data typically arrive?
* What format is the data in? Can my downstream storage and transformation systems handle this format?
* Is the source data in good shape for immediate downstream use? If so, how long, and what may cauase it to be unusable?
* If data is stream, does it need to be transformed?

## Batch vs Stream
Virtually all data is streaming; Batch data is only a convenient way of streaming in large chunks. Streaming allows us to provide data in near-real time to downstream systems; frequently batch is the processing method of choice of legacy systems. There might be a significant temptation to go streaming-first, but choice between two should be based on following considerations.

### Key considerations for batch versus stream ingestion
* If in real-time, can downstream systems handle the rate of data flow?
* Do I need millisecond real-time ingestion, or would micro-batch approach with small intervals work?
* What are my use cases for streaming ingestion, and what benefits I am to reap if I am to implement streaming?
* Will streaming cost more in term of money, time, maintanance, downtime and opportunity cost than simply doing batch?
* Are my streaming pipeline and system reliable and redundant for cases of failure?
* What tools are most appropriate for the use case? Should I use managed service or stand up on my own instances of Kafka etc.? Who will manage it in the latter case? What are the tradeoffs?
* If I'm deploying a ML model, what benefits do I have with online predictions and possiby continuous training?
* Am I getting data from a live production instance? If so, what's impact of my ingestion process on this source system?

## Push vs Pull
**Push model**: Source system writes data to a target, wherever that is.
**Pull model**: Data is retrieved from the source system.
The line can be quite blurry; data is often pushed and pulled as it works it way through various stages of the pipeline.

For example, **ETL process** (Extract, Transform, Load) is a pull ingestion model - ingestion system queries a current source table snapshot on a fixed schedule.

On the other hand, consider continuous CDC. One common method of achieving it is 

# Transformation
Transformation changes the raw ingested data into formats that are useful for downstream tasks.

## Key considerations for the transformation phase
* What's the cost and ROI of the transformation?
* What is the associated business value?
* Is the transformation as simple and isolated as possible?
* Am I minimizing data movement between the transformation and the storage system during transformation?

We want to treat transformation as a standalone operation, but in practice it might be much more complicated. Typically, transformation occurs in the source or in flight during ingestion - for example, system might add a timestamp.

Business logic is a major driver of data transformation, often in data modelling. Data featurization for ML is another data transformation process.

# Serving data
Serving data is where business value is delivered from the data. Some popular uses of data are:

## Analytics
### Business intelligence
BI marshalls collected data to describe business past and current state.

Data serving for BI is a yet another area where stages of stages of data lifecycle can get tangled - logic-on-read approach is becoming increasingly popular.

As business grows, it will evolve towards self-serving analytics that will not need IT to intervene.

### Operational Analytics
Operational Analytics is different from BI in that it focuses entirely on present.

### Embedded analytics

## Machine learning
### Key considerations for data for ML
* IS the data of sufficient quality to perform reliable feature engineering?
* Is the data discoverable?
* Where are the boundaries between ML engineering and data engineering?
* Does the dataset properly represent ground trurh? It is biased?

## Reverse ETL
Reverse ETL has long been a practical reality in data, even though it was seen as antipattern. *Reverse ETL* takes processed data and feeds it back to the source systems. It is often necessary; it allows us to take analytics, scored models etc. and feed those back into production systems. 
# Major undercurrents in data engineering
[ to be completed]

## Security

## Data Management

## Orchestration
Orchestration is a process of coordinating many jobs to run as quickly and efficiently as possible on a scheduled cadence. People often refer to orchestration tools like Apache Airflow as *schedulers*. This isn't quite accurate - those tools are concerned not only with scheduling and timing but also with metadata and order, usually using DAGs.

We assume that the orchestration system stays online and has a high availability. This allows the orchestration system to sense and monitor constantly without human intervention and run new jobs anytime they are deployed. An orchestration system monitors jobs that it manages and kicks off new tasks as internal DAG. It can also monitor the external systems and tools to watch for data to arrive and criteria to be met. It will send alerts if something goes wrong.

Orchestration systems also build job history capabilities, visualization, and alerting. Advanced orchestration engines can backfill new DAGs or individual tasks as they are added to a DAG. They also support dependencies over a time range. For example, a monthly reporting job might check that an ETL job has been completed for the full month before starting. 

We must point out that orchestration is strictly a batch concept. The streaming alternative to orchestrated task DAGs is the streaming DAG. Streaming DAGs remain challenging to build and maintain, but next-generation streaming platforms such as Pulsar aim to dramatically reduce the engineering and operational burden.

## DataOps
DataOps maps the best practices of Agile methodology, DevOps, and statistical process control (SPC ) to data. Whereas DevOps aims to improve the release and quality of software products, DataOps does the same thing for data products.

Data products are built around business logic and metrics.

As Data Kitchen (experts in DataOps) describes it:
*DataOps is a collection of technical practices, workflows, cultural norms, and architectural patterns that enable:
* Rapid innovation and experimentation delivering new insights to customers with increasing velocity
* Extremely high data quality and very low error rates
* Collaboration across complex arrays of people, technology, and environments
* Clear measurement, monitoring, and transparency of results”*

“If the company has no preexisting data infrastructure or practices, DataOps is very much a greenfield opportunity that can be baked in from day one. With an existing project or infrastructure that lacks DataOps, a data engineer can begin adding DataOps into workflows. We suggest first starting with observability and monitoring to get a window into the performance of a system, then adding in automation and incident response. A data engineer may work alongside an existing DataOps team to improve the data engineering lifecycle in a data-mature company. In all cases, a data engineer must be aware of the philosophy and technical aspects of DataOps.

![[ZZZ_graphics/Pasted image 20220629122708.png]]

### Automation

Stages of automation deployment in an organization might look like this:
1. Organization attempts to schedule multiple stages of data transformation processes using cron jobs. As data pipelines get more complicated, several things are likely to happen:
	1. If hosted on a cloud service, the instance may have an operational problem causing the jobs to stop running unexpectedly.
	2. As spacing between jobs get tighter, one of the cron jobs will eventually run long causing a subsequent hob failures.
2. Engineers will adopt an orchestration framework such as Airflow. It is an operation burden, but the benefits outweight the complexity. Cron jobs will gradually be migrated - now, dependencies are checked before the jobs run.
3. A data scientist deploys a broken DAG which brings down the Airflow web server and leaving the data team operationally blind. The team realizes they need to stop allowing manual deployment of DAGs. As a next phase, they adopt automated DAG deployment - DAGs are now tested before deployment.


### Observability and monitoring
