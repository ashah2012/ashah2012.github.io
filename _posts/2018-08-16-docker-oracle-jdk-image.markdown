---
layout: post
title:  "Running Oracle Jdk in Docker Container"
date:   2018-08-16 00:00:00 +0530
categories: docker
author: "abhishek shah"
tags: docker
---

My learning of [Docker](https://www.docker.com/) continues and I want to create as many Docker images till I get really comfortable with them. I will write a Dockerfile to
create a Oracle Jdk 8 image. I usually refer [Arun Gupta](https://github.com/arun-gupta)'s [blog](http://blog.arungupta.me/) for my Docker learnings.

In my previous [article](https://abhishek-shah.org/docker/2018/08/13/docker-mysql-image.html), I ran MySql in a container.

## Creating a Dockerfile

Open up your favourite text editor and copy the following lines : 

```bash
FROM ubuntu

RUN apt-get update

RUN mkdir -p /usr/java

COPY jdk1.8.0_112 /usr/java/jdk1.8.0_112

ENV JAVA_HOME=/usr/java/jdk1.8.0_112

ENV PATH=$JAVA_HOME/bin:$PATH
```
And save this file as `Dockerfile`. 

## Understanding the Dockerfile

Let's us understand what we are actually doing here :

* `FROM ubuntu` - we are saying that our image should be built on top of [Ubuntu](https://hub.docker.com/_/ubuntu/) image.
* `RUN apt-get update` - perform updates on the Ubuntu container.
* `RUN mkdir -p /usr/java` - create a directory. Also create the parent directory if they don't exist.
* `COPY jdk1.8.0_112 /usr/java/jdk1.8.0_112` - copy the contents of directory from docker host to container.
* `ENV JAVA_HOME=/usr/java/jdk1.8.0_112` - set the `JAVA_HOME` environment variable.
* `ENV PATH=$JAVA_HOME/bin:$PATH` - add the environment variable to path.

## Building an image

To create an image we need to have a Dockerfile. And we have created one in previous steps. Now issue the following command to create an image : 

```bash
docker build -t $DOCKER_ID/jdk
```

This will create an image named $DOCKER_ID/jdk. $DOCKER_ID is our Docker Id stored as environment variable.

We can call the `docker images` command to check the image, that our image is added to the Docker.

## Running the Oracle JDK Container

Issue the following the commands to run the image :

```bash
docker container run @DOCKER_ID/jdk \
--detach
```

This will run our jdk container in detached mode.

