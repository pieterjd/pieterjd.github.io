---
layout: single
title:  "Spring boot without a web container"
date:   2019-07-31 18:34:12 +0100
excerpt: "How to use Spring Boot without Tomcat - the default web container"
categories: [tech]
tags: [Spring Boot]
---
I really like spring boot a lot - just throw in some dependencies and **BAM** it's configured right out of the box.

For a pet project of mine, I'm consuming a REST API. So I included the ``spring-boot-starter-web`` artificact.

Not so surprisingly, the logs showed Tomcat was booting. In this particular project,
I don't need tomcat or any other web container for that matter. So how to exclude this?

My first guess was the application properties, but there was nothing to turn the web container on or off. And then I remembered:

> Whatever is found in the classpath will be configured

``spring-boot-starter-web`` depends on:
* Core spring
* Web MVC
* Jackson
* Validation
* Embedded container - tomcat
* Logging

So disabling Tomcat comes down to excluding it in the pom file:

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-web</artifactId>
  <exclusions>
      <exclusion>
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-starter-tomcat</artifactId>
      </exclusion>
  </exclusions>
</dependency>
```
