---
permalink: /blog/:path/
layout: blog-subpages
title: Fighting Cyber Fraud with Real-Time Graph Analytics
description: What to Look for in a Graph Database for Real-Time Fraud Detection 
date: 2018-09-06
sitemap: true
header-image: "https://github.com/memgraph/blog/blob/master/images/fraud-1.jpg?raw=true"
category: [Business]
---

# Fighting Cyber Fraud with Real-Time Graph Analytics

Fighting Cyber Fraud with Real-Time Graph Analytics

The fast rise of digital payments and high profile data breaches is driving cyber fraud to record levels. According to the [Javelin’s Strategy & Research 2018 Identity Fraud Report](https://www.javelinstrategy.com/coverage-area/2018-identity-fraud-fraud-enters-new-era-complexity), a record 16.7 million consumers experienced fraud or identity theft in 2017 in the US alone, resulting in $16.8 billion in losses. Faced with this new reality and pressured by growing regulatory compliance and increasingly large financial and brand damage losses, organisations have no choice but to deploy more sophisticated fraud detection systems to stop fraudulent activities as they happen.

## Scale, Complexity and Speed

The growing scale and complexity of customer and transaction data, combined with the rise of real-time payments across the globe, are reshaping the payments industry as we know it.

1. According to the [World Payments Report 2017](http://www.worldpaymentsreport.com/) (WPR 2017), global digital payments volumes are predicted to increase by an average 10.9% through to 2020, reaching nearly 726 billion transactions and generating unprecedented amounts of data.

1. The shift to personalized and seamless shopping anytime, anywhere and on any device, is driving the explosion in alternative digital payment channels including mobile wallets, wearables and social media payment options. These are increasing the complexity of the payments ecosystem by allowing fraudsters to exploit loopholes and weaknesses of new and less monitored payment services such as [Zelle](https://www.nytimes.com/2018/04/22/business/zelle-banks-fraud.html).

1. With real-time payments becoming the norm around the world, fraud is expected to increase drastically. “*As banks begin to implement faster payment networks, they have to be ready to react to new cases of fraud in higher numbers*”, says Genevieve Gimbert, head of fraud management consulting for financial consultancy [Bank Innovation](https://bankinnovation.net/2018/02/with-realtime-payments-comes-realtime-fraud-and-banks-need-to-prepare/). She points to a recent example in the UK where financial services institutions saw a 300% increase in fraud after implementing a faster payments network.

These shifts within the payments ecosystem, combined with ever-adapting and more sophisticated tactics used by criminals, are rendering traditional rule-based fraud detection systems highly inefficient in detecting fraudulent transactions.

* Rule-based fraud detection systems analyse every transaction as a single data point and ignore the benefits of network intelligence. This results in a limited number of weak fraud signals.

* Rule-based engines are generic, rigid and cannot easily adapt to rapidly changing and dynamic environments.

* A fraud application has to analyse a transaction in a few hundred milliseconds, only a limited number of primitive rule-based operations can be run, which results in a high rate of false positives and false negatives.

* Rule-based engines require continuous manual monitoring from a large team, making the process costly and highly inefficient in today’s real-time business environment.

As a result of these inefficiencies, card issuer and banks are usually left bearing the losses related to fraud, hurting their profits and leaving customers dissatisfied.

## The Next Generation of Fraud Detection Systems

According to a recent [Gartner report](http://www.comdate.com.au/media/cleafy/Market%20Guide%20for%20Online%20Fraud%20Prevention%20-%202018.pdf), traditional rule-based and siloed fraud detection systems need to be quickly updated to adapt to more sophisticated and complex fraud attacks. In their report, they highlight seven key capabilities as best practices in financial fraud systems. These range from the most basic security measures, such as static data-based identification, to the more sophisticated ones, such as behaviour analytics and continuous risk assessment.

Whereas current fraud detection systems address the first three basic layers, graph databases enable organisations to unlock three of the remaining four layers to build the next generation of fraud detection systems.

![Gartner Fraud Detection Capability Model for Cross-Channel Fraud Prevention-*Source: Gartner (July 2017)*](https://cdn-images-1.medium.com/max/3440/1*xKU9CR2vM3J3jKhHoEy42w.png)*Gartner Fraud Detection Capability Model for Cross-Channel Fraud Prevention-*Source: Gartner (July 2017)**

In contrast to the one-dimensional traditional rule-based systems, graph databases allow for the aggregation of multiple data silos to integrate all customer interaction channels and easily map complex relationships between data points. This unlocks the capability for fine-grained, sophisticated and real-time entity relationships and behavioural analytics as well as for a 360-degree continuous risk assessment.

Adopting a holistic approach to fraud prevention, by combining the unique capabilities of graph databases with existing rule-based approaches, has the potential to dramatically improve accuracy, cost and efficiency of fraud detection systems. According to [Forbes](https://www.forbes.com/forbes/welcome/?toURL=https://www.forbes.com/sites/forbespr/2017/03/07/new-report-relationship-based-programs-improve-financial-institutions-transaction-monitoring-systems/&refURL=https://www.google.com/&referrer=https://www), a relationship approach to fraud detection using graph databases reduces false positives, improves false negative detection, eases investigations, and reduces overall fraud investigation costs.

## What to Look for in a Graph Database for Real-Time Fraud Detection

You will find that many current use-cases for graph databases in fraud detection revolve around visualisation and investigation as opposed to core processing functions. The reason being the limitations of first and second generation graph databases when it comes to delivering real-time performance on large amounts of streaming and historical data.

A modern graph database, suitable for real-time fraud detection should, therefore, be able to deliver sub-second execution on graphs with 100M+ edges/vertices. More specifically, it should be able to support:

1. **Read & Write Horizontal Scalability for Large Graphs** — Distributing your data and processing across multiple nodes and maintaining performance while your fraud dataset grows.

1. **Real-Time Distributed Graph Capabilities** — Combining real-time querying and updating for highly dynamic datasets.

1. **Multi-Depth Graph Queries** — Achieving sub-second execution of complex queries with 3+ depths to uncover complex relationships.

1. **Machine Learning Capabilities**: Empowering your data science teams to seamlessly build sophisticated graph enhanced machine learning fraud applications.

## About Memgraph

Memgraph is the world’s first native in-memory distributed Graph Database platform powering real-time graph analytics and transaction at web scale. Memgraph is engineered from the ground up to bring startups and enterprise the speed, scale, simplicity and security required to build the next generation of intelligent applications powered by real-time connected data.

Learn more at [www.memgraph.com](https://memgraph.com/)

Visit [www.memgraph.com/docs](https://memgraph.com/docs/) for:

* Documentation

* Download Memgraph

Join our community for free support: [www.memgraph.com/community](https://memgraph.com/community/)

Follow us on Twitter: @memgraphdb
