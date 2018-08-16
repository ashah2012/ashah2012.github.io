---
layout: post
title:  "Running Apache Kafka"
date:   2018-08-16 00:00:00 +0530
categories: kafka
author: "abhishek shah"
tags: kafka
---

In this short article, I want to list down all the commands which are used to run a **Kafka Broker** in our local system. 

## Getting Apache Kafka

Head over to [download page](https://kafka.apache.org/downloads) of Apache Kafka and download the latest stable release.

I would advise running on 64 bit operating systems, I once faced a lot of trouble running it in 32 bit operating system, only to find it running smoothly on former. I still don't know the reason behind it. 

## Changing Configurations

In the config folder we will find all the properties file. We are interested in two of them right now : 

* zookeeper.properties  
* server.properties 

In *zookeeper.properties*, change the following :

* dataDir=D:/kafka/tmp/zookeeper 

And, in *server.properties*, change the following :

* log.dirs=D:/kafka/tmp/kafka-logs

In both of the properties file I have changed the log directory, to my desired location.

## Starting Apache Zookeeper

Before running a Kafka Broker, we are aware that we need to start zookeeper. Apache Kafka provides a build in script for starting zookeeper. Navigate to `kafka/bin/windows` if running in windows system. Else navigate to `kafka/bin/`.

We will run the following command to start zookeeper : 

```bash
>zookeeper-server-start.bat ../../config/zookeeper.properties
```

If in Unix/Linux systems : 

```bash
>zookeeper-server-start.sh config/zookeeper.properties
```
Zookeeper will start running at port 2181. We can change the port on which zookeeper runs by modifying the `clientPort` field in `zookeeper.properties`.

## Running Kafka Broker

Once zookeeper is running we can start our single broker (I will run multiple broker some other time and will definitely write about it).

To start Kafka Broker run the following command : 

```bash
>kafka-server-start.bat ../../config/server.properties
```

If in Unix/Linux systems : 
```bash
>kafka-server-start.sh config/server.properties
```

Kafka Server will start running in port 9092. I did not find any property in `server.properties` file where it says the server will run at port 9092. I guess will have to dig it out.

Now, we have our Kafka Server up and running.

## Listing all the Topics in our Kafka Broker

Run the following command to list all the existing *Kafka topics* in our broker : 

```bash
>kafka-topics.bat --zookeeper localhost:2181 --list
```
This will list all the topics in our broker. We can wish to use one of the existing topics. But let us do create a new topic.

## Creating a new Kafka Topic

Creating Kafka topic needs a lot of expertise. Well, there is no expertise required to just run topic creation script. But the expertise is required in the areas such as :

* What should be the number of partition ? Answer to this question also depends upon how many broker we have ? 
* What should be the replication factor ? 

One should keep in mind these things when creating a topic. Also, I will try to write separate articles trying to answer these questions.

But for the sake of brevity, we will create a topic with one partition and one replication factor.

Run the following command to create a topic :

```bash
>kafka-topics.bat --zookeeper localhost:2181 --create --replication-factor 1 --partitions 1 --topic test
```
Understanding the parameters : 

* `--zookeeper localhost:2181` - host of the running zookeeper.
* `--create` - arugment to create a topic.
* `--replicatio-factor 1` - this topic should have only one replication factor.
* `--partitions 1` - the number of partitions this message will be logically divided into.
* `--topic test` - name of the topic, which in this case is `test`.

We can list the topics again to verify the creation of the new topic.

If you wish to run in Unix/Linux system replace the `.bat` with `.sh`.

## Running Kafka Clients

Apache Kafka also comes with Kafka clients i.e Producer and Consumer. You can also check  Java implementation of the Kafka Clients in my other articles.

### Kafka Client Producer

To run Kafka client Producer, run the following command :

```bash
>kafka-console-producer.bat --broker-list localhost:9092 --topic test
```

### Kafka Client Consumer

To run Kafka client Consumer, run the following command :

```bash
>kafka-console-consumer.bat --bootstrap-server localhost:9092 --topic test --from-beginning
```


