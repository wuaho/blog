---
title: "The ABCs of NoSQL: lessons from \"NoSQL Distilled\""
date: "2024-09-21 07:43:53 +0200"
tags: ["bases de datos", "libros"]
thumbnail-img: "/assets/img/posts/nosql-distilled/cover.jpeg"
share-img: "/assets/img/posts/nosql-distilled/cover.jpeg"
share-description: "Why did I read this book?\nSome time ago, I asked my first manager as a software engineer, Andrea Carrozo, to recommend me some books to healp me learn more about databases. Andrea immediately suggested NoSQL Distilled: A Brief Guide to the Emerging W..."
---
## Why did I read this book?

Some time ago, I asked my first manager as a software engineer, [*Andrea Carrozo*](https://www.linkedin.com/in/andreacarrozzo/), to recommend me some books to healp me learn more about databases. Andrea immediately suggested *NoSQL Distilled: A Brief Guide to the Emerging World of Polyglot Persistence* by Pramod J. Sadalage and Martin Fowler. As I write about it now, I revisited Andrea’s personal website ([jonnyjava.net](https://www.jonnyjava.net/)) and noticed that he had even listed it as the first book to read in his recommended [databases learning path](https://www.jonnyjava.net/learning-databases/).

Although I’ve read several programming books, I hadn’t yet explored one focused exclusively on NoSQL databases. So, it felt like the right time to visit my local bookstore and pick up a fresh copy.

I'll be going through the book chapter by chapter, sharing the key insights and lessons I learn along the way.

<div class="callout"><span class="callout-emoji">📖</span><div class="callout-text">Link to the book: <a href="https://martinfowler.com/books/nosql.html" target="_blank" rel="noopener">https://martinfowler.com/books/nosql.html</a></div></div>

---

## Chapter 1: Relational Databases and NoSQL

### Relational Databases:

**What they solve:**

* **Getting at Persistent Data:** they allow more flexibility and speed than file systems when getting small bits of information from big amounts of data.
    
* **They solve concurrency:** the problem of having many people looking at the same data at once and even modifying it. Relational databases help handle this by controlling all access to their data through transactions. This isn't the cure, you will also get transactional errors. But with transactions, you can make a change, and if an error occurs during the processing of the change you can roll back the transaction to clean things up.
    
* **Integration:** using one database for multiple applications (integration database)
    
* **A Standard Model:** the core mechanisms of relational databases is the same. allowing developers to learn it an apply to multiple projects.
    

**Impedance Mismatch from Relational Databases**

Given that the relational data model organizes data into a structure of tables and rows, if you want to use a richer data structure, you have to translate it to that representation. This difference between the relational model and the in-memory data structure is called impedance mismatch and it has been frustrating for application developers.

### NoSQL

**How it appeared**

1. ➡️ 1990s dot-com bubble
    
2. ➡️ Growth in data from websites
    
3. ➡️ Need more computing resources: use clusters
    
4. ➡️ Relational databases are not designed to be run efficiently on clusters: mismatch
    
5. ➡️ Organizations consider an alternative route to data storage
    
6. ➡️ Google and Amazon (who were the forefront of running large clusters) produce highly influential papers: BigTable from google and Dynamo from Amazon.
    

**There is no proper definition for NoSQL databases but rather some common characteristics:**

* They don't use the relational model
    
    * They run well on clusters
        
* They are generally open-source projects
    
* They are schemaless, allowing you to freely add fields to database records without having to define any changes in structure first.
    

**The two primary reasons to consider NoSQL are:**

* Handle data access with size and performance that demand a cluster.
    
* Improve the productivity of application development by using a more convenient data interaction style.
    

### Extra

> In order to make the polyglot persistence world to work, organizations need to shift from integration databases to application databases.

> We don't generally consider NoSQL databases a good choice for integration databases.

## Chapter 2: Aggregate Data Models

<mark>A </mark> **<mark>data model</mark>** <mark>is the model through which we perceive and manipulate our data.</mark>

> Agregate orientation takes a different approach. It recognizes that often, you want to operate on data in units that have a more complex structure than a set of tuples.

➡️ **An aggregate is a collection of data that we interact with as a unit.**

> If we're running on a cluster, we need to minimize how many nodes we need to query when we are gathering data.

➡️ **Aggregates make it easier for the database to manage data storage over clusters.**

> It's often said that NoSQL databases don't support ACID transactions and thus sacrifice consistency. This is a rather sweeping simplification. In general, it's true that aggregate-oriented databases don't have ACID transactions that span multiple aggregates. Instead, they support atomic manipulation of a single aggregate at a time. This means that if we need to manipulate multiple aggregates in an atomic way, we have to manage that ourselves in the application code.

➡️ **Aggregates form the boundaries for ACID operations with the database.**

In practice, we find that most of the time we are able to keep our atomicity needs to within a single aggregate; indeed, that's part of the consideration for deciding how to divide up our data into aggregates.

> With key-value databases, we expect to mostly look-up aggregates using a key. With document databases, we mostly expect to submit some form of query based on the internal structure of the document; this might be a key, but it's more likely to be something else.

➕ the main difference between key-value and document databases is that in the first one, the aggregate is opaque to the database - just some big blob of mostly meaningless bits. In contrast, a document database is able to see a structure in the aggregate.

> Column-family models divide the aggregate into column families, allowing the database to treat them as units of data within the row aggregate.

## Chapter 3: More about Data Models

**Operating across multiple aggregates and managing lots of relations on NoSQL databases based on aggregates is awkward.**

* If you want to have data based on lots of relationships, you should prefer a relational database. But relational databases aren't that excellent with complex relationships either.
    
* **Graph databases** are good doing this. They are based on an opposite model from relational databases: small records with complex interconnections. A graph database allows you to query that network with query operations designed with this kind of graph in mind so making traversal along the relationships is cheap.
    
* Agregate-oriented databases often compute **materialized views** to provide data organized differently from their primary aggregates. This is often done with map-reduce computations.
    

### Schemaless databases

* A common factor between all the forms of NoSQL databases is that they are **schemaless**.
    
* This means that when you want to store data, you dont need to define schema for it. This allows you to easily change your data storage as you learn more about your project.
    
* It also facilitates handling **non-uniform data**: data where each record has a different set of fields.
    
* Schemaless databases althought appealing, they bring problems of their own like:
    
    * Whenever we write a program that accesses data, the program relies on some form of implicit schema and assume something about the type of data stored within that field. So, however schemaless our database is, there is an implicit schema. This **implicit schema** is a set of assumptions about the data's structure in the code to manipulate the data.
        
        * With an implicit schema it means that to understand what data there is present, you have to dig into the application code.
            
        * The database remains ignorant of the schema and it can't use it to help it decide how to store and retrieve data.
            
* So, although NoSQL fans often criticize relational schemas for having to be defined front and being inflexible, that's not really true. Relational schemas can be changed at any time with standard SQL commands.
    

## Chapter 4: Distribution Models

These are the following ways of running a database on a cluster of servers:

* **Single Server**. The simplest distribution option and the one the authors of the book would most often recommend - no distribution at all. It consists of just running the database on a single machine that handles all the reads and writes to the data store. It is prefered because it eliminates all the complexities that all the other options introduce.
    
* **Sharding**. It distributes different data across multiple servers, so each server acts as the single source for a subset of data.
    
* **Replication**. Copies data across multiple servers, so each bit of data can be found in multiple places. Replication comes in two forms:
    
    * **Master-Slave Replication**. Makes one node the authoritative copy that handles writes while slaves synchronize with the master and may handle reads.
        
    * **Peer-to-Peer Replication**. Allows writes to any node and the nodes coordinate to syncrhonize their copies of the data.
        
    * Master-slave replication reduces the chance of update conflicts, but peer-to-peer replication avoids loading all writes onto a singles point of failure.
        
* **Combining Sharding and Replication**. These strategies can be combined to gain benefits from both of them like master-slave replication and sharding or peer-to-peer replication with sharding which is common strategy for column-family databases.
    

## Chapter 5: Consistency

**<mark>One of the biggest changes from a centralized relational database to a cluster-oriented NoSQL database is in how you think about consistency. Relational databases try to exhibit strong consistency by avoiding all the various incosistencies introduced below.</mark>**

> **Write-write conflicts** occur when two clients try to write the same data at the same time. **Read-write conflicts** occur when one client reads incosistent data in the middle of another client's write.

**Approaches for maintaining consistency in the face of concurrency as often described as pessimistic or optimistic:**

* **Pessimistic approaches** works by preventing conflicts from occurring. It lock data records to prevent conflicts.
    
* **Optimistic approaches** lets conflicts occur, but detects them and takes action to sort them out.
    

> Often, when people first encounter these issues, their reaction is to prefer pessimistic concurrency because they are determined toa void conflicts. While in some cases this is the right answer, there is always a **tradeoff**. Concurrent programming involves a fundamental tradeoff between **safety** (avoiding errors such as update conflicts) and **liveness** (responding quickly to clients). **Pessimistic approaches often severely degrade the responsiveness of a system to the degree that it becomes unfit for its purpose.**

Some claims about NoSQL databases and the consistency on its updates.

> A common claim we hear is that NoSQL databases don't support transactions and thus can't be consistent. Such claim is mostly wrong because it glosses over lots of important details. Out first clarification is that any statement about lack of transactions usually only applies to some NoSQL databases, in particular the aggregate-oriented ones. In contrast, graph databases tend to support ACID transactions just the same as relational databases.
> 
> Secondly, aggregate-oriented databases do support atomic updates, but only within a single aggregate. This means that you will have logical consistency within an aggregate but not between aggregates.

**Data that is out of date is generally referred to as stale.**

> The basic statement of the **CAP theorem** is that, **given the three properties of Consistency, Availability, and Partition tolerance, you can only get two.**
> 
> The CAP theorem states that if you get a network partition, you have to trade off availability versus consistency.

* Distributed systems see read-write conflicts due to some nodes having received updates while other nodes have not. Eventual consistency means that at some point the system will become consistent once all the writes have propagated to all the nodes.
    
* To get good consistency, you need to involve many nodes in data operations, but this increases latency. So you often have to trade off consistency versus latency.
    
* Durability can also be traded off against latency, particularly if you want to survive failures with replicated data.
    
* You do not need to contact all replicants to preserver strong consistency with replication. You just need a large enough quorum.
    

## Chapter 6: Version Stamps

**<mark>Version stamps help you drect concurrency conflicts. When you read and then update data, you can check the version stamp to ensure nobody updates the data beteween your read and write.</mark>**

**Implementations of version stamps are:**

* Counters
    
* GUIDs
    
* Content hashes
    
* Timestamps
    
* A combination of these
    

Timestamps are simple but problematic because is difficult to ensure that all the nodes have a consistent notion of time. A counter is usually better than timestamps.

The most common approach use by peer-to-peer NoSQL systems is a vector stamp. It would look like \[blue: 43, green: 54, black: 5\]

## Chapter 7: Map-Reduce

**<mark>A map-reduce is a pattern to allow computations to be parallelized over a cluster. It is composes of a map function, which performs filtering and sorting, and a reduce method, which performs a summary operation.</mark>**

* The **map task** reads data from an aggregate and summarizes it to relevant **key-value pairs**. Maps only read a single record at a time and can thus be parallelized and run on the node that stores the record.
    
* **Reduce tasks** take **many values for a single key output** from map tasks and **summarize them into a single output**. Each reducer operates on the result of a single key, so it can be parallelized by key.
    
* Reducers that have the same form for input and output can be combined into **pipelines**. This improves parallelism and reduces the amount of data to be transferred.
    
* **Map-reduce operations can be composed into pipelines where the output of one reduce is the input to another operation’s map.**
    
* If the result of a map-reduce computation is widely used, it can be stored as a **materialized view**.
    
    * **Materialized views** can be updated through incremental map-reduce operations that only compute changes to the view instead of computing again everything from scratch.
        

---

<div class="callout"><span class="callout-emoji">📣</span><div class="callout-text">This article is getting too long, so I’ll cover the second part in a separate post.</div></div>
