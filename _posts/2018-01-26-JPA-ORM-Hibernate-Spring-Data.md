---
layout: post
title: JPA, ORM, Hibernate and Spring Data
key: 10005
tags: Java database
category: blog
---

Make clear the difference and relationship among JPA, ORM, Hibernate, Spring Data.<!--more-->

## ORM (Object-relational mapping)

Object-relational mapping in computer science is a programming technique for converting data between incompatible type systems using object-oriented programming languages

From relational model to Object.

Example: Entity EJB, Hibernate, iBATIS(MyBatis), TopLink

## JPA(Java persistent API)

The Java Persistence API provides a POJO persistence model for object-relational mapping. 

A general form for different ORM framework.

## Hibernate

Hibernate ORM is an object-relational mapping tool for the Java programming language. It provides a framework for mapping an object-oriented domain model to a relational database

Hibernate implements JPA.

## Spring Data

Spring Data’s mission is to provide a familiar and consistent, Spring-based programming model for data access while still retaining the special traits of the underlying data store. 

Data access abstraction 

### Spring Data JPA

Spring Data JPA, part of the larger [Spring Data](https://projects.spring.io/spring-data) family, makes it easy to easily implement JPA based repositories. This module deals with enhanced support for JPA based data access layers. It makes it easier to build Spring-powered applications that use data access technologies.

### Spring Data REST

Spring Data REST is part of the umbrella [Spring Data](https://projects.spring.io/spring-data-rest/#) project and makes it easy to build hypermedia-driven REST web services on top of Spring Data repositories.

For simple service. No validation, just CRUD operations.