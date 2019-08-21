---
permalink: /blog/:path/
layout: blog-subpages
title: Architecture of a Modern Graph Database — A Look Under the Memgraph’s Hood
description: The next generation of graph databases focus on speed and scalability
date: 2018-02-19
sitemap: true
header-image: "https://github.com/memgraph/blog/blob/master/images/architecture.jpg?raw=true"
category: [Business]
---


# Architecture of a Modern Graph Database — A Look Under the Memgraph’s Hood

If you’ve read Dominik’s Hello Memgraph post, you already know what we’re all about, but today, I’d like to give you a little bit more about how Memgraph operates under the hood.

If you’ve missed it, Memgraph is an in-memory graph database system optimized for real-time transactional use-cases. In the next section, I’ll explain the underlying data model known as the property graph in case you’re not familiar with it, but feel free to dive straight into the action to learn more about the architecture if you are.

## The Property Graph Model

A property graph consists of nodes and relationships (mathematicians call them vertices and edges). They are the key concepts because they define the core structure of the data showing how things relate to each other. Nodes can hold any number of properties (key-value pairs) and can be assigned labels which represent their roles within a particular domain. An example would be a node with a label *Person* and properties such as *name* or *age*. Relationships are directed connections between two nodes and contain properties (just like nodes) and a single edge type. Relationships usually have properties such as *time*, *distance*, *cost*, *rating* or *weights* which are also stored as key-value pairs. Figure 1 shows a basic example of a property graph with four nodes and three edges.

![Figure 1.](https://cdn-images-1.medium.com/max/2208/1*KakNPYHxh2TXoGuuFvqeog.png)*Figure 1.*

There are a couple of alternatives to the property graph.

Probably the most common one is the [RDF graph data model](https://en.wikipedia.org/wiki/Resource_Description_Framework). Compared to the RDF graph, property graph is more powerful. It can always be set up to mimic the RDF structure, but if you don’t require traversals on all of the objects you can store them as properties on vertices and edges which is more memory efficient and less time-consuming during the traversals.

Another alternative would be to store nodes and edges in a relational database within two separated tables. The downside of this approach is that the graph has to be constructed during the run-time by performing joins over those two tables which is not time efficient.

## Memgraph’s Architecture

Figure 2 shows a high-level overview of Memgraph’s architecture. The point of contact between the client and Memgraph is the communication server which supports the Bolt protocol. Bolt is a binary protocol designed to enable data-efficient communication between clients and graph DB engines. More about the protocol can be found on [https://boltprotocol.org](https://boltprotocol.org).

![Memgraph’s Architecture Overview](https://cdn-images-1.medium.com/max/2000/1*28aTfiofV5ZqrYQixvKD0A.png)*Memgraph’s Architecture Overview*

An early design decision was to find a way to integrate with the existing ecosystem of tools and applications seamlessly. While building Memgraph, we’ve seen that the best way to integrate with the ever-growing graph ecosystem is to support the openCypher query language. It’s a declarative query language specifically designed to query graphs. You can learn more about openCypher at [http://opencypher.org](http://opencypher.org).

When the openCypher query arrives, the communication server routes it into the query engine. Before execution, the query engine generates an optimal execution plan based on the underlying data statistics and the available indexes. When the query plan is ready, it’s executed against the in-memory graph storage. Even though the planning time is usually a small chunk of the whole query processing time, the query engine also contains cache for the execution plans because it could be beneficiary in high-throughput scenarios.

The in-memory graph engine is the core part of the whole system. Memgraph achieves high throughput by using highly concurrent (and sometimes even lock-free) data structures and multi-version concurrency control ([MVCC](https://en.wikipedia.org/wiki/Multiversion_concurrency_control)). In Memgraph, writes never block reads and vice versa. Traditional databases manage concurrency with global locks, which results in some processes blocking others until they complete and release the lock. Memgraph’s implementation of MVCC provides snapshot isolation level which is an common choice between the speed and isolation guarantees. Furthermore, Memgraph uses a highly concurrent skip list for indexing. Skip lists represent an efficient technique for searching and manipulating data, delivering concurrency and performance benefits inexistent in other graph databases and other disk-based databases that use B-Trees to store indexes.

Memgraph also provides durability guarantees, the combination of snapshotting and write-ahead logging (WAL) ensures that data won’t be lost after an underlying system failure. Later this year we also plan to provide a way for manually creating snapshots for backup purposes and manual or automatically scheduled backups to S3 and similar.

## A Look Into the Future

Apart from the features mentioned above, many others will come in the following months. Memgraph’s team is tackling the problem of efficient distributed graph storage and efficient execution of openCypher queries across the cluster.

Have any feedback or suggestions? Leave a comment or dive into the discussion by join our public Slack channel on [https://memgraph.com/slack](https://memgraph.com/slack).

Let me know what should I write about next or what part of Memgraph you’d liked I explained in more details.

I’d also encourage you to follow us on our journey and join our awesome community. You can download Memgraph at [https://memgraph.com/download](https://memgraph.com/download).
