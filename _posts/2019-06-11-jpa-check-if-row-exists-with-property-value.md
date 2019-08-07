---
layout: single
title:  "How to check if a row with a specific column value exists in Spring Data"
date:   2019-06-11 19:16:00 +0100
excerpt: "The big old question: looking for rows in a database where a column contains a specific value"
categories: [tech]
tags: [Spring Boot, Spring Data]
published: true
---
Spring Data has a great declarative way of defining queries, called derived queries. Suppose you have a Person class with an email field. Then you can define the following method in a
Repository interface:

```java
public interface PersonRepository extends JpaRepository<Person,Long>{
  List<Person> findByEmail(String email);
}
```
Spring derives the query based on the method name, in this case a query to retrieve all persons with a given value for the email field.

As you can see, the method returns a ``List``. If you want to process all Persons, that is ok. But sometimes you want to know 'does a row with this particular value exist?'.

One way to do this is as follows:
```java
if(!personRepository.findByEmail("test@mail.com").isEmpty()){
  //do something if a person with the test email already exists in the repository
}
```
Personally, the additional call to the ``isEmpty()`` method is a bit too much for me. This can be solved as follows:
```java
public interface PersonRepository extends JpaRepository<Person,Long>{
  @Query("SELECT CASE WHEN COUNT(c) > 0 THEN true ELSE false END FROM Person p WHERE p.email = :email")
    boolean existsByEmail(@Param("email") String email);
}
```
Now the method does return a boolean, but then again we now have this ugly case in the query. Is there a better way?

Yes, there is - but it is very well hidden in [one of the appendices of the spring data docs](https://docs.spring.io/spring-data/jpa/docs/current/reference/html/#repository-query-return-types). A query can in fact return a ``boolean``. As always,
there is a disclaimer stating 

> consult the store-specific documentation for the exact list of supported return types, because some types listed here might not be supported in a particular store.

So we can just update the very first code snippet to:

```java
public interface PersonRepository extends JpaRepository<Person,Long>{
  boolean findByEmail(String email); //now returns a boolean instead of a List<Person>
}
```