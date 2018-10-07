---
layout: post
title:  "Sprint Boot Without Parent POM"
date:   2018-10-07 00:00:00 +0530
categories: java spring
author: "abhishek shah"
tags: microservice
---


In the previous article - [Sprint Boot Starters](https://abhishek-shah.org/java/spring/2018/10/06/spring-boot-starter.html), we set up Spring Boot project 
and we saw we had to specify Maven Parent POM to `org.springframework.boot` `groupId `, but what if we want a different parent POM? 

I work for a client where they have this, a parent POM which must be inherited in our every project. 

We can still create a Sprint Boot application by using the `scope=import` of the dependency management. 

In our POM, add the following lines in `DependencyManagement`:

```xml 
<dependencyManagement>
		<dependencies>
			<dependency>
				<!-- Import dependency management from Spring Boot -->
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-dependencies</artifactId>
				<version>2.0.5.RELEASE</version>
				<type>pom</type>
				<scope>import</scope>
			</dependency>
		</dependencies>
</dependencyManagement>
```

## Dependencies 

We will keep the dependencies simple as of now: 

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

## We created a Spring Boot Application without inheriting the Parent POM

Finally our POM should look like this:

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>

	<groupId>org.ashah.springboot</groupId>
	<artifactId>sprintbootwithoutparentpom</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<packaging>jar</packaging>

	<name>sprintbootwithoutparentpom</name>
	<url>http://maven.apache.org</url>

	<properties>
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
	</properties>

	<dependencyManagement>
		<dependencies>
			<dependency>
				<!-- Import dependency management from Spring Boot -->
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-dependencies</artifactId>
				<version>2.0.5.RELEASE</version>
				<type>pom</type>
				<scope>import</scope>
			</dependency>
		</dependencies>
	</dependencyManagement>

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

	<build>
		<plugins>

			<plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-compiler-plugin</artifactId>
				<configuration>
					<source>1.8</source>
					<target>1.8</target>
				</configuration>
			</plugin>
		</plugins>
	</build>

</project>
```
