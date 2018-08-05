---
layout: post
title:  "Apache Kafka"
date:   2018-08-05 00:00:00 +0530
categories: kafka
author: "abhishek shah"
---

## 1. Overview

In Kafka, the system of Publish and Subscribe has been brought forward in a more resilient and fault tolerant way. In Kafka entity producing the event/message are called Publisher and entity consuming the message are Consumer. The Publisher has their own set of APIs known as Publisher APIs, and Consumer has their own set of APIs, known as Consumer APIs.

The messages are categorized by the Topics in Kafka. Each Topic has a unique name. The Publisher should know the name of the Topic they want to publish their messages to, similarly, the Consumers should also know the names of the Topics they want to read the message from.

## 2. Kafka Brokers

Brokers are where the messages and topics reside. A broker can have a lot of topics in it. Precisely, brokers are daemon process that runs in the host machines. Each broker has access to the file system on the host machine where they maintain all the messages and topics. Since brokers are daemon process there can be multiple running broker on the host with each having their own separate file system to maintain the message log and separate configurations.

## 3. Kafka Clusters

A collection of Kafka brokers is called Kafka Cluster. Multiple brokers can run in a single system or can span across multiple hosts. The grouping of brokers is called Cluster.

Usually, in a distributed system, the workers elect the controller node. Mostly, the longest running working node is selected as controller node. Then tasks of the controller node are :

* Maintain the availability of the nodes.
* Track the progress of the tasks assigned to the follower nodes.


























