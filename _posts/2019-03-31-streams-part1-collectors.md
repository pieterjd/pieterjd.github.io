---
layout: single
title:  "Stream examples Part 1 - Collectors"
date:   2019-03-30 12:34:19 +0100
excerpt: "Streams are new since Java 8, but still a bit unknown. This is a handy overview with plenty of examples."
categories: [tech]
tags: [Java, Streams]
published: true
---
Streams are quite fun - in short: it's a different approach to writing loops. Suppose you have a List of Strings and you want to filter out the String starting with an 's' and put them in a new List.

```java
List<String> sStrings = new ArrayList<>();
for(int i = 0; i < strings.size(); i++){
    if(strings.get(i).startsWith('s')){
      //do something with strings starting with s
      sStrings.add(strings.get(i));
    }
}
```
Although still readable, there is a lot of redundant code: the for loop itself with the counter and condition, the if statement. With streams, it looks more readable.

```java
List<String> sStrings = strings.stream()
  .filter(s -> s.startsWith('s'))
  collect(Collectors.toList());
```

Inside the filter method, you have a function - this has the same meaning as in mathematics: you have some inputs, the functions does it magic and something different comes out. In this case, the function takes a String and outputs true when the String starts with an 's'.

This filter generates a stream on its own, in this case a stream of Strings all starting with an 's'. You can collect the results, in this case a List.

Besides filter you also have map, suppose you want the length of all Strings starting with an 's'. You can just add a map after the filter statement. map also takes a function, in this case a String as input, and it outputs a number, the length of the strings.

```java
List<Integer> stringLengths = strings.stream()
  .filter(s -> s.startsWith('s'))
  .map(s -> s.length())
  .map(i -> new Integer(i))
  collect(Collectors.toList());
```
