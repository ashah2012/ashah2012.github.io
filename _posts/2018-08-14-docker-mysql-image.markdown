---
layout: post
title:  "Running MYSQL in Docker Container"
date:   2018-08-14 00:00:00 +0530
categories: docker
author: "abhishek shah"
tags: docker
---


In this short article, we'll look at running MYSQL in docker container. The official MYSQL image can be found here at [Docker Hub](https://hub.docker.com/_/mysql/).

## Downloading and Running the Docker MYSQL Docker image

Open up the terminal and run the following command : 

```bash
docker run -d \
--name=test-mysql \
--env="MYSQL_ROOT_PASSWORD=root123" \
-p 6306:3306 \
-v /Documents/ashah/data/mysql: /var/lib/mysql \
mysql:latest
 ```
 If we don't have a local MYSQL image in our Docker host then Docker will download it from Docker Repository. 
 
## Understanding the Parameters
 
 * `-d` - runs the image in detached mode as background service.
 * `--name` - name of the container.
 * `-env` - sets the password as Environment variable.
 * `-p 6306:3306` - expose the port on which mysql is running, i.e, 3306 to host port 6306.
 * `-v /Documents/ashah/data/mysql: /var/lib/mysql` - maps the local volume on docker host to container volume at /var/lib/mysql.
 * `mysql:latest` - directing Docker to download the latest tag of the MYSQL image.
 
## That's it! 
 
That is all that is required to run a MYSQL database in the Docker host. We can check whether our container is running or not. To do this issue the command :

```bash
docker ps
```
It will print the names of the container running on our docker host. We can verify the name of our container in the printed list.

## Working on MYSQL

Since we started our container in detached mode we need to connect to it to start executing our sql commands on it. Copy the container id of our container by executing `docker ps` from the printed list.

Now to connect to this container execute the following command :

```bash
docker exec -it 0499d911f5c6 bash
```


