---
layout: post
title:  "Apache Kafka Producer API"
date:   2018-08-13 00:00:00 +0530
categories: kafka
author: "abhishek shah"
tags: kafka
---


In this article, we will look at the [Producer API](https://kafka.apache.org/20/javadoc/index.html?org/apache/kafka/clients/producer/KafkaProducer.html) of Apache Kafka.
These API allows creation of Kafka Producer and expose various other options to configure our Producer to our needs. 

## Maven Dependency

Add the following dependency in your `pom.xml`. 

```
<dependency>
    <groupId>org.apache.kafka</groupId>
    <artifactId>kafka-clients</artifactId>
    <version>2.0.0</version>
</dependency>
```

Kafka clients include all the jars for Producer and Consumer. Same maven dependency can be used while writing Consumer clients using Consumer API.

## Configurations

All the configuration data is written in `java.util.Dictionary.Hashtable.Properties` and passed as contructor parameter while instantiating a `Producer`.

Sample code snippet illustrating few configurations for the Producer :

```
Properties props = new Properties();
props.put("bootstrap.servers", "localhost:9092");
props.put("acks", "all");
props.put("retries", 0);
props.put("batch.size", 16384);
props.put("linger.ms", 1);
props.put("buffer.memory", 33554432);
props.put("key.serializer", "org.apache.kafka.common.serialization.StringSerializer");
props.put("value.serializer", "org.apache.kafka.common.serialization.StringSerializer");
```

