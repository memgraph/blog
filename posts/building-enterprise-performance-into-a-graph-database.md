---
permalink: /blog/:path/
layout: blog-subpages
title: Building Enterprise Performance Into a Graph Database
description: The next generation of graph databases focus on speed and scalability
date: 2018-12-03
sitemap: true
header-image: "https://github.com/memgraph/blog/blob/master/images/enterprise-performance.jpg?raw=true"
category: [Business]
---

# Building Enterprise Performance Into a Graph Database

The next generation of graph databases focus on speed and scalability

This article introduces some of the key concepts of graph databases and their use cases, and explains how speed and scalability are critical to the next generation of graph databases if they are to tackle enterprise-level use cases and deliver high-performance machine learning.

![[“B of the Bang”](https://en.wikipedia.org/wiki/B_of_the_Bang) [Image by Duncan Hull on Flickr ](https://www.flickr.com/photos/dullhunk/162967260)(CC BY 2.0)](https://cdn-images-1.medium.com/max/2048/1*gmhrAe_RR9wBZuPuCU4XeA.jpeg)*[“B of the Bang”](https://en.wikipedia.org/wiki/B_of_the_Bang) [Image by Duncan Hull on Flickr ](https://www.flickr.com/photos/dullhunk/162967260)(CC BY 2.0)*

## Introduction

The NoSQL (“not only SQL”) space presents a number of different data models and database systems which can be more suitable than relational systems for particular types of data and use cases. Among these are [graph databases](https://en.wikipedia.org/wiki/Graph_database), in which data is formed of a collection of nodes, the edges that connect them, and a set of properties. Nodes are entities, such as people or items, edges represent the relationships between the nodes, and properties are information about the nodes and edges.

The underlying data model of common graph databases is either an RDF triple store or a labeled property graph, and you can find out more in an [article here on DZone](https://dzone.com/articles/rdf-triple-stores-vs-labeled-property-graphs-whats), which describes the differences between the two approaches. RDF databases and property graphs might be native products, but they can also be built on top of other database types, such as a NoSQL database like Hadoop or Cassandra.

![](https://cdn-images-1.medium.com/max/2000/0*n7uSq11gz6tVN2Qw.png)

*A property graph, illustrating nodes, properties, and edges from Wikimedia Commons [CC0].*

## Graph Database Use Case

Interconnection is the key differentiator for graph databases. The edges represent relationships more directly than other database systems and can reveal patterns about the connections between data without the use of foreign keys or MapReduce.Graph databases are excellent for navigating a relationship structure quickly — for example, the number of steps needed to get from one node to another.

Wherever you use a relational database, you can use a graph database; to get the most from it, you need to structure your data to take advantage of the relationships. Typical use cases include recommendation engines, fraud detection, rules engines, text processing and text analytics in media and publishing, and discovery projects that explore a “data lake” to uncover previously unseen relationships between entities — an approach which has applications in security and research. A great example is the [recent use of a graph database](https://neo4j.com/blog/analyzing-panama-papers-neo4j/) by the International Consortium of Investigative Journalists (ICIJ). Their analysis of the [“Panama Papers”](https://www.theguardian.com/news/series/panama-papers) revealed highly interconnected networks of offshore tax structures. This latter use case is sometimes referred to as cognitive computing since it uses data mining and pattern recognition.

In the current age of interconnected data, graph databases are showing increased uptake within large online systems and have become the fastest growing category of NoSQL databases. A recent report from IBM surveyed over 1,300 entrepreneurs and developers worldwide about their current and planned use for graph databases, and 43% of respondents were reported to be using or planning to use graph technology. New graph databases are emerging, and existing vendors are adding graph capabilities to their current solutions. Some of the key players include Neo4J, TitanDB, Blazegraph, HypergraphDB, OrientDB, IBM Graph, and Amazon’s Neptune.

## Historical Disadvantages of Graph Databases

Neither graph computing as a field nor graph databases as a technology are particularly new; both have been around for at least 30 years. Given the exciting possibilities that they offer, you might wonder why they have not been more widely adopted before now. Early adopters of the first generation of graph databases have reported some issues, which include the following:

* Some disk-based graph databases are unable to handle a constant stream of updates and stay sufficiently responsive to provide high performance on queries.

* Not all traditional graph databases are suitable for transactions and some are more focused on providing analytical frameworks.

* It is difficult to determine how to shard or partition data to get the best performance out of a graph database system as it scales.

* It can be difficult to know how to model domain data onto a graph since it requires a specialist skill set — that is, proficiency in defining an ontology.

* When the data model is defined, it is essential that data loaded into the graph database is consistent with that ontology. Graph databases, like other NoSQL databases, delegate adherence to a schema to the application system and do not enforce a schema, so the data entering the database needs guaranteed consistency up front.

## Introducing Memgraph

In the rest of this article, I am going to look at how the next generation of graph databases is tackling some of the drawbacks of the trailblazing, early graph database systems using [Memgraph](https://memgraph.com/) as an example.

Memgraph is a next-generation graph database system which sits in-memory and has been optimized for real-time transactional use cases, aiming to handle highly concurrent operational and analytical workloads within the enterprise space.

The [Memgraph architecture](https://blog.memgraph.com/architecture-of-a-modern-graph-database-a-look-under-the-memgraphs-hood-89e6a8b41459?gi=ee5f2f63e9f8) sets out to eliminate the issues leveled at earlier generation graph databases by prioritizing“four pillars:” speed, scale, simplicity, and security. Let’s look at the first two of these in some more detail.

## Speed

Memgraph supports real-time use cases for enterprises with colossal amounts of data. The team has achieved high-performance benchmarks, demonstrating low latency and high throughput on high volumes of read and write queries (with ACID transactions) on real-world scale datasets. For reasons of space and timing, I can’t share the benchmark results here, but they will be published within the next few weeks, and I can say that they stack up well when compared against incumbent graph database systems.

Memgraph is capable of loading tens of thousands of nodes/edges per second on a single machine. The database achieves high throughput by using highly concurrent (and sometimes seven lock-free) data structures and multi-version concurrency control (MVCC). This ensures that writes never block reads and vice versa. Traditional databases manage concurrency with global locks, which results in some processes blocking others until they complete and release the lock. Memgraph’s implementation of MVCC provides snapshot isolation level which allows better performance than serializability while avoiding most of the concurrency anomalies.

Additionally, Memgraph uses a highly concurrent skip list for indexing purposes. Skip lists represent an efficient technique for searching and manipulating data, delivering concurrency and performance benefits beyond those seen for other graph databases and disk-based databases which use B-Trees to store indexes.

## Scale

On a single machine, Memgraph scales up to the size of a main memory or disk space where properties could be stored on disk. In a distributed system, the graph is automatically repartitioned in the background to improve query execution time and scalability.

Memgraph features a distributed query planning and execution engine. Each plan is divided into two; a plan that will be executed on the machine where the query is received and a plan that will be executed on the other machines. Nodes are allowed to exchange data during the execution process to maximize performance.

In its distributed environment, Memgraph offers a custom and high-performance implementation of common graph algorithms such as level-synchronous parallel breadth-first search(BFS), which in many cases outperforms a single machine BFS.

To improve query performance and scalability, Memgraph runs a dynamic graph partitioning algorithm in the background to minimize the number of crossing edges between machines whilst keeping the cluster optimally balanced.

## AI and Analytics

Memgraph can integrate with the most popular machine learning tool such as TensorFlow and PyTorch, readily allowing the stored data to be used to train a model. To facilitate training and production deployment across the enterprise space, theMemgraph client will be wrapped in a TensorFlow operation to allow direct querying into Memgraph. A similar approach will be used for PyTorch and ONNX deep learning, with the goal of making the data within Memgraph easily accessible to data scientists — for example, those developing systems for fraud detection in finance and retail.

Thanks to its in-memory architecture, Memgraph is also suitable for running analytical workloads. Algorithms like BFSand Weighted Shortest Path constitute the core of the query execution stack.

## Summary

Graph databases are increasing in popularity, and though the next-generation graph technology is still very much under development, it promises to eliminate some of the current disadvantages and deliver a powerful backend for use in enterprise and beyond.

If you have specific questions about Memgraph, you may find them answered by their [FAQ](https://memgraph.com/docs/memgraph/v0.14.1/faq/), but you can find out more by [contacting the team](https://memgraph.com/contact/).

*This article initially featured in the [DZone Guide to Databases: Relational and Beyond](https://dzone.com/guides/databases-relational-and-beyond).*
