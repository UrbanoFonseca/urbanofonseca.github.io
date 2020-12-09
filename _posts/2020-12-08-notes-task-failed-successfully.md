---
layout: post
title: "Notes on 'Task Failed Successfully' talk by Jeremiauh Lowin  on PyData DC 2018"
author: "Francisco Fonseca"
categories: notes
tags: [notes]
image: barcelona-2.jpg
---
## The Talk
<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/TlawR_gi8-Y?controls=0" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Workflow Management

> Workflow Management is when you have things you want done and you put a system in place to make sure they happen.

### Negative vs Positive Engineering

While Positive Engineering refers to things you want to do \(e.g. features\), **Negative Engineering** is everything you need to do to make sure those things work \(e.g. error-trapping, type coercion, data serialization\).

> **Negative Engineering** can be resumed as anticipating and containing an infinity of possible and dependent failures.

While any task can in theory lookout to its own state and failures, we need to lookout to the integrity of the system as a whole and, in order to do that, we need to know how to work with failure.

### First-Class Failure

Failing successfully means that failure is a first-class part of your system. To your system, failure is just one possible outcome; not necessarily good or bad. It becomes simply the starting state for the next piece of logic.This is crucial to guarantee that _task failure_ does not necessarily imply a _system failure._

This doesn't mean that we will override the failures. For some occasions, you _do_ want the system to shutdown if you face one \(or many\) failure\(s\). It is not up to the workflow system to decide whether failure is necessarily good or bad, its job is to reveal, promote and make failure available to any business purpose.

### Wrong Way Risk

What happens when we don't embrace failure and treat it as an outlier?

1. Workflow systems assume success, failure as outlier
2. Workflow manager is most critical when things fail
3. Workflow system introduces **wrong way risk**: it is least helpful when it is most needed

> **Wrong way risk** is the idea that your exposure to a bad outcome is exaggerated when the bad outcome occurs. _Example: negatively-correlated insurance._

#### Robust Data Pipelines

Often people design "robust data pipelines" by worrying exclusively about data, which is fine if you believe your workflow system is flawless. But that is not always the case.

### Workflow Guarantees

#### What it should do

1. **Protect** against errors and failure
2. **Promise** that tasks will run as intended
3. **Provide** tools for interpreting task states

#### Workflow Sins

In order to make good on their guarantees, workflow management systems commit 3 sins:

1. By **compromising**, they limit what they let you do. They minimize the things that can go wrong by minimizing the things you can actually do.
2. The **cheat** is how we get past the compromise. These are workarounds to grant you functionalities which are not possible in the first place but will eventually create two systems \(original + workaround\) which are not aware of each other.
3. This leads to **corruption,** when the _cheat_ goes against the philosophy of the workflow manager. Its danger comes from the system allowing you to do something which it is not fully aware and doesn't understand completely. 

## XComs

#### Compromise

In **Airflow**, tasks communicate only by a state. But how can a system that only returns _success_ or _fail_ move data between tasks? It cheats by using XComs \(cross-communication\). Under the hood, XComs are just a way for tasks to impersonate an admin user and write arbitrary data to the database.

With XComs you can create arbitrary data dependencies across tasks, DAGs and time.

#### Corruption

The Airflow system that governs the workflow's dependencies has no way of seeing that XComs exist. Even worse, such dependencies are stored in an unmonitored part of Airflow's metadata database with no transparency or TTL.



## A Brief History of Workflows

### CRON \(1987\)

In the beginning there was CRON. There was a time trigger, no dependencies and people would often cheat on scheduling jobs on intervals. If you want something to run in sequence, you trigger the first for some epoch, the second to a _later_ epoch and you hope the first runs fast enough and that they work in the end.

`"0 0 * * *"` means at 00h00 every day.

### Oozie \(2011\), Azkaban \(2012\), Luigi \(2012\), Spark \(2014\), Airflow \(2015\)

**Oozie** is "Cron, but in Java and XML".   
**Azkaban** is "Cron, but with dependencies".   
**Luigi** is "Cron, but with dependencies based on side effects."  
**Spark** has a way of building computational graphs \(passes data instead of state\) has the same idea.  
**Airflow** is "Cron, but with a database".


### Idempotency

> **Idempotency** is the concept that if you run something many times it only causes an action once.

## Prefect

### Design

**Rich State** objects are the currency of the entire system. These track both the task state \(success, failure, pause\) and its result, but allows users to customize semantics and logic.

### Dask

**Dask** is an open-source library for parallel computing written in Python and it was core for the development of Prefect.









