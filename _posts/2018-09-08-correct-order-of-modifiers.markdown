---
layout: post
title:  "Correct Order of Modifiers in Java"
date:   2018-09-08 00:00:00 +0530
categories: java
author: "abhishek shah"
tags: java
---

Have we ever wondered if there's any language specification for writing the modifiers in [Java](https://www.java.com/en/) language?

I was recently reviewing the code I wrote for my current assignment. I found one thing very peculiar. I had `logger` initiliazed in lots of my utility
classes. So times I ended up writing :

```java
private static final Logger LOGGER = Logger.getLogger();
```
 and sometimes I ended up writing :
 
 ```java 
 private final static Logger LOGGER = Logger.getLogger();
```

## The Java Language Specification 

So I checked my [SonarQube](https://www.sonarqube.org/) build report and found that the latter was wrong. [The Java Language Spefification](https://docs.oracle.com/javase/specs/)
actually recommends the modifiers to be written in the  following order in precedence :

* `Annotations`
* `public`
* `protected`
* `private`
* `abstract`
* `static`
* `final`
* `transient`
* `volatile`
* `synchronized`
* `native`
* `strictfp`

So remember them and if not turn up to [SonarQube]() report, to find any non compiliance to the language specification.
