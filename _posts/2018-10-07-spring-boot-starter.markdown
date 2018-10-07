---
layout: post
title:  "Sprint Boot Starter"
date:   2018-10-07 00:00:00 +0530
categories: java spring
author: "abhishek shah"
tags: microservice
---

In this short guide, I'll be exploring how to **setup a Spring Boot project**. I will be using  *Maven* as my dependency management tool. Spring Boot 
says in their official site that they're compatible with Apache Maven 3.2 or above. So make sure if you're following this, you've the right version of Maven installed.

## Project POM

In `pom.xml` I added the following lines:

```xml
<!-- Inherit defaults from Spring Boot -->
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.0.5.RELEASE</version>
	</parent>
```

Spring Boot dependencies uses `org.springframework.boot` `groupId`. Maven POM inherits from parent POM `spring-boot-starter-parent`. 

## Dependencies

Dependencies are added using one of the **Spring Boot Starter** project. In Maven POM following dependencies are added: 

```xml
<dependencies>
		<!-- Add typical dependencies for a web application -->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>

		<dependency>
			<groupId>junit</groupId>
			<artifactId>junit</artifactId>
			<scope>test</scope>
		</dependency>
</dependencies>
```

So, I have added `spring-boot-starter-web` starters as my dependency.  All the important jars are added as Maven depdencies. And the best part is that we
don't have to manage the versions by ourself. Spring Boot does this behind the scenes (Spring Boot maintains a BOM).

## Running the application

Spring Boot application is no different than a normal Java Maven project. I did the following to run:
* Built the jar - `mvn clean package`, and then run the executable jar. (build executable jar using a  Spring Boot Maven plugin)

`Hello World!` will greet us in the console output.

## Spring Boot Maven plugin 

Okay, to actually create an executable jar we usually have to do a little, extra work, again, Spring Boot does all this for us.
Just use Spring Boot Maven plugin. Add the following lines in Maven POM:

 ```xml
 <!-- Package as an executable jar -->
	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
		</plugins>
	</build>
  ```
  
## Overview

In this short article we created a Spring Boot application with all the necessary dependencies added to them. Added Spring Boot Maven plugin 
to create executable jar.

At the end, our Maven POM should look like this:

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>

	<groupId>org.ashah.springboot</groupId>
	<artifactId>sprintbootstarters</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<packaging>jar</packaging>

	<name>sprintbootstarters</name>
	<url>http://maven.apache.org</url>

	<properties>
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
	</properties>

	<!-- Inherit defaults from Spring Boot -->
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.0.5.RELEASE</version>
	</parent>



	<dependencies>

		<!-- Add typical dependencies for a web application -->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>


		<dependency>
			<groupId>junit</groupId>
			<artifactId>junit</artifactId>
			<scope>test</scope>
		</dependency>

	</dependencies>

	<!-- Package as an executable jar -->
	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
		</plugins>
	</build>

</project>
```
