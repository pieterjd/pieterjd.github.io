---
layout: single
title:  "Version endpoint in Spring"
date:   2019-01-15 19:24:21 +0100
excerpt: "Version endpoints are always helpful for quickchecks, but how can you build your custom one?"
categories: [tech]
tags: [Spring]
published: false
---


Maven file:

```xml
<build>
   <plugins>
      <plugin>
         <groupId>org.springframework.boot</groupId>
         <artifactId>spring-boot-maven-plugin</artifactId>
         <executions>
            <execution>
               <goals>
                  <goal>build-info</goal>
               </goals>
               <configuration>
                  <additionalProperties>
                     <java.source>${maven.compiler.source}</java.source>
                     <java.version>${java.version}</java.version>
                     <application.name>${project.name}</application.name>
                     <application.description>${project.description}</application.description>
                  </additionalProperties>
               </configuration>
            </execution>
         </executions>
      </plugin>
   </plugins>
</build>
```

restendpoint:

```java
package com.example.edev.ott.ta.log;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.info.BuildProperties;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;

import java.io.IOException;
import java.util.Optional;
import java.util.Properties;

@RestController
@RequestMapping("/version")
public class BuildInfoController {
    @Autowired
    private BuildProperties buildProperties;

    @GetMapping
    public Properties getInfo() {
        Properties prop = new Properties();
        //need to explicitly loop over all entries, just returning the BuildProperties object only contains the specific fields (artificact, group, name, time and version)
        buildProperties.forEach(entry -> prop.put(entry.getKey(),entry.getValue()));
        //proper date formatting for time
        prop.put("time", Instant.ofEpochMilli(Long.parseLong(prop.getProperty("time"))).toString());
        return prop;
    }
}
```

curl examples:

c:\_dev\git>curl localhost:8080/version

```json
{"name":"ta","java.version":"1.8","time":"2019-02-05T13:13:48.282Z","application.description":"Demo project for Spring Boot","application.name":"ta","version":"0.0.1-SNAPSHOT","java.source":"1.8","group":"com.example.edev.ott","artifact":"ta"}
```

