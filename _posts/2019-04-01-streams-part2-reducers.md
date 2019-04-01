---
layout: single
title:  "Stream examples Part 2 - Reducers"
date:   2019-04-01 17:30:53 +0100
excerpt: "Streams are new since Java 8, but still a bit unknown. This is a handy overview with plenty of examples"
categories: [tech]
tags: [Java, Streams]
---
Next to collectors, Streams offer reducers as well. You can consider it as building a result, starting from a partial solution and building on top of that. It is actually comparable to how the result is built up when using recursion.

As an example, let's take the sum of all integers starting from 1 up until n. The trivial solution is 0 for the 'empty case', this is the value before you start looping. This will be the initial value or IdentityValue in stream speak. We move to the next value in the range (1). We combine our intial value and current value by taking the sum. Now we have the sum of all integers 1 up until 1. Now take 2, and add it to the partial solution that we already have, and now we have the partial solution for all integers starting from 1 up until 2.

```java
Integer sum = IntStream.range(1,n+1)
.reduce(0, (partial_solution, next) -> partial_solution + next);
```
Here we use the ``reduce`` method taking 2 arguments: the first one is the value of the 'empty case', the second is a function taking 2 inputs: the partial solution so far, and the next value of the stream you can use to extend the partial solution.

Off course this reducer is over the top as you can use the ``sum`` method on an Integer Stream.

A second example is joining a List of Strings by separating them by a comma.

```java
String commaSeparated = strings.stream()
.reduce("", (partial_solution, nextString) -> partial_solution +", " + next);
```

An alternative is the collector variant:
```java
String commaSeparated = strings.stream()
.collect(Collectors.joining(","));
```
At first, streams can appear difficult to grasp, but once you get the hang of it, you'll really enjoy it!
