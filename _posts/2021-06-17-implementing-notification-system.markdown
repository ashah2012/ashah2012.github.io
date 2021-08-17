---
layout: post
title:  "Implementing a simple notifications for our application"
date:   2021-06-17 00:00:00 +0530
categories: java
author: "abhishek shah"
tags: java
---

In this article I am going to talk about how I implemented a 
simple notification system for the frontend. 

The basic idea is we fire an event whenever we want to update the user about a
change. 

We create specific events class by extending ```ApplicationEvent``` class. A listener
by implementing ```ApplicationListener<?>```. 

Inside the Listener class we insert an entry into the database for our notification
table. We can also use the same opportunity here to fire emails, sms notifications as well. 

The notification table in its bare minimum, should contain few things like the notification text, expired, 
intended_user. We can create a ```Rest Resource Controller```
to expose basic crud operations via the APIs.

The Frontend UI should call the ```GET APIs``` to fetch the notifications for a particular user. 

This simple process can help us with a simple notification system and get the job done. 

There are many ways which we can use for live notifications, but we wanted things to be simple. There is
a major drawback with this approach, i.e, the frontend has to poll the APIs to fetch the notifications on regular 
interval.

