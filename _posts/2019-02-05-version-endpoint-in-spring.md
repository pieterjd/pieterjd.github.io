---
layout: single
title:  "Version endpoint in Spring Boot"
date:   2019-02-05 22:48:21 +0100
excerpt: "Version endpoints are always helpful for quickchecks, but how can you build your custom one with Spring Boot?"
categories: [tech]
tags: [Spring Boot]
published: true
---

When generating your Spring Boot project using [Spring Initializr](https://start.spring.io/), you already have the ``spring-boot-maven-plugin`` plugin in your Maven ``pom.xml``file.

Generating build info is done by adding the ``build-info`` goal. This results in a ``/META-INF/build-info.properties`` file in your resulting package. By default this contains group id, artifact, buildtime and version of the project. 

Off course you can add additional properties in the ``additionalProperties`` element. Just like you define properties in any Maven section.

Below is the relevant snippet from a Spring Boot application. It contains

* the ``spring-boot-maven-plugin``definition
* the ``build-info``goal
* additional properties such as application name and application description. You can add as many as you like

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

Next part is to read this properties file. Thank god for [``BuildProperties``](https://docs.spring.io/spring-boot/docs/current/api/org/springframework/boot/info/BuildProperties.html) . Spring Boot also creates a Bean when starting an application, so you can just auto wire it to a field.

Only downside is that this class is not related to the default ``java.util.Properties``class. So you have to create an intermediate ``Properties``object and copy all (key,value) pairs from the buildProperties to the property object you want to return. This is required as the ``BuildProperties``class does not define getters for custom properties

The ``time``property had to be transformed to a String as the default ``Instant``does not map properly to json.

If you put all things together, you get the following controller.

```java
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

Now you can run the application and do a request for ``localhost:8080/version``, and this results in the following json:

```json
{  
   "name":"ta",
   "java.version":"1.8",
   "time":"2019-02-05T13:13:48.282Z",
   "application.description":"Demo project for Spring Boot",
   "application.name":"ta",
   "version":"0.0.1-SNAPSHOT",
   "java.source":"1.8",
   "group":"com.example.edev.ott",
   "artifact":"ta"
}
```

## Summary

To conclude: only two steps required for a version endpoint:

1. configure Maven and add your custom properties if required
2. create a RestController, auto-wire BuildProperties and transform it to ``java.util.properties`` , ``Map`` or whatever you want