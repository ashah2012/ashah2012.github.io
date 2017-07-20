---
layout: post
title:  "Why you should learn Docker?"
date:   2017-07-18 00:00:00 +0530
categories: devops
author: "abhishek shah"
---

[Docker](https://www.docker.com/) is a sensation now. It was little difficult for me six months ago to understand why Docker was created? Isn't Docker the 20th solution to the previous 19th solution for software shipment? VMs already existed to promote our code through different environments through testing and finally in the production environment. It would not be fair to point out the advantages of Docker in a single article. I'll give it a due share of credit in articles to come, and honestly, the more I've clear and concise advantages of it.

## So What is Docker?
Docker is something that has become the standard for developers and sys admins for packing, shipping and deploying distributed applications. The developers create a template called **images** and Docker creates a VM from that image called **container**.

Image contains instruction for creating a new image entirely or from an existing image. One fine and simple example will be - suppose John likes to work on Ubuntu, but doesn't like working on VIM. So he creates a new image of Ubuntu by removing the VIM package from it.

John can also share this new image with the world, via [Docker Hub](https://hub.docker.com/). He just needs to upload the new image to the Docker repository which is similar to Git Push. One great added advantage here is, that uploading the new image will not much of your time, since the original image is already with Docker repository, it will only upload the differences and Docker will manage on its own. It's like - Docker Hub says to John, "Just push the differences, and we'll create a new image from our old image by looking at difference table". But, I may have over said thing there, when we push the image, we don't manually send the difference, that is handled by Docker.  

Docker makes it easier for the teams to automate infrastructure, isolate code. And maintain consistency.

Docker can be easily installed in Linux, Mac and Windows. Head over to Docker site and read the instructions carefully for installing it. Next I'll discuss about the installation in Linux.

## Installing Docker in Linux!
Getting late for office - I'll continue in the evening. Let's push this !
