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
docker container run \
--detach \
--name mydb \
-e MYSQL_ROOT_PASSWORD=my-secret-pw \
mysql:latest
 ```
 If we don't have a local MYSQL image in our Docker host then Docker will download it from Docker Repository. 
 
## Understanding the Parameters
 
 * `--detach` - runs the image as background service.
 * `--name` - name of the container.
 * `-e` - sets the password as Environment variable.
 * `mysql:latest` - directing Docker to download the latest tag of the MYSQL image.
 
## That's it! 
 
That is all that is required to run a MYSQL database in the Docker host. We can also add `port host_port:container_port` mapping to access the MYSQL database from outside the container. 
