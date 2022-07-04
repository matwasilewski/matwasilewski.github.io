---
title : Airflow and TaskFlowAPI
notetype : feed
date : 04-06-2022
---

I read a [great article](https://towardsdatascience.com/taskflow-api-in-apache-airflow-2-0-should-you-use-it-d6cc4913c24c#1bde) on Airflow's TaskFlowAPI by [Anna Geller](https://www.linkedin.com/feed/#). I will summarize her points and share my thoughts as a fresh user of Airflow. Her exploration of TaskFlowAPI helped me understand the purpose and problems of the package at large, and I highly recommend you read the full article.

# The summary

1. Airflow is a scheduler and data orchestration platform, NOT a data processing framework. You should not try to build data flows on Airflow.
2. Airflow uses XComms system to transfer (meta)data between tasks. Airflow instance contains a relational database that is used by XComms, and Airflow tasks save their output in said database. When using TaskFlowAPI, this happens by default and implicitly. Saved data can be later retrieved by other tasks.
3. This XComms mechanism is problematic for following reasons:
- *XComs add a hidden dependency between tasks as the pulling task has an implicit dependency on the task pushing the required value.*
- The database has no TTL and no cleanup logic. Therefore, it's very easy to imagine an inexperienced user push a number of large objects to the database, not knowing that they are saving anything!
4. Because TaskFlowAPI converts functions into a PythonOperators - essentially, a chunk of code that is to be executed by AirflowWorker in TaskInstance, all of Airflow pipelines would have to have the same Python version & dependency versions to work.

# My thoghts  
## The Python dependencies conundrum
My understanding is that Anna's criticism in point 4. is not targeted at TaskFlowAPI specifically, but more generally at use of Python Operators in Airflow code, that might introduce dependencies on specific version of Python. 

To avoid it, the only solution would be to switch to using Kubernetes-based operators that would fully isolate the functional logic of the code and allow software engineers to specify dependencies for each tasks individually. In most cases that might be an overkill and be costly in engineering time, but would introduce the best engineering practices.

Such isolation had already been suggested back in 2018 by (Jessica Laughlin)[https://medium.com/@jldlaughlin] in article (We’re All Using Airflow Wrong and How to Fix It)[https://medium.com/bluecore-engineering/were-all-using-airflow-wrong-and-how-to-fix-it-a56f14cb0753]. Since then, Airflow had introduced out-of-the-box KubernetesPodOpertor. She pointed to another problem that dogs Airflow - poor separation of concerns between orchestration logic and processing logic.

## Separation of functional and orchestration logic
Airflow has poor separation of concerns between orchestration and execution. For me using the tool seems like a constant act of balancing trade-offs between doing things the proper way suggested by Jessica (dockerization, and delegating data processing tasks to other tools eg. Apache Spark) and ease of use - just doing it within Airflow.

When reading the documentation, a new user is warned that Airflow is not a data processing tool and that only small bits of metadata might be passed between tasks; but then the very tutorial meant to introduce users to the tool (implicitly) passes a piece of simulated data between Airflow Tasks within an ETL job. The same ETL job also performs some basic processing of the data and does not delegate it anywhere - so, do as we say, not as we do?

In all of the articles that I've read, the most frequent complaint about Airflow was that a failing pipeline is very difficult to debug.

# Resources
I've found some excellent resources on Airflow; I include them here:
- [Anna Geller's blog on Medium](https://annageller.medium.com)
- [Marc Lamberti's blog about Airflow](https://marclamberti.com/blog/)
- [The official documentation](https://airflow.apache.org/docs/apache-airflow/stable/index.html)