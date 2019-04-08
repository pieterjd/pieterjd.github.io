---
layout: single
title:  "Stream examples Part 3 - conclusion"
date:   2019-04-08 20:14:53 +0100
excerpt: "Streams are new since Java 8, but still a bit unknown. This is a handy overview with plenty of examples"
categories: [tech]
tags: [Java, Streams]
---
I would like to wrap up with some snippets I use a lot!

# Collect to a map
When you want to collect to a Map, you need a function for the key and a function for the value.

Suppose you have a list of Person objects with a firstname and last name. If you want to map the firstname to the lastname, you would write something like this:

```java
Map<String, String> firstToLastMap = persons.stream()
.toMap(
 p->p.getFirstname(), //key mapper function
 p->p.getLastname()   // value mapper function
);
```

# Grouping
Suppose you have a List of strings. Per possible length, you want to know which Strings have
this particular length. Then you would group all Strings in the list by their length.

```java
List<String> strings = Arrays.asList("one","two","three","four","five","six");
Map<Integer, List<String>> counts = strings.stream().collect(Collectors.groupingBy(s->s.length()));
//output: {3=[one, two, six], 4=[four, five], 5=[three]}
```

# Sum and Average

You can also easily do different kinds of count, for instance sum (summingInt) and average (averageInt). In this example, the sum and average of string lengths are computed.

```java
List<String> strings = Arrays.asList("one","two","three","four","five","six");
System.out.println(strings.stream().collect(Collectors.summingInt(s->s.length())));
//output: 22
System.out.println(strings.stream().collect(Collectors.averagingInt(s->s.length())));
//output: 3.6666666666666665 -> this equals 22/6
```
If you want all the statistics (min, max, sum, count, average) in one go, then check out
[``summarizingInt``](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collectors.html#summarizingInt-java.util.function.ToIntFunction-)

# Partitioning
Partioning is also quite easy - in this case you are limited to a bi-partition, based on a predicate that is either ``true`` or ``false``. In this example, String with length bigger than 3 are considered long Strings.

```java
List<String> strings = Arrays.asList("one","two","three","four","five","six");
System.out.println(strings.stream().collect(Collectors.partitioningBy(s->s.length()>3)));
//{false=[one, two, six], true=[three, four, five]}

```
