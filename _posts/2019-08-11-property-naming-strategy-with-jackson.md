---
layout: single
title:  "Custom property naming strategy with Jackson"
date:   2019-08-11 14:41:12 +0100
excerpt: "What if the REST API you are consuming doesn't use lower camelcase?"
categories: [tech]
tags: [Spring Boot]
---

For a pet project of mine, I'm consuming a REST API. Unfortunately it's probably not written with Spring (Boot), as the body of requests and response are not according to what you're used to, UpperCamelCase instead of lowerCamelCase.

```json
{"SessionKey":"7d38ec29-d1f3-42e9-b35f-91b505bf3206",
"Status":"UpdatesComplete"
}
```

At first, I thought I could solve it with a ``@JsonProperty("...")`` annotation on every field, but there are so many of them :o

This can easily be fixed with the property naming class annotation:

```java
@JsonNaming(PropertyNamingStrategy.UpperCamelCaseStrategy.class)
public class SomeClassName {}
```
