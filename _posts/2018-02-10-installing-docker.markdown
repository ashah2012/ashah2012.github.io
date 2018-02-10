---
layout: post
title:  "Installing Docker in Linux Ubuntu"
date:   2018-02-10 00:00:00 +0530
categories: docker
author: "abhishek shah"
---

## 1. Overview

In this short guide, we will look at the steps required to install *Docker in Ubuntu*. *Docker* is not available from the package repository. We will have to add a new package repository and install *Docker* from there.

## 2. Prerequites

There are two versions of *Docker* available :

* Docker CE - which is Community edition
* Docker EE - which is Enterprise edition

As we all know, EE version will have few extras feature which is must when we go to production with *containers*. And CE is just enough to get started for development.

### 2.1. OS Requirements

To install Docker CE, we need 64-bit versions of these Ubuntu :

* Artful 17.10 (Docker CE 17.11 Edge and higher only)
* Xenial 16.04 (LTS)
* Trusty 14.04 (LTS)

### 2.2. Remove old versions of Docker

Earlier versions of Docker were known as `docker` or `docker-engine`, and we will remove them, if installed.

```
$ sudo apt-get remove docker docker-engine docker.io
```

## 3. Set up Docker repository

There are other ways to install Docker, but for the brevity of the article we will stick to the most simplest way :

* Adding a docker repository and installing packages from there.

### 3.1. Update the `apt` package index

```
$ sudo apt-get update
```

### 3.2. Install packages to allow `apt` to use a repository over HTTPS

```
$ sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    software-properties-common
```

Verify that you now have the key with the fingerprint :

```
$ sudo apt-key fingerprint 0EBFCD88
```

### 3.3. Add Docker’s official GPG key

```
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

### 3.4. Set up a stable repository

```
$ sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
```

 The prerequites have been set up. Now, we will install Docker.

## 4. Install Docker CE

Since we have updated the package index, let us update the `apt` package index :

```
$ sudo apt-get update
```

Now, finally we will be able to install *Docker CE* :

```
$ sudo apt-get install docker-ce
```

The above command will install the latest `stable` version of docker ce in our system. It will also replace an existing installation of docker.

## 5. Testing the installation

To verifiy *Docker CE* is installed correctly, we will run the `hello-world` image in the container :

```
$ sudo docker run hello-world
```

It's likely that we don't have the `hello-world` image already, docker will download the test image and run the image in container.
There will be an informational message printed on the terminal about docker.

## 6. Conclusion

Installing *Docker CE* is fairly simple in Ubuntu. All we did was :

* Update the `apt` package index
* Add Docker GPG key
* Install from repository
