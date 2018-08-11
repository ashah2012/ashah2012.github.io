---
layout: post
title:  "Apache Kafka Basics"
date:   2018-08-05 00:00:00 +0530
categories: kafka
author: "abhishek shah"
---

## 1. Overview

In [Apache Kafka](https://kafka.apache.org), the system of Publish and Subscribe has been brought forward in a more resilient and fault tolerant way. In Kafka entity producing the event/message are called Producer and entity consuming the message are Consumer. The Publisher has their own set of APIs known as [Producer API](https://kafka.apache.org/documentation.html#producerapi), and Consumer has their own set of APIs, known as [Consumer API](https://kafka.apache.org/documentation.html#consumerapi).

The messages are categorized by the *Topics in Kafka*. Each Topic has a unique name. The Publisher should know the name of the Topic they want to publish their messages to, similarly, the Consumers should also know the names of the Topics they want to read the message from.

## 2. Kafka Brokers

Brokers are where the messages and topics reside. A broker can have a lot of topics in it. Precisely, brokers are daemon process that runs in the host machines. Each broker has access to the file system on the host machine where they maintain all the messages and topics. Since brokers are daemon process there can be multiple running broker on the host with each having their own separate file system to maintain the message log and separate configurations.

## 3. Kafka Clusters

A collection of Kafka brokers is called Kafka Cluster. Multiple brokers can run in a single system or can span across multiple hosts. The grouping of brokers is called Cluster.

Usually, in a distributed system, the workers elect the controller node. Mostly, the longest running working node is selected as controller node. Then tasks of the controller node are :

* Maintain the availability of the nodes.
* Track the progress of the tasks assigned to the follower nodes.

## 4. Fault Tolerance - Replication Factor

When a task is assigned to node / broker, they select some backup peers. These backup peers take up the task from leader when they go down. The number of the backup peers assigned depends upon the *Replication Factor*.

*Setting up Quorum* - The controller assigns the task to a leader which can form a *quorum*. If a leader fails to form a quorum then controller assigns the task to some other leader who can form a quorum. A quorum is formed when a leader is assigned a task and the follower nodes acknowledges the fact that when leader goes down they will have to perform the task in it's place. This arragement is what we refer to as *setting up a quorum*.

## 5. Topics

Topics are a logical entity. An abstraction of the categorization of the messages. Messages are stored in *topics* in time ordered sequence. Each message consists of :

* Time Stamp.
* Referenceable Id.
* Binary Data / payload.

*Message Offset* - These are identifiers which tell the *Kafka Consumer* what message to read next. Offsets are actually the message id. The offsets are maintained by Kafka Consumers. When the consumer has read the last message, it keeps a note of the last offset. When a new message arrives an event notifies the consumer.

*Message Retention Policy* - Each topic is configurable, as for how long they want to preserve a message. By default, it is one hundred and sixty-eight hours or a week. It is configurable and can be found in `server.properties`  file. Retention period depends on the brokers.

Also, each topic can span over multiple brokers. This is achieved using the partitions.

## 6. Partitions

Topics are represented by one or more physical log files known as *Partitions*. Partition is stored as commit logs in the file system.

The number of partitions that can be created for a partitcular topic is configurable. Each topic must have atleast one partition.

Each partition must reside on one node only. So we should decide the number of partitions clearly before creating a topic. Partitions cannot span over multiple nodes / brokers. If the topic has multiple partitions the message are distributed to partitions exclusively. Each partiton receives a different message. The message are distributed in Round Robin fashion unless specified. 

## 7. Conclusion

In this article, I wanted to stay away from code snippets and just concentrate on the theory part. In the next article, I will write about how the Producers connect to Zookeeper and Broker to create a message and explore Producer API to create a message from a Java program.

 























































