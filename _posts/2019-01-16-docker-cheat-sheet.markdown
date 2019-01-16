---
layout: post
title:  "Docker Cheat Sheet - Starter"
date:   2019-01-16 00:00:00 +0530
categories: docker
author: "abhishek shah"
tags: docker
---

Useful Docker commands, for beginner level.

## List Docker CLI commands
```bash
docker
docker container --help
```

## Display Docker version and info
```bash
docker --version
docker version
docker info
```

## Execute Docker image
```bash
docker run hello-world
```

## List Docker images
```bash
docker image ls
```

## List Docker containers (running, all, all in quiet mode)
```bash
docker container ls
docker container ls --all
docker container ls -aq
```
