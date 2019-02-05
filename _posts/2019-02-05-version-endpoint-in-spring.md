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
    public Properties getInfo(@RequestParam(required = false) Optional<Boolean> full) throws IOException {
        Properties props = new Properties();
        if(full.orElse(false)){
            props.load(getClass().getResourceAsStream("/META-INF/build-info.properties"));
        }
        else{
            buildProperties.forEach(
                    entry -> props.put(entry.getKey(),entry.getValue())
            );
        }
        return props;
    }
}
```

curl examples:

c:\_dev\git>curl localhost:8080/version

```json
{"name":"ta","java.version":"1.8","time":"1549363706256","application.description":"Demo project for Spring Boot","application.name":"ta","version":"0.0.1-SNAPSHOT","java.source":"
1.8","group":"com.example.edev.ott","artifact":"ta"}
```

c:\_dev\git>curl localhost:8080/version?full=true

```json
{"build.artifact":"ta","build.name":"ta","build.version":"0.0.1-SNAPSHOT","build.time":"2019-02-05T10:48:26.256Z","build.group":"com.example.edev.ott","build.application.descriptio
n":"Demo project for Spring Boot","build.java.version":"1.8","build.application.name":"ta","build.java.source":"1.8"}
```

