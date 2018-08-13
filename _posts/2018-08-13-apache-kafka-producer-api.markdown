---
layout: post
title:  "Apache Kafka Producer API"
date:   2018-08-013 00:00:00 +0530
categories: kafka
author: "abhishek shah"
---

## Overview

In this article, we will look at the [Producer API](https://kafka.apache.org/20/javadoc/index.html?org/apache/kafka/clients/producer/KafkaProducer.html) of Apache Kafka.
These API allows creation of Kafka Producer and expose various other options to configure our Producer to our needs. 

## Maven Dependency

Add the following dependency in your ` pom.xml `. 

```
<dependency>
    <groupId>org.apache.kafka</groupId>
    <artifactId>kafka-clients</artifactId>
    <version>2.0.0</version>
</dependency>
```
